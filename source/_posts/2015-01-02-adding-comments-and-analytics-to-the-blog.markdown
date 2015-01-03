---
layout: post
title: "Adding comments and analytics to the blog"
date: 2015-01-02 20:37:26 -0600
comments: true
categories: [coding, blogging]
---

Adding Disqus and Google Analytics to Octopress is pretty trivial. I found entries in the `_config.yml` file for `google_analytics_tracking_id` and `disqus_short_name` that needed values. I signed into Disqus and clicked "Add Disqus to your site" and filled out the fields. I then put the corresponding short name into the Octopress config file.

Google Analytics was similarly easy: Go to https://www.google.com/analytics and fill out the forms to generate a tracking code, then paste that code into the Octopress config file.

Redeploy the site and you are good to go! Easy peasy!
