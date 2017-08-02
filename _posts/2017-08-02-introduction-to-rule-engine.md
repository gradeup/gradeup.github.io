---
layout: post
title: Introduction to Rule Engine
---

At Gradeup, we organize weekly engineering sessions, where in our whole engineering team shares their experiences, rants about some technology or having a knowledge session over some topic.

This week, the topic for our session was __Introduction to Rule Engine__ being organized by Taranjeet Singh [@taranjeet](https://github.com/taranjeet).

A brief summary of the talk is as follows

## What is Rule Engine ?

A inference system that executes a set of rules in a runtime production environment.

It executes rules, one by one in the order specified (according to priority).

It is an inference system, not a reactive system. This means that it can be used to raise an alert if too many spam posts are created within the last 15 minutes.

## Why Rule Engine ?

* Allows non-programmers to add or change business logic. This allows them to change business rules without having to ask a programmer for help.

* Allows to change business driving logic without even deploying code. This is possible because rules are fetched from database, instead of being hardcoded in the codebase.

* Gives flexibility in fine-tuning business related optimization parameters.

## What is a Rule ?

A rule is a set of condition and action. Along with these, each rules has its own priority and tag.

Priority helps in deciding the execution order of the rules. Lower the priority, earlier will be its execution.

One or more rule can be grouped together under a tag. Tagging a rule makes it easy to associate rules by business analogies.

A condition is a conditional statement(generally if), which evaluates to _true_ or _false_. A rule can have multiple conditions, which can be _ANDed_ or _ORed_.

When the result of all the condition evaluates to _True_, actions are applied. Actions are the consequences that must happen once a condition is met. The actions are set as variable in the input received to Rule Engine.

The execution logic of one rule can be summed up as

```

If
    ALL the condition of rules are met
then
    execute ALL actions

```

## Example

Consider that we want to find the sign of the number _x_. The condition will be written as

```
if( x < 0 ) sign = -1

if( x == 0 ) sign = 0

if( x > 0 ) sign = 1

```

For the above condition, we have created three rules under a tag of *NUMBER_LINE*. [Django admin](https://docs.djangoproject.com/en/1.11/ref/contrib/admin/) is used as an interface to provide __CRUD__ operations for rules.

Overview of one rule

![rule-positive-sign](/public/img/re-admin-view.png)
![rule-positive-sign-other](/public/img/re-admin-other-view.png)

## Rule Engine Simulation

Though rules gives us immense power to drive and tune our business logic, this could be disastrous if the rules are not properly checked. To prevent this, we have created a simulation interface. This interface helps us in simulating the rules by specifying and tags to rule engine.

Below we have attached screen-shot for our *NUMBER_LINE* tag when _number_ is _1_.

##### Overall View

![re-sim-overall](/public/img/re-sim-overall.png)

##### Rules wise view

Rules not applied are shown in red color, and rules applied are shown in green color.
![re-sim-applied-rules-view](/public/img/re-sim-applied-rules-view.png)
![re-sim-not-applied-rules](/public/img/re-sim-not-applied-rules.png)

## How to Use ?

To use rule engine, you need to pass an array of rules, which its loads in memory. The array looks like

```
const arrayOfRules = [
    {
    "id": 1, // unique
    "name": "Checks if number is positive",
    "external_reference": "",
    "conditionsOperator": "&&",
    "priority": 10001,
    "tags": [
        "NUMBER_LINE",
    ],
    "conditions": [
        {
            "id": 1,
            "key": "requestInfo.number",
            "operation": ">",
            "value": "0"
        }
    ],
    "actions": [
        {
            "id": 2,
            "action": "SET_NUMBER",
            "key": "actionInfo.sign",
            "value": 1
        },
        {
            "id": 3,
            "action": "RE_EXIT"
        }
    ]
}
];

```

To load these array, call `loadRules`

```
# loaded_hash is hash generated for the list of rules.
# This can be used to differentiate the set of rules.
hash = ruleEngineObj.loadRules(arrayOfRules);
```

To apply rules, call `applyRules`.

```
/*
    msg : the object which needs to be changed.
    ruleTag : single tag.
    generateMeta : (boolean) If you want to generate meta information of rule execution. Default false
*/

metaObject = ruleEngineObj.applyRules(msg, ruleTag, generateMeta);
```

You can find the source code at below links

* [Rule Engine](https://github.com/gradeup/youknowwho)

* [Rule Engine Interface](https://github.com/gradeup/youknowwho-gui)

Note: We are using a forked version of [youknowwho](https://github.com/paytm/youknowwho)
