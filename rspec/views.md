# RSpec and Cucumber

## RSpec for Views

First, don't. Second, don't. Third, don't. Here's why: 

1. View Specs are a pain to write, and that means they're expensive.
2. Views change frequently, generally more frequently than your business logic, especially early in development.
3. You have to manually test view changes anyway, to make sure they look OK in the browser. This essentially means view testing isn't automatable, so writing code to automate the task doesn't make sense.

## What if I introduce a bug or parse error or edge case?

Your integration tests should be catching this scenario, so you're already good.

### Example

```ruby
require "spec_helper"

describe "widgets/index.html.erb" do
  it "displays all the widgets" do
    assign(:widgets, [
      stub_model(Widget, :name => "slicer"),
      stub_model(Widget, :name => "dicer")
    ])

    render

    rendered.should contain("slicer")
    rendered.should contain("dicer")
  end
end
```


### Exercise

Generate a view spec for the `profiles/edit.haml` view. Ensure that if we end up with a non-existant state for our `@user` model, the view handles the error gracefully.

