# gradeup.github.io
Engineering Blog


### How to Setup ?

First make sure that ruby(version > 2.0) is installed on your system. To check if ruby is installed simply type

```
ruby -v
```

Now install jekyll and github-pages gem after ruby is installed.

```
sudo gem install jekyll
sudo gem install github-pages
```

To run server, simply write

```
jekyll serve
```


### Common issues during installation

In case you run into the following error on Mac, for ruby version <2.1.0: 
ERROR:  Error installing jekyll:
liquid requires Ruby version >= 2.1.0.
Upgrade the ruby version using:

```
\curl -sSL https://get.rvm.io | bash -s stable
rvm install ruby-2.4.1
rvm use ruby-2.4.1 --default
```
