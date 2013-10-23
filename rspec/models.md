# RSpec and Cucumber

## RSpec for Models

This is where you'll spend most of your time with RSpec.

### Generating Models

Rails should automatically create the specs when you generate a model.

```console
rails generate model Page name:string
cat spec/models/page_spec.rb
```

### Vocabulary

A very simple RSpec file:

```ruby
require 'spec_helper'

describe Page do
  subject { Page.make! }

  it 'should setup a slug by default' do
    expect(subject.slug).to_not be_nil
  end
end
```

A few things going on here.  `describe` accepts a `class` as it's argument and runs everything inside the block.

A `subject` is a block that creates an example record or instance to work with.

`it` is like Test::Unit's `test`, we just name the test and it runs the block. Generally this is in the form "it 'should do x'".

Now we get to the weird stuff.
 
### Matchers

### Exercise


