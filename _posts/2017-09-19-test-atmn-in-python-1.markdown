---
layout: post
title:  "Test Automation in Python I - Prologue"
date:   2017-09-19 19:30:23
categories: generic
tags: 
image: /assets/article_images/2017-09-19-test-atmn-in-python-1/time.jpg
---

I have submitted a proposal to talk at several software test conferences taking place in 2018 where I hope to share our implementation of test automation we have taken at FanDuel (which are really just checks, not the automation of testing). I hope that this talk will be able to serve as both an introduction for beginners in test automation, specifically in Python, as well as provide an insight into a slightly atypical set of design patterns to those more experienced. And the same goes for this series of posts, which serves as representation of the proposed talks. To clarify, by test automation, I mean to only address our implementation of service level integration tests as well as end-to-end or integration tests at both client and client facing api levels.

For some context, FanDuel's current core product solution architecture, at a high-level, consists of a SOA of microservices and pseudo-microservices all using the JSON-RPC protocol, which is all served up to clients via a RESTful API. Our clients consist of just a responsive design website, Android and iOS native applications.

Finally, it always bears echoing around our industry that to implement a comprehensive, stable and scalable test automation infrastructure is difficult and should be carefully considered to why and what extent, if at all, it should be applied. Whilst we have certainly not got this exactly right yet, it is something we are progressing well with at FanDuel. Perhaps in another post, I'll address this in more detail but for the continuation of these series of posts, on test automation in python, I will assume at the very least you are interested in gaining a small insight into what this test automation implementation could look like. 

Starting right at the top, why Python? 

Your reasons for selecting a language or framework for automation, like with anything, should be based on context. Context in this case involves understanding at least the skillset of your colleagues, the complexity of your system or applications under test and the ability to interface with them. Of course, if you have test automation located in multiple repositories which are each testing different platforms (e.g. service layer, clients) then choosing the language for your automation to be the same as the language of the application under test may prove more beneficial, as you may increase your base of potential contributors. However that said, if I could reduce reasons down to two simple factors it would be this; Python has a lower barrier for entry and has an excellent selection of easy-to-use libraries and frameworks.

It is widely claimed, something which I hold to also, that Python has a more shallow learning curve, specifically from beginner to intermediate level, when compared to other languages such as Java, C# and Javascript. I don't want to delve into more detail on this, as this isn't based upon anything other than my experience with training colleagues who are entirely new to programming as well as those already exposed. It has a conciseness and intuitiveness that lends itself really well to helping you become productive and pragmatic.

Python has an extensive standard library as well as a vibrant community that has produced a wide range of fully featured, well documented and mature open source libraries and frameworks useful for a broad set of cases including testing. In this series I will specifically cover the test framework named pytest as well as complementary libraries that allow for automating clients using selenium webdriver, parallelisation, and easier debugging.

I hope that this series will provide you with a good insight into how you may go about implementing test automation or tooling in your projects or organisation. If I were to choose which part of this series I wish to impart upon you the most, it would be around the use of pytest fixtures as a means of injecting test setups and teardowns into your tests and how to structure these. Don't worry if this doesn't mean anything to you yet, this will be covered in future posts.

The series takes this form:

<ol type="a">
  <li>Prologue</li>
  <li>Getting off at the ground floor; reference to guides to getting setup with Python.</li>
  <li>An introduction to a test framework named pytest and its key features.</li>
  <li>Example - Writing a test for an API.</li>
  <li>Example - Writing a test for a browser.</li>
  <li>Example - Writing a test for a native client.</li>
  <li>On stability - Good rules of thumb and design patterns.</li>
  <li>On scaling - Options for reducing overall test run times.</li>
  <li>Wrapping up - High-level case study and series summary.</li>
</ol>

Hopefully these will be posted very soon!