# Methods
Methods, also known as functions, are one of the foundational building blocks in programming.
Almost every language implements some way of making methods.

You will often find yourself writing the same code in different places in your code.
Wouldn't it be great if there was a way to reuse the same code over and over again
without having to write it all out again?

This is what methods are for. They allow you to wrap code in a name which you
can then use where you need that code to be run in your programs.

In this lesson we are going to deconstruct what methods are, their behaviour, and how they are used.

## Learning outcomes
*Look through these now and then use them to test yourself after doing the assignment*

* You understand how to use Rubys built in methods on objects
* You know how to create your own custom methods
* You know how to *call* your own methods
* You understand the difference between a explicit and implicit return in your methods
* You understand the difference between `puts` and `return`

## Rubys Built in Methods
Ruby has a ton of built in methods you can use with your programs. You have learnt
and been using a lot of them in the previous lessons to this one.

Rubys built in methods come in two different flavours:

* Methods you can use on objects such as strings and arrays. They are used on objects
  by using the dot syntax. For example `"Hello".upcase #=> "HELLO"`

* Methods you can use anywhere in your program such as `puts` and `print`.

## Creating your own Methods
You can create your own custom methods in Ruby like so:

```(ruby)
def my_name
  "Joe Smith"
end

my_name #=> "Joe Smith"
```

Lets break it down:

`def` - is a built in Ruby keyword. It lets Ruby know this is the start of a method *definition*.

`my_name` - is the name of the method. You can pretty much name it whatever you want. But there are some constraints and conventions which are described in the next section.

`"Joe Smith"` - is in the *method body*. The method body is where the logic of your method goes.This particular method will just *return* a string when its *called*

`end` - as you might have guessed `end` marks the *end* of the method definition. Its another Ruby keyword.

To call the method you simply need to use its name, as shown in the last line of the example `my_name`


## Method Scope and Arguments
Scope in programming basically means where your variables and methods are available. You have
mostly been working in the `main` scope in ruby so far which in the **global scope**.
This means your variables are available anywhere in the particular program you've been working on.

One of the important concepts about methods that you need to have a good understanding
of is that *methods create their own scope*. So variables declared outside of your methods
will not be available within them. Lets take a look at an example:

```
name = "Katrina"

def greetings
  "Hi #{name}"
end

greetings #=> undefined local variable or method `greeting' for main:Object
```

This example will cause an error `undefined local variable or method message'`
Run the example on your local machine [repl.it](https://repl.it) to see for yourself.

This works in reverse as well, variables declared within a method will not be available
outside of the method:

```
def greeting
  greeting_message = "Hey human"
end

greeting_message #=> undefined local variable or method `greeting' for main:Object
```

### Arguments to the rescue
You can pass data into your methods from the outer scope using arguments. Heres
an example:

```(ruby)
name = "Katrina"

def greetings(name)
   "Hey #{name}"
end

greetings(name) #=> "Hey Katrina"
```

As you can see from the example you pass arguments into a method when you call it by putting the argument after the method name. The parenthesis around the argument are optional, you could also write it this way

`greetings name`

Using parenthesis looks cleaner and makes your code easier to read though.

You can pass as many arguments as you like into a method:

```(ruby)
def greeting(first_name, surname, location)
 "Welcome #{first_name} #{surname} from #{location}"
end

greeting("Katrina", "Mc Laughlin", "London") #=> "Welcome Katrina Mc Laughlin from London"
```  

## What Methods Return
* explicit and implicit returns

## Chaining Methods
* how it works

## Best practices
* naming them
* not too many parameters
* bang methods
* predicate methods

## Assignment
1. To get a good introduction to all the different concepts related to methods read [this chapter about methods](https://launchschool.com/books/ruby/read/methods) from Launch Schools Introduction to Programming with Ruby book. Make sure to do the exercises at the end of the chapter too.
2. For a deeper look at methods read [this chapter](http://ruby.bastardsbook.com/chapters/methods/) from the Bastards book of Ruby. Again try to complete the exercises throughout the chapter.  


## Exercises
This will link to an external repo which will include exercises and tests

## Further Reading Resources
*This section contains helpful links to other content. It isn't required, so consider it supplemental for if you need to dive deeper into something*

Link to no more than three additional resources to avoid this section becoming too cluttered.
