---
layout: default
title: Advanced Ruby on Rails
---

# Advanced Ruby on Rails

## Example Project

```git clone git@github.com:thirdprestige/advanced-rails-example.git```

## RSpec and Cucumber

* [Setting up RSpec](/rspec/setup.html)
* [RSpec for models](/rspec/models.html)
* [Using Blueprints with RSpec](/rspec/blueprints.html)
* [RSpec for controllers](/rspec/controllers.html)
* [RSpec for views](/rspec/views.html)
* [RSpec for helpers](/rspec/helpers.html)
<!-- * [RSpec for `lib/`](/rspec/libs.html) -->
* [Mocks](/rspec/mocks.html)
* [Writing Cucumber tests](/rspec/tests.html)
* [Debugging Cucumber with Launchy](/rspec/debugging-cucumber.html)
* [Using Guard with Rails](/rspec/guard.html)


## More Resources on Testing

* [Using Guard with Rails (RailsCasts)](http://railscasts.com/episodes/264-guard)
* [Testing like the TSA by DHH, Creator of Rails](https://37signals.com/svn/posts/3159-testing-like-the-tsa)
* [A Guide to Testing Rails Applications, by RailsGuides](http://guides.rubyonrails.org/testing.html)
* [Email Steps for Cucumber](https://github.com/bmabey/email-spec)

## Writing maintainable code

* [Skinny Controller, Fat Model](/refactoring/skinny-controller-fat-model.html)
* [Presenters](/refactoring/presenters.html)
<!-- * [Interactors](/refactoring/interactors.html) -->
* [Command Pattern](/refactoring/commands.html)
* [Concerns](/refactoring/concerns.html)
* Refactoring in the Real World

## More resources on Maintainable Code

Definitely check out _Refactoring: Ruby Edition_, if it's the only one in the list you look at. I read through the Java edition of Refactoring about 7 years ago and it changed how I craft software forever.

* [Refactoring: Ruby Edition](http://www.amazon.com/Refactoring-Ruby-Jay-Fields/dp/0321603508/ref=sr_1_1?s=books&ie=UTF8&qid=1382519003&sr=1-1&keywords=refactoring+ruby+edition)
* [Clarity over Brevity in variable and method names, by DHH](https://37signals.com/svn/posts/3250-clarity-over-brevity-in-variable-and-method-names)
* [Put chubby models on a diet with concerns, by DHH](https://37signals.com/svn/posts/3372-put-chubby-models-on-a-diet-with-concerns)
* [Pattern vision, by DHH](https://37signals.com/svn/posts/3341-pattern-vision)
* [Code Climate, Automated Ruby Code Review](https://codeclimate.com/)
* [Imperator gem, for the Command pattern, by Keith Gaddis](https://github.com/karmajunkie/imperator)

## Advanced model & association features

* [Understanding association proxy objects](/models/proxies.html)
* [Working with eager loading](/models/understanding-eager-loading.html)
* [Scoping](/models/scoping.html)
* [Using association callbacks](/models/association-callbacks.html)
* [Adding association extensions](/models/association-extensions.html)
* [Single-Table-Inheritance (STI)](/models/sti.html)

## Scalability & Caching

* [Performance Profiling](/scaling/performance-profiling.html)
* Page caching, Action caching, Fragment caching
* MemCached and plugins
* Using model caching

## Caching resources

* [Expirable Concern, by Nathaniel Jones](https://gist.github.com/nthj/7132073)
* [Multi Fetch Fragments gem](https://github.com/n8/multi_fetch_fragments)

## Other Rails Libraries & Resources

* [RailsAdmin, easy administrative consoles](https://github.com/sferik/rails_admin)
* [Foreman, handling environment variables in development](https://devcenter.heroku.com/articles/procfile)
* [`quiet_assets` gem](https://github.com/evrone/quiet_assets)
* [Timecop gem, Time Travel in Ruby](https://github.com/travisjeffery/timecop)
* [Terminal OS X Notifier for Guard](https://github.com/Springest/terminal-notifier-guard)

## Security

* [Ruby on Rails Security Guide](http://guides.rubyonrails.org/security.html)
* [GitHub security incident highlights Ruby on Rails problem](http://www.h-online.com/open/news/item/GitHub-security-incident-highlights-Ruby-on-Rails-problem-1463207.html)
* [Strong Parameters](http://weblog.rubyonrails.org/2012/3/21/strong-parameters/)
* [Rails Tip #7: Mass Assignment Security](http://excid3.com/blog/rails-tip-7-mass-assignment-security/)

## Deployment 

* [Airbrake.io](http://airbrake.io)
* [Airbrake JS](https://github.com/airbrake/airbrake-js)
* [Airbrake Resque](https://github.com/Viximo/airbrake-resque), asynchronous error reporting

## APIs

* [Rest-Client gem](https://github.com/rest-client/rest-client)
* [HTTParty Gem](https://github.com/jnunemaker/httparty)
* [Rails API](https://github.com/rails-api/rails-api)
* [Beginners Guide to Creating a Rails API](http://www.andrewhavens.com/posts/20/beginners-guide-to-creating-a-rest-api/)
* [GRAPE](https://github.com/intridea/grape)

## Guides

* [Active Support Core Extensions](http://edgeguides.rubyonrails.org/active_support_core_extensions.html)
* [Configuring Rails Applications](http://edgeguides.rubyonrails.org/configuring.html)
* [Zero-Downtime Deploys for Rails](http://www.slideshare.net/pedrobelo/zero-downtime-deploys-for-rails-apps)

## Generators

* [Creating and Customizing Rails Generators &amp; Templates](http://edgeguides.rubyonrails.org/generators.html)
* [Generate a JavaScript File in Rails](https://gist.github.com/nthj/7161156)

## Engines &amp; Gems

* [Blorghety gem](https://rubygems.org/gems/blorghety)
* [Getting Started with Rails Engines](http://edgeguides.rubyonrails.org/engines.html)
