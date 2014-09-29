---
layout: post
title:  "How to Write Good Code"
date:   2014-09-29 20:30:29
categories: code
---

# How to Write Good Code

## How to Write Good Code/Be a Better Programmer

-------------
There was a speaker talk about beautiful code and the gist of his argument was that beautiful code comes in a lot of different forms. There’s beauty in simplicity, beauty in compromise, beauty in stability, beauty in functionality, beauty in robustness, etc. You may not be able to find all these things in every piece of code, but from each thing you learn to recognize, you can keep more and more of it in your mind when embarking on writing new code or review works of others.

	
  * Good code is a little subjective. There are good practices that you can learn, but even with good practices, without understanding why certain things are the way they are, words like good/beautiful/bad/ugly can only be assigned on a case-by-case basis. Looking at code at Amazon would require some business context.

   * One method is to read other people's code and try to figure out which good ideas you can pull from it, what things you find ugly, analyze why certain things work well and others do not and how you could improve on them if you were in their shoes. There are lots of books out there on good practices and things to look out for or avoid (bad smells). But nothing is ever written in stone.
   
   * Sit in on code reviews with "known good" programmers (where the "known good" is either author *or* reviewer). We stress the "sit in" part. It's time consuming (don't be surprised if you don't cover much code in an hour) but when sitting in the same room looking at the same screen, both author and reviewer can learn a LOT about the use cases, standard idioms, style, test cases, design, etc. Conversations during reviews sparked by questions like "why not write that function using a ..." or "should this one method be factored into two methods?" are much more valuable than reading good code alone.
   
   * Get proficient in using the standard API/libraries.
   
   * Another place to find good Java code is the set of core libraries. (e.g. The Java Collections and other core libraries at http://openjdk.java.net/groups/core-libs/ ).
   
   * A lot of "good coding" practices are general concepts which apply to pretty much every language. For instance, "good design" and "simplicity" (*) is independent of the programming language. You can write very beautiful, well designed and readable code in Perl and very crappy code in Java. Of course there are language specific constructs which only apply to certain languages (think OO for Java / C++ / ML / SmallTalk, think Lisp for functional languages). And even then a good design can at least borrow some concepts. For instance you can think of the Unix device driver model and the VFS model as a rudimentary OO approach.
   
   * Becoming a better programmer is primarily a social activity. You'll make the best gains when talking about / reasoning about code that interests you with other smart people. Being involved in open source is a great way. Maybe this sounds silly, but I would recommend finding a very active Java IRC channel. (IRC is an old-school Internet protocol for chat, still widely in use for open source software dev and other things). You can learn a lot about best practices and tons of other useful things from programming channels. You can also meet a lot of people who go on to work in big popular companies. It's a tremendous social resource. 
   * The other 50% of becoming a great programmer is building tremendous knowledge. You don't have to *remember* things, but you have to know where to find them. If you work on a lot of diverse projects, eventually you will just have in passing heard about every major software product in that space, and probably know a bit about their APIs and design decisions. If you want to become a great Java programmer, then learn lots of other programming languages too. It helps you to think about things in new ways, and realize things you wouldn't otherwise.
   * Learn one language thoroughly. Super super well. Learn all the funny little things about it and why they were done that way. Find the language's standard document, if it has one, and read it. If you want to challenge yourself, try to understand the entire semantics of C++. If you make a habit of learning things in this way, with high precision and knowledge, you will eventually become surprised by people that (for example) claim to know a language like C++ well but don't even know the meaning of all its basic operators ;-) A lot of people don't know languages as well as they think they do (myself included). Don't let them discourage you from valuing some knowledge just because they think it isn't important. Knowledge of these last couple sorts is valuable not for its practical applications but for the sharpening of the mind that occurs when acquiring it.


## Good Reading
----
Articles


   * http://www.informationweek.com/news/showArticle.jhtml?articleID=191901844
   * http://www.cs.virginia.edu/~robins/YouAndYourResearch.html

Books

   * Beautiful Code (read online for free at Safari Books Online)
   * Implementation Patterns (This book is to help you communicate your intentions through your code. As a result, you will be writing readable code.)
   * Effective C++
   * Effective Java
   * Refactoring
   * Pragmatic Programmer
   * Code Complete
   * Debugging by Thinking
   * Designing Flexible Object Oriented Systems
   * Design Patterns
   * AntiPatterns
   * The Lion's Commentary on Unix
   * Literate Programming(This is one of forces that drives the evolution of Ruby language.)
   * The Art of Computer Programming - Donald Knuth
   * Programming Pearls


