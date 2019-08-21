---
layout: post
title:      "Just add RSpec!"
date:       2019-08-21 16:29:48 +0000
permalink:  just_add_rspec
---


We're going to add RSpec to Rails today, as in it's August of 2019 and things change quickly out there. I found some [solid resources](https://medium.com/@amliving/my-rails-rspec-set-up-6451269847f9) for adding RSpec to a Rails app; but after running into a deprecation warning and discovering a wonderful gem often overlooked, I realised this is exactly the time for a blog post. Join me for a quick walkthrough of how exactly I decided to set up RSpec with my Rails app ([How To Veg](https://kaelaclement.github.io/a_good_place_for_a_dining_car_joke)).

How did I even get here? I'll tell you. I promise I won't go all food blogger on you, but feel free to skip to the goods anyway. So I graduated from Flatiron. Then I got on a plane to England, and had a jet-lag fuelled (mini) existential crisis. What comes after graduation? Aside from a visit to the UK. Is it time to do a bunch of new projects? Should I upgrade my existing projects? What should I be learning?

I've heard the advice more than once in the last week: focus on what you love. While I panicked about limiting myself, far more experienced developers said, "Focus on what you want to be doing. If you love frontend, build that expertise, don't waste your time with the database." So there was step one, I know I love backend development. I'm still working out some kinks on what steps 3+ will be, but step 2 turned out to be, "add testing to your current projects." Of course I know *about* TDD. While writing tests is considered more advanced, one of the first things we learn is what Test Driven Development is, why it works, and how to run the tests. I've gathered that, at my level, I'm not necessarily expected to write robust tests, but that doesn't mean I can't get started.


### The Goods


## Step 0.5: What about this test folder?

If you're like me, you forgot to tell Rails you didn't want tests/wanted to use RSpec when you ran `rails new`. In that case, there's already a test folder in your root directory. We don't need this if we're using RSpec.

## Step 1: I need what gems, now?

The relevant portions of my Gemfile:

```
group :development, :test do
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]

  gem 'rspec-rails', '~> 3.8'

  gem 'capybara', '>= 2.15'
  gem 'selenium-webdriver'

  gem 'factory_bot_rails'
end

group :test do
  gem 'chromedriver-helper'

  gem 'database_cleaner'

  gem 'shoulda-matchers'
end
```

I mostly followed the docs on this one. Like most things, there could be a completely different blog post on why things are done this way, but essentially some things go into the :development and :test groups because you want a seamless experience while, well, developing & testing.

The gems we're using:

1. [RSpec](https://github.com/rspec/rspec-rails) is, of course, the testing framework we're here for in the first place.
2. [Capybara](https://github.com/teamcapybara/capybara) will give us a very helpful DSL for feature testing
3. [Factory Bot](https://github.com/thoughtbot/factory_bot) replaces Rails fixtures for data our tests will use later (formerly Factory Girl, renamed for [reasons](https://github.com/thoughtbot/factory_bot/blob/master/NAME.md))
4. [Database Cleaner](https://github.com/DatabaseCleaner/database_cleaner) will help with our testing database, namely we're going to tell it when to wipe data
5. [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers) has some really helpful methods for testing basic functionality, like validations and associations

## Step 1.5: The obvious part I forget every time

Run `bundle install` / `bundle`

## Step 2:  Remember that test folder?

We still don't need it. But we *are* going to run `rails generate rspec:install` which will generate

* the `app/spec` directory
* `spec/rails_helper.rb`
* `spec/spec_helper.rb`
* `app/.rspec`

I promise, we're so close. We just need to hop into `spec/rails_helper.rb`

## Step 3: Config, config, config

Under `require 'rspec/rails'` you'll need to add:

```
require 'database_cleaner'
require 'capybara/rspec'
```

#### Database Cleaner

In the `RSpec.configure` block, change

`config.use_transactional_fixtures = true`

to

`config.use_transactional_fixtures = false`

then add

```
config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end

  config.before(:each, type: :feature) do
    driver_shares_db_connection_with_specs = Capybara.current_driver == :rack_test
    unless driver_shares_db_connection_with_specs
      DatabaseCleaner.strategy = :truncation
    end
  end

  config.before(:each) do
    DatabaseCleaner.start
  end

  config.append_after(:each) do
    DatabaseCleaner.clean
  end
```

This is specific to using Capybara which, of course, we are.

#### Factory Bot

At the top of your `RSpec.configure` block, you have this:

`config.fixture_path = "#{::Rails.root}/spec/fixtures"`

You want this:

`config.include FactoryBot::Syntax::Methods`

#### Shoulda Matchers

Finally, to use these lovely methods, at the very very bottom of your `spec/rails_helper.rb` or in its own file if you'd like, you'll need to add this little block:

```
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

## Step 4: Write the things

Now comes the fun part, writing those tests! Rails generators will now generate your spec and factory files for new models, controllers, etc. One last tidbit: if you wanted to add tests for something that already exists, there's a generator for that, too. Say I have a User model and a Posts controller that already exist, and I want to add testing? I can simply run:

`rails generate rspec:model User`

or

`rails generate rspec:controller Posts`

...and off we go!

## Step 5: ???

There is so much more that will go into testing, much of it (ok all of it) is new territory for me. There's more configuring one might want to do, and very well may do later. There are, I'm sure, scores of other gems to help out. Who knows what's around the corner as I learn to write tests myself, rather than just read them. But, for now, we got set up fairly quickly and easily, and we did it together.


