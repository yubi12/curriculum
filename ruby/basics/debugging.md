# Debugging
Debugging is the process of finding and fixing defective code. Sometimes it'll be obvious there's a problem and you'll get an error message you can use to locate the problem. Other times the code runs, but doesn't behave as expected and you'll tear your hair out trying to work out what's going wrong.

If you want to avoid a concussion from repeatedly banging your head on the desk then make sure you know how to debug code effectively.

There are numerous aspects to effective debugging. Here, we'll cover a couple of the more common ways you can investigate problems in your code. By the end you should be able to Sherlock Holmes your way through most code problems you face.

## Learning outcomes
*Look through these now and then use them to test yourself after doing the assignment*

what the student is expected to know or be able to do by the end of this lesson

* What is the stack trace?
* How do you read through the stack trace?
* What is puts debugging?
* What are the downsides of using puts to find errors?
* What is PRY?
* How is stepping through code?
* What is Rubber Duck Debugging?
* What is the best brand of Rubber Duck to talk to?
* Does the Duck need to be yellow? I prefer green.
* I probably need some help writing these learning outcomes.

## Reading the stack trace
* What is the stack trace?

The stack trace can be viewed as simply the list of method calls the program had made to get to it's current point of execution. When your program encounters an exception it prints out the current stack to allow you to locate the point in your code that raised the error.

* How do you read the stack trace

The stack trace should always be read from top to bottom as the stack trace is printed from the most recent method call all the way back to the first method call at the bottom. By reading the top of the stack trace you'll be able to see where your code was in execution before it was stopped.

Now that you know you should start at the top of the trace, there are two other important pieces of information that you should look for in order to locate that pesky bug.

1) Because Ruby code is executed line by line through each method called you will be provided with the line number the interpreter had reached when execution stopped. This usually allows you to go straight to the source of the problem and fix it.

2) You'll be provided with the type of error that was raised. This is handy because knowing what error was raised means you'll know where you focus your attention. If you receive a ZeroDivisionError you'll know the issue is that your code is trying to devide by zero. Not all errors are that helpful but you'll definitely get pointed in the right direction. If you aren't sure what the error type means, have a look at the Ruby Docs or do a quick Google. You won't be the first person to encounter it.

To offer you a completely contrived and in no way an example of good Ruby code, we'll have a look at raising a ZeroDivisionError

```ruby
def divide(num1, num2)
  num1 / num2
end

def call_divide(num1, num2)
  divide(num1, num2)
end

call_divide(5, 0)
```
The stack trace will look something like this:
```ruby
divide.rb:2:in `/': divided by 0 (ZeroDivisionError)
        from divide.rb:2:in `divide'
        from divide.rb:6:in `call_divide'
        from divide.rb:9:in `<main>'
```
Remembering the rules for reading the stack trace we can start at the top and see it was line 2 `num1 / num2` that caused a ZeroDivisionError. From that we can deduce that one of the arguments being used in the division is 0. If you follow the stack trace down you can see divide was called in line 6 from the call_divide method, which was called from line 9 with the main method which is the top level object Ruby uses. Here you can see one of the arguments was 0. It won't always be that obvious as you won't usually directly input arguments to methods like that. It might be user input or the return value from another method call but as long as you read the stack trace you should be able to locate the source of the problem.

