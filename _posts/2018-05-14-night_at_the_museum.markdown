---
layout: post
title: "Night at the Museum"
date: 2018-05-14 21:42:05 +0000
permalink: night_at_the_museum
tags: [CLI]
---

I’m a big fan of the American Museum of Natural History in New York; that’s the one that was featured in the movie “Night at the Museum.” I visit the museum fairly often and go to special events from time to time. So when the time came to build my first CLI application, I knew what I wanted to do – I wanted to make the Calendar page of the museum easier to navigate for myself!

I knew that I wanted to be able to search for upcoming events based on event type. Believe it or not, but the museum’s [Calendar page](http://www.amnh.org/calendar?facetsearch=1) does not allow you to search by event type. So right away I knew that I would be aggregating my events into an array that would be able to return event type, name, date, short description, and a url to the page where I can purchase tickets.

I started building my CLI application from the CLI itself. This means that I started with CLI class and put in a few initial methods to greet the user and provide him/her with a list of event types based on the upcoming events. I also created a method that would return more detail about individual events based on user’s input.

Once I was happy with my very basic and completely hardcoded CLI, it was time to move on to real work – scraping! Scraping is definitely a tedious task, it made me love Pry. Some of the items, like event type, name and date, were actually pretty easy to get. What I had more trouble with was a short description of the event because I was ending up with some unnecessary information simply because of the way html code was written. I had to rely on some good old Googling to figure out a way to remove some of the unnecessary information. I found that gsub worked perfectly for this task.

I had to write a series of methods to get me from raw data I was getting from the webpage to the presentable format for the user. I also wrote some methods just to please myself, like ability to list event types in alphabetical order and provide the user with a url to a more detailed page. Honestly, I added those things just for myself, knowing that I probably will be using this application when going to the museum.

Once I was pleased with the scraped data I was getting, I replaced my hard coded CLI methods with the real data, added a few more items just for presentational purposes and set out to see how I can improve my code.

While trying to scrape the data I took advantage of a one-on-one with a technical coach because I was having trouble with some return values of my methods. Thinking back at that, I probably could have done some more Googling and figured it out, but I have to say that I learned a lot during my one-on-one session. In the end I had the answer to my question and also walked away with some new knowledge and suggestions on how to write better code.

Overall, I am pleased with how my application turned out. At the very least, I am now able to search museum events by type and decide which events I want to attend. I know it’s not an entirely altruistic approach to building CLI applications, but as Avi Flombaum always says, “Write the code you wish you had!”
