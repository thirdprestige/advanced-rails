---
layout: default
title: Advanced Ruby on Rails, RSpec and Cucumber
---

# RSpec for ActiveRecord Models

This is where you'll spend most of your time with RSpec.

### Generating Models

Rails should automatically create the specs when you generate a model.

<pre>
rails generate model Post title:string body:text slug:string
rake db:migrate
cat spec/models/post_spec.rb
</pre>

This should default to something like:

<pre>
require 'spec_helper'

describe Post do
  pending "add some examples to (or delete) #{__FILE__}"
end
</pre>

### Vocabulary

Of course, we don't want to leave our `Post` spec pending forever. What if we just ensure that we calculate a slug (a friendly URL) whenever we create a post?

A very simple RSpec file:

<pre>
  require 'spec_helper'

  describe Post do
    subject { Post.create(title: 'Hello World') }

    it 'should setup a slug by default' do
      expect(subject.slug).to_not be_nil
    end
  end
</pre>

A few things going on here.  `describe` accepts a `class` as it's argument and runs everything inside the block.

A `subject` is a block that creates an example record or instance to work with.

`it` is like Test::Unit's `test`, we just name the test and it runs the block. Generally this is in the form "it 'should do x'".

### Exercise

Use ActiveRecord's `before_validation` filters to make this spec pass:

<pre>
  require 'spec_helper'

  describe Post do
    subject { Post.create(title: 'Hello World') }

    it 'should setup a slug by default' do
      expect(subject.slug).to eq 'hello-world'
    end
  end
</pre>


Run `rspec spec/models/post_spec.rb` to see if the spec passes or fails.

### Matchers

So, more about what's going on here.

`expect` accepts a result, and allows us to set up various expectations of that result.

Here, we say we expect it "to not be nil." We could also expect it TO be nil, for example:

<pre>
  require 'spec_helper'

  describe Post do
    subject { Post.new }

    it 'should have a default slug of nil' do
      expect(subject.slug).to be_nil
    end
  end
</pre>

Note that here, we have a `Post.new` in the `subject` block, not a `Post.create`, so our `before_validation` callbacks are not run, and this spec should pass.

### DRYing up your code: `let`

In addition to `subject`, you can set up additional records to be used across specs by using the keyword `let`:

<pre>
  require 'spec_helper'

  describe Post do
    subject { Post.make!(author: author) }

    let(:author) { User.make! }

    it 'should remember the author' do
      expect(subject.author).to eq author
    end

    # ... more specs that use `author` go here
  end
</pre>

**[Next: Using Blueprints with RSpec](blueprints.html)**
