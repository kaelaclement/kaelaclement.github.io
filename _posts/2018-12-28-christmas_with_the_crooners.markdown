---
layout: post
title:      "Christmas with the crooners"
date:       2018-12-28 22:18:23 +0000
permalink:  christmas_with_the_crooners
---


My Sinatra project presented me with an entirely new set of problems. Rather, it was time for my Sinatra project while in the middle of a new set of problems. The most obvious being, the holidays (that's a lot of time with Sinatra in my household). Does anyone stay on track during those last two weeks of the year? I'll want to know their secrets if they do.

All that to say, though it was slow building, I quickly knew what I wanted my Sinatra project to be - an app to keep track of a user's medications. With the purpose of being as helpful as possible for the individual, I wanted it to be customizable. Each medication has a name, dose, refill date, and any notes one wants to leave themselves about how the med is working. Even people who don't take an abundance of medications can have trouble remembering what they've taken and how it worked for them, so having notes was especially important.

I was grateful to learn of the Corneal gem, which took care of the more tedious set up work. I used two models, medications and users, with each medication belonging to a user and each user having their own medications. Every step of the way, it was most important to validate that a user could only add to, view, edit, and delete their own medications.

My biggest challenge has been confronting my strong preference for backend. I had previous experience with HTML & CSS and even some introductory JavaScript, but the entire Sinatra section I delighted in the behind the scenes logic with a little less love for making my views presentable. Mixed with the hectic holidays, it was easy to simply not touch my project. As with everything else, however, there is a certain joy and satisfaction of making something myself, and even cleaning up my views (greatly aided by the Corneal gem's auto-generated CSS file) felt like an accomplishment.
