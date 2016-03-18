---
layout: post
title: Intergrating Slack into Your Orchard Workflows
categories: [.net, productivity]
tags: [orchard, slack]
description: How to use Slack within your Orchard Workflow
---

The last few months I have been working on migrating one of our organization's current websites to a new design.  We've been using <a href="http://www.orchardproject.net/" target="_new">Orchard CMS</a> for a few years becuase as a .NET dev team, it provided so opportunities for expansion within our current tech stack.  

However, one of the issues we have found looking at the old site during the migrating wasn't a technical one.   The issue has been the lack of workflow.  Content gathered was posted that didn't always go through an approval or review proccess.   There were issues with both spelling and design consistency that weren't being found do to the lack of workflow.   This needed addressed.

Looking towards the future, from the onset of me joining this team, I've desired to change this by not just getting a site rolled out, but to implement the necessary workflows to be able to track and maintain our site's standards.  To do this, I wanted to intergrate into our teams current tools by sending notifications into <a href="http://slack.com" target="_new">Slack</a> to alert the approvers that content was updated.   This took some reasearch and a question on Stack Overflow (oh, how I love thee....) to solve.   In order to maybe help others, I thought I would outline the process I went through to set this up. 

<ol style="list-style:decimal;">
<li> Go to your team's Slack site, and set up a webhook.   You'll receive a long url like: https://hooks.slack.com/services/##################################</li>

<li> In Orchard, add a Web Request to your Workflow.</li>

<li>In the Workflow, you'll paste in the url of the webhook. Then you'll POST a JSON object.   The trick is the format.  Brackets are protected in Orchard, so you wrap the JSON with (( )).
<pre>
(("text": "The following page was updated:  
       http://youdomain.com{Content.DisplayUrl} \n 
       You can edit/approve it here: http://yourdomain.com{Content.EditUrl}")) 
</pre>  
</li>
</ol>

The final result will look like this: <img src="/assets/media/orchard-workflow.png" style="border: #000 1px solid;">