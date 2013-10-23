---
layout: default
title: Advanced Ruby on Rails
---

# Performance Profiling

## Helper methods

### ActiveRecord

<pre>
Project.benchmark("Creating project") do
  project = Project.create("name" => "stuff")
  project.create_manager("name" => "David")
  project.milestones &lt;&lt; Milestone.all
end
</pre>

### Controllers

Inside a Controller, we can either just use `Project` like we would anyway, or also access the `benchmark` method on the controller class:

<pre>
# Inside a controller
self.class.benchmark("Creating project") do
  project = Project.create("name" => "stuff")
  project.create_manager("name" => "David")
  project.milestones &lt;&lt; Milestone.all
end
</pre>

### Views

Use it straight up!

<pre>
&lt;% benchmark("Creating project") %&gt;
  &lt;%= render @projects %&gt;
&lt;end&gt;
</pre>

## Strategies

Generally, it's just a matter of methodically breaking down what's going on until you find bottlenecks.  Reading your server logs will get you most of the way there.

## New Relic

Subscribing to New Relic can help you catch weird stuff in Production that only shows up with certain data or certain conditions.

## More Resources

* [Performance Testing Rails Applications, RailsGuides](http://guides.rubyonrails.org/v3.2.13/performance_testing.html)