The curious among you may be wondering if you can access the stack trace yourself. Of course you can! You can see an example of this in the [Ruby Docs](https://ruby-doc.org/core-2.4.0/Exception.html#method-i-backtrace).

## Puts Debugging
Puts debugging is using a series of puts statements in your code when you encounter an problem in your code so that you can see the values of whatever you puts to the terminal. This may prove useful for a small issue but for a ore complicated problem using puts will be unlikely to prove productive.

To show you another contrived example and building from the first example, consider the following code.

```ruby
def divide(num1, num2)
  num1 / num1
end

def call_divide(num1, num2)
  divide(num1, num2)
end

call_divide(10, 5) #=> 1
```
You would usually expect 10 divided by 5 to equal 2, but the example above is returning 1. You might have already spotted the issue but for the purpose of showing you where using puts could prove useful stay with me.

Before I go further I want to point out now that the method `puts` returns nil after being called, so make sure you understand how this can affect your code if you use it in a method.

Getting back on track. You have some code that returns 1 instead of 2 and you've already pulled out some hair, banged your head on the desk and threatened the strangers who tried to sit at the table next to you at Starbucks. What do you do? The problem here is that no error is being raised because the code itself is valid Ruby code, so you can't consult the stack trace with a nice error message. Using your awesome powers of deduction, you know that the error is most likely being caused in the division calculation so you can start there. You can use puts in the divide method to show both the values of eahc argument as well as the result of the calculation.

The game is afoot!

```ruby
def divide(num1, num2)
  puts "The value of num1 is #{num1}" 
  puts "The value of num2 is #{num2}" 
  puts "The result of the calculation is #{num1 / num1}"
end

def call_divide(num1, num2)
  divide(num1, num2)
end

puts call_divide(10, 5)
```
The result of running this code is:
```ruby
The value of num1 is 10
The value of num2 is 5
The result of the calculation is 1

```
Using puts you can see num1 is 10 and num2 is 5 so you know the issue isn't with the argument values to the method. However you can see that the result of the calculation is 1, which is not the expected value. Looking a little closer you can see the calculation is `num1 / num1`. A rookie mistake. You obviously meant you divide num1 by num2. Using the power of puts you've apologised to Starbucks for your outburst and got yourself a Triple, Venti, Half Sweet, Non-Fat, Caramel Macchiato to celebrate.

Two words of caution however.

If you look at the result of code when using puts you can see the last line is blank, although I did use `puts call_divide(10, 5)` in the code example. This is an example of Ruby returning the value of the last line of code in a method. The last line of my Ruby code uses a puts statement which I mentioned earlier returns nil. This meant nil was returned from the divide method and when you call puts on nil it prints nothing to the screen, which explains the blank line.

The second thing to bear in mind is that littering your code with puts statement can make it even more confusing to debug. If you have complicated methods, or are uses arrays with a lot of values you can get a lot of output to the terminal which can be more rage-inducing than the original issue you were facing. You may even cause yet more bugs if you forget to remove all of the puts statements you added to the code.

## Using PRY
Pry is an alternative to the Ruby irb shell that you've hopefully used by now, although it comes with some additional features allowing you to get access to the value of variables in your code. There is a lot more to Pry than we'll cover here and I urge you to investigate it further to fully realise its potential.

As Pry is a Gem you'll need to install it. (You may remember from the installations project that Rails is also a Gem that you needed to install).

In your terminal type `gem install pry`
```ruby
Fetching: coderay-1.1.1.gem (100%)
Successfully installed coderay-1.1.1
Fetching: slop-3.6.0.gem (100%)
Successfully installed slop-3.6.0
Fetching: method_source-0.8.2.gem (100%)
Successfully installed method_source-0.8.2
Fetching: pry-0.10.4.gem (100%)
Successfully installed pry-0.10.4
4 gems installed
```

You can check it's installed properly by typing `pry -v` and you should see something like `Pry version 0.10.4 on Ruby 2.3.0`. Great!

Pry is a very powerful tool and here we're only scratching the surface to get you started. To keep things simple and in line with all my other contrived example I'll show you how you could have used pry to debug the code example used in the stack trace section if you weren't able to use the stack trace to figure it out.

Here's a reminder of the code.
```ruby
def divide(num1, num2)
  num1 / num1
end

def call_divide(num1, num2)
  divide(num1, num2)
end

call_divide(5, 0)
```
This resulted in a ZeroDivisionError.

To use pry to debug this issue you need to require it into your program, and then add the line `binding.pry` into your code on the line above where the issue is being raise. The stack trace told us it was line 2 so we can add binding.pry above that and your code should then look something like...

```ruby
require 'pry'

def divide(num1, num2)
  binding.pry
  num1 / num1
end

def call_divide(num1, num2)
  divide(num1, num2)
end

puts call_divide(5, 0)
```

Now from the terminal you run the command `ruby -r pry <filename>`. In this case my file is named divide.rb so I'd type `ruby -r pry divide.rb` and hit return. What happens next is your code is executed as normal until pry hits the line `binding.pry`. Once it reaches that line is halts execution and opens up a pry terminal which will look similiar to irb.

```ruby
From: /home/ubuntu/workspace/divide.rb @ line 4 Object#divide:

    3: def divide(num1, num2)
 => 4:   binding.pry
    5:   num1 / num1
    6: end

[1] pry(main)>
```

Here you are shown where Pry has halted execution and you are then left with a terminal for you to type commands. I know the error is being caused by a ZeroDivisionError so if I want to check the values of each argument to my divide method at that pointin the program I can simply call `num1` and `num2` in the pry terminal and it will return their values.
```ruby
[1] pry(main)> num1
=> 5
[2] pry(main)> num2
=> 0
[3] pry(main)> 
```

Here I can see the value of num2 is causing the issue and if it wasn't obvious where the value of num2 was being obtained from that's where I could focus my efforts, maybe using more pry points in my code to halt execution and explore.

I could have used the same approach for the puts debugging example by throwing in a binding.pry and checking the value of the arguments to the divide method as well as the result of dividing them to deduce that the problem must be in my calculation.

I have probably done pry a huge disservice using such simple examples but once you start writing more complex code with classes and objects it can be a really powerful tool. The reading exercise will ive you a little more insight into how it can be used.

As of Ruby 2.4 you now have access to a similar tool to pry built into ruby with `binding#irb`. It's used in a simliar way by dropping `binding.irb` into your code where you want to halt execution and an irb terminal is opened for you to explore your code. At the time of writing this Ruby 2.4 is still very new and many people may not have upgraded so pry still remains a valid and powerful tool.

## Rubber Duck Debugging
If you don't know what this is already you'll probably snort-laugh when you are told. But don't underestimate the power of the duck side of the force.

Rubber Duck Debugging is the process of explaining your code out loud to a rubber duck when you're having a problem. By having to explain it step-by-step you are likely to gain an insight into your code you missed when you were just staring at the screen. You can keep a literal rubber duck, talk to yourself out loud or find a friend and tell them what you're doing. I've seen countless people in the Odin gitter chatrooms come to explain their problem and solve it without any assistance once they have had to explain the issue they are having step-by-step.

You can't debug code you don't understand so using the rubber duck technique makes sure you yourself understand what your code is doing when it is run. By having to explain your code you'll also have to look at it line by line so if you aren't having an issue with your code that isn't raising an error but instead not functioning as intended, this technique can allow you to locate the problem as you scan each line to explain it to your duck.

To flog a dead horse and use the code example from the puts debugging example let's see how we can solve it using the rubber duck technique.

```ruby
def divide(num1, num2)
  num1 / num1
end

def call_divide(num1, num2)
  divide(num1, num2)
end

call_divide(10, 5) #=> 1
```

My program runs fine, but isn't producing the desired result. So I pick up my rubber duck and get explaining.

* My program should return the result of dividing two integers.
* I can see the two integers are 10 and 5 so I know the result should be 2
* call_divide passing those same values onto the divide method
* The divide method divides num1 with num2

HOLD UP!

I can see what my code is actually doing is dividing num1 with num1. A simple typo causing all that grief.

Again. These examples are simple but the real power of these debugging techniques will become apparent when you start writing more complex code where objects call methods from other objects and you're suddenly knee-deep in error doodoo.

## Exercises
Exercises in an external repo

## Additional Resources
*This section contains helpful links to other content. It isn't required, so consider it supplemental for if you need to dive deeper into something*

Link to no more than three additional resources to avoid this section becoming too cluttered.
