---
layout: post
title: Warming Up The Cold Start&#58; How To Reduce Cold Startup Time Of Your Android App 
---

Have you ever had your patience tested by apps that cheekily keep flashing their logos at you for a moment too long, while you just cannot wait to order food or book a cab or pay the cab driver and rush to work? If that sounds like you, you too have been a victim of poor **cold startup time**, and unless you want your app to add to that growing pool of the digitally downtrodden, read on!

#### Cold Start? Enlighten me, please!
Cold start refers to an app starting from a scratch, that is the time taken from the moment the code is initialised till the UI is responsive to the user. It happens in cases such as your app is being launched for the first time since the device booted, or since the system killed the app. Needless to say, the lesser the cold startup time, the better the user experience.

Developer.android provides a comprehensive explanation of app startup time [here](https://developer.android.com/topic/performance/vitals/launch-time).



#### How much is too much?

For cold start, anything above **5 seconds** is considered excessive.



#### How do I track the cold start performance of my app?

To check how your Android app is performing in comparison to other apps, go to **Play Console > your app > Android Vitals > App Startup Time.**



#### And how do I get the exact number?

In Android 4.4 and higher, logcat includes an output line containing a value called **"Displayed"**. This value represents the amount of time elapsed between launching the process and finishing drawing the corresponding activity on the screen. The reported log line looks similar to the following example:

```
 ActivityManager: Displayed com.android.app/.StartupTiming: +3s534ms 
```

P.S. if you are tracking the time in Android studio, make sure you disable filters in your logcat view because it is the system server, and not the app, that serves this log.


Don't be confused if the "Displayed" line in the logcat output contains an additional field for **total** time. For example:

```
 ActivityManager: Displayed com.android.gradeup/.StartupTiming: +3s534ms (total +1m22s643ms) 
```

Here, the first time measurement is only for the activity that was first drawn and the "total" time measurement begins at the app process start, and could include another activity that was started first but did not display anything to the screen (for example a splash screen). This extra measurement is shown when there is a difference between the single activity and total startup times.



#### Enough said! How do I fix it now?

Before we delve into the code and start digging it for prospective flaws, let's first try and identify the performance bottlenecks. Android provides us with some really cool tools that provide us with an in-depth analysis of our app's behaviour.

* **Android profiler**:  The new Android Profiler window in Android Studio 3.0 and higher replaces the Android Monitor tools. It provides realtime data for your app's CPU, memory, and network activity. You can perform sample-based method tracing to time your code execution, capture heap dumps, view memory allocations, and inspect the details of network-transmitted files.

* **Traceview**: Traceview is a tool that provides a graphical representations of trace logs. You can generate the logs by instrumenting your code with the Debug class. This method of tracing is very precise because you can specify exactly where in the code you want to start and stop logging trace data.

* **Systrace**: The Systrace command allows you to collect and inspect timing information across all processes running on your device at the system level. It combines data from the Android kernel to generate an HTML report that provides an overall picture of an Android device’s system processes for a given period of time and also inspects the captured information to highlight problems that it observes, such as UI jank, and provides recommendations about how to fix them.

* **Hierarchy Viewer**: Hierarchy Viewer  allows you to measure the layout speed for each view in your layout hierarchy. It can help you find performance bottlenecks caused by the structure of your view hierarchy.

    P.S. If you're using Android Studio 3.1 or later, you should instead use **Layout Inspector** to inspect your app's view hierarchy at runtime. 

* **Debug GPU Overdraw tool**: It uses color-coding to show the number of times your app draws each pixel on the screen. The higher this count, the more likely it is that overdraw affects your app's performance.

For those of you who are unfamiliar with these tools, Udacity provides a free course on Android performance that nicely explains how to leverage these tools to your benefit. Access the course [here](https://in.udacity.com/course/android-performance--ud825).

#### Good to go. What next?

Now that you have analysed your app and zeroed in the problematic areas, let's see how we can fix them for a better user experience and healthy app vitals.



#### RESTRUCTURING THE CODE

* **Tweaking the onCreate()** - The chief reason for a poor cold-startup time is doing too much of work in the onCreate of your launcher activity. 

    * Check the onCreate of your class and move all the independent and unblocking but time-consuming methods to a separate thread.

    * Make sure you use lazy initialisations wherever you can, so that the onCreate is not bombarded by unnecessary method or class initialisations.

    * Use Local variables wherever possible.

* **Use Dagger2 for dependency injection**: Dagger is a fully static, compile-time dependency injection framework for both Java and Android. It implements the dependency injection design pattern without the burden of writing the boilerplate and aims to address many of the development and performance issues that have plagued reflection-based solutions.

    You can refer to [this](https://medium.com/@harivigneshjayapalan/dagger-2-for-android-beginners-dagger-2-part-i-f2de5564ab25) article for a detailed explanation of Dagger 2 and its usage.


* **Using Handler with delay**: Now this one might sound like a hack but it does wonders to your app startup performance- in cases where there is some piece of lethargic code which you cannot afford to shift from the main thread, you can put in in a Handler and use the postDelayed method, adding an appropriate delay(100 to 200ms should do just fine). This takes the weight off the onCreate and your app launches much quicker than before ,with unnoticeable delay in the execution of the delayed method.


    ```
    new Handler().postDelayed(() -> {
            //your code
    }, 200);
    ```

#### OPTIMISING THE UI

* **Restructure the splash screen**: Image rendering is a time consuming process and can mess up with your startup time. If you app uses a splash screen that displays your logo in an ImageView, one optimization you can do is to remove the ImageView from the xml and instead add the logo to the theme of your activity as windowBackground. Add the following line of code to your activity's theme:

    ```
    <item name="android:windowBackground">@drawable/splash_screen_background</item>
    ```

* **Flatten the view hierarchy**: A nested hierarchy comes with a huge performance overhead called Overdraw. Overdraw refers to the system's drawing a pixel on the screen multiple times in a single frame of rendering, where only the last drawn pixels are shown to the user while the others only add to the performance cost. For a better rendering performance, check your xml and flatten the view hierarchy as much as you can, removing any nested Relative or Linear layouts. Constrain layout suffices our need of creating a flat hierarchy to a large extent and gives great flexibility by providing  multiple anchoring points, vertical and horizontal guidelines, chaining and more.

* **Use View Stubs**: Views with default visibility set to "gone" add an unnecessary rendering overhead because they are drawn every time the view is rendered, but used only when the user sets their visibility to visible e.g. an activity that needs to show coach marks in the first session only will still draw the coach marks every time it is opened, setting their visibility to gone otherwise. By replacing such views with a Viewstub, we can reduce this overhead. A ViewStub is an invisible, zero-sized View that can be used to lazily inflate layout resources at runtime. It is only when a ViewStub is made visible, or when inflate() is invoked, the layout resource is inflated. 

    ```
    <ViewStub
    android:id="@+id/viewStub"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:background="#dd000000"
    />
    ```
    and then
    ```
    ViewStub coachStub = findViewById(R.id.homeCoachLayoutStub);
    View stubView = coachStub.inflate();
    ```
    whenever you need to show the view.



* **Use WebP Images**:  WebP is an image file format from Google that provides lossy compression (like JPEG) as well as transparency (like PNG) but can provide better compression than either JPEG or PNG. Using images in WebP format instead of PNG can reduce the size of on image by about 75%, resulting in less memory consumption and hence better performance!



Thats all, folks! Needless to say, there are many other factors that impact your app's start up performance like the memory allocation of your app or the efficiency of your mobile device but by employing the above mentioned technique we managed to reduce our startup time by 64%! Cheers!


