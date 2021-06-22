---
layout: post
title: "All the Ruby Modules"
date: 2019-08-05 22:28:55 -0400
permalink: all_the_ruby_modules
tags: [Ruby]
---

_The goal of this post is to provide a brief introduction to Ruby modules and to show the distinction between Ruby modules and Ruby classes._

Ruby modules are quite similar to Ruby classes, with the main difference being that modules, unlike classes, don’t have instances. We start our definition of a module with a `module` keyword:

```
module MyModule
 def say_hello
  puts “Hello, World!”
 end
end
```

This module can then get included in a class, giving instances of the class the ability to call the instance methods defined in the module:

```
class MyClass
 include MyModule
end

c = MyClass.new
c.say_hello
```

Note the second line of the above code snipped, this is how `MyModule` module gets included in `MyClass` class. The result of the above will be `“Hello, World!”` output because module `MyModule` was included in class `MyClass`, which resulted in `MyClass` class getting access to the instance methods within `MyModule` module.

It is important to note the difference between inheriting from a class and including a module. Ruby classes can inherit from only one class at a time, but multiple Ruby modules can be included in any single class. Note the difference below between class inheritance (first line) vs. model inclusion (second and third lines):

```
class MyClass < MySuperClass
 include MyModule1
 include MyModule2
end
```

Additionally, multiple Ruby classes can include the same module, thus, modules provide the ability to share the same instance methods between multiple Ruby classes. This ability comes in handy when you are writing a program where multiple classes share behaviors.

It is generally best practice to use a noun for your class name, while using an adjective for your module name. This general guidance is not followed in the above examples, mainly for ease of understanding and presentation.

Considering that multiple modules can be included in any single class, it is not impossible that we can encounter two different methods with the same definition. What’s an Object to do in a case like that? When a message, for example a method definition, is passed to an Object, the Object will look for that method in the following order:

1. Object’s class
2. Modules included in Object’s class, in reverse order of inclusion
3. The class’s superclass
4. Modules included into the superclass, in reverse order of inclusion

The very first method that the Object encounters that matches the message passed to the Object will be executed. This means that if method `my_method` is defined within the Object’s class, that method will be executed, even if there is another method `my_method` defined in a module included in the Object’s class. If the same method is defined twice in the same class, then the second definition (the one defined later) will take precedence. Same rule applies to modules. Keep in mind that modules included in Object’s class are searched in reverse order of inclusion, thus the model that is included later will be searched first. Fun fact, including the same module twice to influence the order in which modules are searched does not actually do anything. This means that including `MyModule1` in line 4 below will not force Object to search it first. Object will still search `MyModule2` first and then move on to `MyModule1`.

```
class MyClass < MySuperClass
 include MyModule1
 include MyModule2
 include MyModule1
end
```

How should one decide when to include a module vs. inherit a class? There is no one rule for this, but it may be helpful to keep the following in mind:

- Modules don’t have instances. Thus, it is generally better to set up classes for entities or things and then use modules to encapsulate the behavior of these entities or things. This is also why class names generally tend to be nouns, while module names tend to be adjectives.
- A class can inherit from only one superclass, but multiple modules can be included in a class. If your class will have several characteristics, it may be better to put those in separate modules, since your class would not be able to inherit from multiple superclasses.
