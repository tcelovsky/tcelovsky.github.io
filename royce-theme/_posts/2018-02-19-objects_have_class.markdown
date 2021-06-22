---
layout: post
title: "Objects have Class"
date: 2018-02-19 22:48:54 +0000
permalink: objects_have_class
tags: [Ruby, OOP]
---

It was mostly smooth sailing for me in my coding adventure until I hit the subject of Object Oriented Programming (“OOP”) in Ruby (so I really didn’t go very far before this first road block). I read the text of the lessons until all these getters, setters, classes and objects were all jumbled up in my head and life did not make sense anymore.

I just had to take a little break from the lessons for a few days and, instead, read some other resources online. I have to admit, at first none of them made sense either, but writing some concepts down in my own words really helped and I started to see a glimmer of light at the end of the OOP tunnel.

So what I figured out is that a Class is really just a blueprint for creating a base version of a Thing. All of the attributes of the Thing can be found within its Class and we can add more attributes by defining new methods within our Class. This means that every new Thing is “born” with a set of predefined, standard attributes. I think this is pretty neat because all you have to do is decide what you want your Things to look like and how they should behave and then every new Thing will act exactly as you expect it to. A standard name for a Thing is an Object; so all of the attributes of an Object are stored within its Class.

I was very happy once I understood that Classes are just blueprints and that really everything in Ruby is an Object. Then came instance variables. These are nifty little things; I’m a big fan of instance variables. A local variable defined within one method cannot be accessed by another method. You get around this by prefacing the variable name with @ symbol. That’s it! You’ve got yourself an instance variable that can be accessed by all methods within you Class.

Setter and getter methods were most difficult for me to understand. I just could not comprehend why you would need two methods to get a Thing to do something. [This](http://instruction.learn.co/student/video_lectures#/231) video lecture helped big time! I had to understand that I am really defining two completely different methods, one would be a name method, for example, and another would be name= (name equals) method. The name method is the getter or reader method, it simply returns information stored in the instance variable. The name= method is the setter or writer method, this is the method that can change information stored in the instance variable. Once I understood the difference between getter and setter methods things became a lot clearer!

It may have taken a while, but I finally had a decent grasp on Object Oriented Programming. It was time to apply my newfound skills and rewrite my Tic Tac Toe using OOP. Boy did that take a while! I guess applying concepts is harder than learning them, but I am done with with Tic Tac Toe now, at least until AI Tic Tac Toe. I'll let you know how that one goes!
