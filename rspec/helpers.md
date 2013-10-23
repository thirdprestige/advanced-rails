# RSpec and Cucumber

## RSpec for Helpers


### Example

A file named "spec/helpers/application_helper_spec.rb" with:

```ruby
require "spec_helper"

describe ApplicationHelper do
  describe "#page_title" do
    it "returns true" do
      helper.page_title.should be_true
    end
  end
end
```

A file named "app/helpers/application_helper.rb" with:

```ruby
module ApplicationHelper
  def page_title
    true
  end
end
```
