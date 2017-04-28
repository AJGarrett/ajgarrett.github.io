---
layout: post
title: Building a CMS on top of a Gulp Site
categories: [web development, gulp, node, cms]
tags: []
description: Enabling content creators to publish content without a fully-fledged CMS
---

When we launched a [Gulp-based](/2017/03/04/big-gulps.html) site last year, my boss and I have both enjoyed the flexability of a templating-system, backed by the speed of a static site.   No worries about databases and server-side configs, just a bunch of flat HTML files.  And I've developed a number of build tasks to allow the site to really operate like a CMS such as archive and category pages, RSS feeds, tagging as well.

The downside to all this has been we, as the developers, have also been the ones to post on the content after the editors finished it.   More hands have had to touch the content, which means it's not getting up as fast, and we're modifying Markdown vs writing code.   While at times this has been exiciting, it's also been a lose-lose for everyone too.

Correcting the issue has been my challenge.   Over the next few weeks I am going to present my solution in a series of blog posts that outlines the process I've developed to allow content creators have control over the publishing process by giving them tools to own the site using [Prose](http://prose.io), [Slack](http://slack.com), [Travis](http://travis-ci.org), and [AWS](http://aws.amazon.com).



