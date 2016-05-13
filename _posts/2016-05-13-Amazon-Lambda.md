---
layout: post
title: Using AWS Lamdba to Publish Website Changes on a Schedule
categories: [general]
tags: []
description: Harnessing the power of AWS to automate the update of sites built with Gulp.
---

At the begining of the year, we went live with a new homepage for our organization, <a href="http://athletesinaction.org">Athletes In Action</a>, that uses Gulp to build generate HTML from markdown files, etc.  It's pretty nifty, and working with Gulp has a breath of fresh air.  We like the leaness that we get from using static site generator and the control it provides.However, seeing that we are pushing new content nightly, it's easy to question why not use a CMS to automate some of this work.   

As I have come more alongside this project, one of the tasks I've been given to tackle is if we can automate when the nightly update gets pushed live.   I explored a number of possible avenues, including TravisCI, but in the end we liked having the control of knowing exactly what was staged on our staging site prior to go live.

So I ended up developing a solution using AWS built-in Lambda scripting tool being run on a schedule using a CRON task in a CloudWatch event.   I won't fully re-document everything I did here, as Amazon has done a fairly good of that, but I will share the few things I learned.

Here was my ultimate goal for this project:  <span style="font-style:italic">every night at 11:30pm, copy what's in our staging bucket to our production bucket</span>.

To get started, I would read through their tutorial <a href="http://docs.aws.amazon.com/lambda/latest/dg/with-scheduledevents-example.html"> on using Lambda with Schedule Events</a>.

It took me a little bit of work, especially since I was new to Node, to learn that I couldn't just script this in Amazon's inline editor.   You actually have to create a project, and do npm install.  For me this just required I do a install for aws and async.   After you do that, you create a index.js file, and then zip the files inside the project folder together, and upload them to Amazon. 

Here was my final script (which I was greatly helped with by this anwser on <a href="http://stackoverflow.com/questions/30959251/how-to-copy-move-all-objects-in-amazon-s3-from-one-prefix-to-other-using-the-aws">Stack Overflow</a>):
<script src="https://gist.github.com/AJGarrett/d7c68a0a9f2b2b18f4905f8d045f7aa0.js"></script>

Once the script was tested and proved to be working, I then was able to set up a CRON job with CloudWatch, per Amazon's documentation.  This was very straight-forward.  The one catch was that the CRON tasks run on UTC time, not the local time.  For my needs, I had to create the task to run at 3:30 AM UTC, to fire at 11 PM EST.  

There was definitly some trial and error during the process, but now we can have changes ready to go before we leave the desk at 5, and not have to worry about manually syncing buckets each night.