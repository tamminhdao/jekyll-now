---
layout: page
title: Sequence Diagrams
---

Last week I ran into issues with circular dependencies. To solve my problem, I had to examine my system as a whole. Design software is challenging for many reasons. One of the issues I've experienced is keeping a big picture of the system. As my software gets more complicated, I have a hard time keeping track of how things are wired together, how objects communicate with each other. I sometimes said to my mentors that I am lost in my own system, fully aware that what I am building in these exercises are very small compared to the systems I will work with in actual client projects.

I am a visual learner. Pictures speak to me more than words. So I started to map out my systems. Below is a simple map I drawn with just boxes denoting my classes and arrows connecting them.

![](/images/server.jpg){:class="img-responsive"}

This was better than nothing, but it was still messy and did not deliver much information. Another potential drawback of this way of representing the system is its heavy emphasis on objects. While these languages I am using these days are "object-oriented", I recently read that the focus of a good design should instead be on the messages, and not the objects. In her book Practical Object-Oriented Design in Ruby, Sandi Metz talks about how message is the core of object-oriented systems. Objects have methods. These methods carry messages. When we call a method, one object passes a message to another object. When a method returns something, it is "sending a message", and when a method takes a parameter, it is "receiving a message".

```
We have objects because we send messages, not the other way around.
```

In chapter 4 of Practical Object-Oriented Design in Ruby, Sandi Metz introduces her readers to sequence diagram. She uses them to quickly sketch out a design before she starts coding, as a way to experiment with objects and the messages passed between them. Sequence diagrams not only identify the objects, but also sketch out the messages passing between these objects. Here is one example from the book:

![](/images/DemoDiagram.png){:class="img-responsive"}

I like these diagrams. I have yet to use them the way Metz uses them to brainstorm design, but I have been using them to draw out my existing system, or when I read someone's code, I have experimented drawing out their system using these diagrams. It feels like having an aerial view of the system. I can see how things connect, where the busy actions take place and identify troubles (not all, but some) in my design.

![](/images/serverDiagram.jpg){:class="img-responsive"}

Useful resources on sequence diagrams from Udacity <a href="https://www.youtube.com/watch?v=6Uivxxobu0c"> here </a>, <a href="https://www.youtube.com/watch?v=XIQKt5Bs7II" here </a> and <a href="https://www.youtube.com/watch?v=Vjb86eQgHsA"> here </a>.
