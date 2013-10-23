---
layout: default
title: Advanced Ruby on Rails
---

# Using Blueprints with RSpec

Models usually need data to be useful, and coming up with example data over and over again bloats your spec files and is annoying.

Also, as you add validations, creating "test records" becomes more and more tedious. And adding a validation can easily break all your existing tests.

Machinist &amp; Faker combined make a killer 1-2 punch for quickly testing records: 

<pre>
require 'faker'
require 'machinist/active_record'

# Add your blueprints here.
#
# e.g.
#   Post.blueprint do
#     title { "Post #{sn}" }
#     body  { "Lorem ipsum..." }
#   end

User.blueprint do
  full_name = Faker::Name.name
  first_name { full_name.split(/\s/).first }
  last_name { full_name.split(/\s/).last }
  email { Faker::Internet.email }
  register_as { %w[donor trainee].sample  }
  created_at { rand(100).to_i.weeks.ago }
  confirmed_at { 3.weeks.ago }
  password { "test1234" }
  password_confirmation { "test1234" }
  birth_date { 18.years.ago }
  gender { "male" }
  emergency_contact { "John Smith" }
  emergency_phone { "555-5555" }
  relationship_status { "single" }
  education { "highschool" }
  ethnicity { "white" }
  street { "3001 RR 620 South" }
  city { "Austin" }
  geo_state { "TX" }
  zipcode { "78722" }
end

# ... rest of your models here
</pre>

Now, we can ask for a usable record in one line:

<pre>
require 'spec_helper'

describe User do
  subject { User.make! }

  it 'should have a city' do
    expect(user.city).to eq 'Austin'
  end
end
</pre>

## Setup

Add `machinist` and `faker` to your Gemfile. You might think you only want them in the `test` group, but it's tricky: you also want to use them when running `rake db:seed` in your development &amp; staging environments.

I generally add them to all groups, like this:

<pre>
gem 'faker', require: false
gem 'machinist', require: false
</pre>

Then, in `spec/spec_helper.rb` and `db/seeds.rb`, I just have to `require Rails.root.join('spec', 'support', 'blueprints')`.

## Using Machinist &amp; Faker in your `db/seeds.rb` file:

<pre>
100.times { User.make! }

5.times { Page.make! }
</pre>


**[Next: RSpec for Controllers](/rspec/controllers.html)**
