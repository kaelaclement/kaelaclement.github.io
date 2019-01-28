---
layout: post
title:      "A Good Place for a Dining Car Joke"
date:       2019-01-28 00:39:31 -0500
permalink:  a_good_place_for_a_dining_car_joke
---


I never said I was good at clever blog titles. I don't, however, usually struggle with originality in general. You see, I've heard that recipe apps are a bit cliche for a Rails project. I can tell you I had my fair share of arguments with myself on the whole issue. In the end, however, it came down to something pretty simple: I'm a vegetarian. Not just a vegetarian, but a vegetarian who is low on time and energy and won't stand for tasteless food. Honestly, I hate salads of the pasta-less variety. In the age of the food blogger, easy recipes are hard to come by as it is - easy *vegetarian* recipes are unicorns in the wild.

Since half the battle for a vegetarian (especially a new one) is just finding something that sounds appealing to eat, I decided my join table would be a Review model. People can simply "like" a recipe, but also leave comments - most cooks know how valuable input from other cooks can be! I also set up my Recipe model to sort recipes by recently uploaded or by recipes with the most likes if you just need a recipe you can trust. I have nested routes for a user's recipes, so you can see more from a cook you like, and if you are that cook, routes for editing or creating a new recipe are also nested.

I enjoyed some good quality time with my models for this project. Right from the start I needed to alias some of my associations to differentiate between recipes a user had posted and those they've reviewed. I ended up using a few scope methods and playing around a lot with ActiveRecord query methods. Since the point was having good recipes, I wanted to make sure people could view the most liked recipes, as well as recipes liked by other users whose taste they trust. While databases can certainly be tricky, for me they're also half the fun.

It's hard to tell from the middle of a project, but as I write it all out I think my favorite part of all this is seeing just how much I've learned since Sinatra. I can only imagine what's next (...ok so I know exactly what's next in the curriculum but it's a good sign off, isn't it?).
