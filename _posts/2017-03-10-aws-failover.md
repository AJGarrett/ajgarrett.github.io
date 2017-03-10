---
layout: post
title: Failover Routing with Amazon Web Services
categories: [web development]
tags: []
description: Protecting your website from service outages in the AWS cloud with Route53, S3, and Cloudfront
---

Like many, the [service disruption on Amazon Web Services](https://aws.amazon.com/message/41926/) (AWS) hit us too.   Our website which has sat in the default region experienced some performance issues as a result of failure that happened on Amazon's infrastructure, so my big task this week was to determine if I could minimize the likelihood of this same issue in the future.

### Our Configuration at the Time of the Failure
Our current setup on Amazon was a standard S3 (Simple Storage Solution) bucket sitting behind Cloudfront to cache, zip, and deliver our content.   Using Cloudfront has been a recent change, so our S3 bucket was named athletesinaction.org as we had been hosting straight a year s3 for almost a year before pushing it behind CloudFront.  This is important as you'll see below.

### Finding Amazon's Failover Limitations
In my research to determine the best ways to implement failover, I found three issues in what Amazon would allow me to do.

1. My first thought was to create a duplicated setup in another region -- S3 and CloudFront.   The first issue though is that Amazon does not allow you to setup duplicate CNAME records on multiple CloudFront instances.  Therefore, I couldn't have a backup instance of Cloudfront sitting there configured in case of failure.

2. With duplicated CloudFront setups out, the backup plan became that in a failover we'd rollback to S3 hosting in another region.   The limitation here is that in order to route a 'A' DNS record to S3 within Route53 (Amazon's DNS in AWS), the bucket name must match the host's A record name.  Therefore, I had to re-configure what S3 bucket was my source for CloudFront so in a failover I could serve files from my athletesinaction.org S3 bucket.

3. For failover to work, you have to be able to assosciate your DNS A record with a Health Check within Route53.  Naturally, I felt that I need to monitor my website's response time, so I created a Health Check to monitor my domain.  However, Route53 won't allow you to associate a Health Check that monitor a website with the 'A' record for that site.  I got around this by creating a monitor on the full S3 bucket URL, that my domain is pointed too.

### Our New Configuration

With all that in mind, here is the new setup with failover configured.

1.  Create two S3 buckets in different regions with versioning and cross-region replication enabled.   The primary bucket can be named anything, but the secondary bucket should be named to match the hostname of the website it's serving as the failover for, i.e.  athletesinaction.org.

2.  Configure CloudFront with the primary S3 bucket as it's origin, and the hostname as a CNAME for the distrubution.  *Side note, you also need to list the www version of your site as a CNAM if it's routing here as well.*

3.  Create a Health Check in Route53 to monitor the uptime on the primary S3 bucket using the S3 bucket's hostname.

4.  Configure failover on the A record for the domain in Route 53.   The primary record should point towards CloudFront, and be set to fail if the status of the Health Check failed.  The secondary record should point to S3 and only be used on failover.

All in all, it's not a lot of work, even if it's never needed.  Depending on your site and it's size, the cost is limited to the cost of keeping the two S3 buckets in sync across regions.