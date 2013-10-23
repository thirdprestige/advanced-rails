# RSpec and Cucumber

## Setting up RSpec

### Installation

RSpec is a standalone gem, with a separate library for integrating with Rails. Installation is in two parts: Add `rspec-rails` to your Gemfile, and run the setup generator.

#### Part I: Gemfile

```ruby
group :development, :test do
  gem 'rspec-rails'
end
```

Of course then we need to bundle it all up by running `bundle install`.

#### Part II: Generator

```console
rails generate rspec:install
```

RSpec has done a few things for us here.  We have a `.rspec` file, which we can store different RSpec options in. `--color` is a good one. You can examine the different options by running `rspec --help`

We also should have a `spec/spec_helper.rb` file now. This is where we store logic that runs before we run our test suite.  

Finally there is the `spec/support` directory. We can throw random methods, setup configuration, or things of that sort in there.

**Tip: If you need test-specific code, it's generally preferred to write the code in your app as you need it in development & production, then monkey-patch it in the test environment, instead of adding another branch to your code with an `if Rails.env.test?`**


### Running specs

Run the entire suite with `rake spec`.  Done and done.

### Running specific tests

But that takes forever. What if we're working one specific chunk of code? 

```console
rspec spec/models/user_spec.rb
```

Or, we can even run specific tests:

```console
rspec spec/models/user_spec.rb --e age
```

We can also group specs by tags:

```ruby
  it 'should pre-populate the user feed', slow: true do
    subject.feeds.copy_from_template!
  end
```

```console
rspec spec/models/user_spec.rb -t slow
```

### Auto-running specs with Guard

We don't have time to cover Guard here, but if you guys get tired of running your tests, it's a fantastic gem for automatically re-running the specs or code you just changed. It will even integrate into different notification systems, like the OS X notification center, so you can passively watch your test suite without leaving your editor.


