---
layout: default
title: Advanced Ruby on Rails
---

# Debugging Cucumber Features with Launchy

When a Feature isn't work or breaks, it can be hard to brainstorm where the hangup is. Fortunately, we can view the actual HTML being rendered using a gem called Launchy.

## Installation

Gemfile: 
<pre>
group :test do
  gem 'launchy'
end
</pre>

## And the support file

In `features/support/debugging.rb`: 
<pre>
After do |scenario|
  # We can call `save_and_open_page` from any scenario, 
  # or it runs at the very end of any failing scenarios
  save_and_open_page if scenario.failed?
end
</pre>

**[Next: Using Guard with Rails](/rspec/guard.html)**
