---
layout: post
title: Let Me Count The Words
subtitle: Self-Actualization through Arrays
feature-img: "img/counter.png"
---

One of the new things I had to think about when I began maintaining a blog is word count. The conventional wisdom seemed to be that you should not exceed 500 words in a single post, as anything more is not easily digested by the reader in one sitting. Thus, the question became 'How many words did I type?'

No doubt there are any number of ways I could have answered this question. I could have used a word processor with a built in word-count function instead of the code-focused text editor I currently use, for example. Or I could have searched the web for a site with a blank field where you can paste your text and get back a number. (I have not looked, but I have zero doubt that such a site exists)

Instead (and perhaps in part out of procrastination from finishing the blog post I was supposed to be writing), I chose to solve the problem myself. I wrote a command-line application in Ruby that takes either a string or a text file and returns the total number of words.

The app revolves mainly around the line {% highlight ruby %} puts text.split.length {%endhighlight%} where the text is broken at each instance of white space and stored in an array (which for any non-programmers is much like a box with separate slots for items). The length of the array (number of items in the box) is then returned as the only output to the command-line. The rest of the program is devoted to determining the type of input and raising any errors that might occur if the program is called with improper syntax.

This might seem like an overly complicated solution to an already easily solvable problem, and it is. But in this case, the solution wasn't the only thing that mattered. This was the first time in my life that I had come across a need that I was able to satisfy by building something from scratch. It is difficult to put into words just how empowering an experience that can be. Is my little program the best, most elegant solution to a problem as old as print? Undoubtedly not. But it's mine.
