---
layout: default
title: Advanced Ruby on Rails
---

# RSpec for Views

First, don't. Second, don't. Third, don't. Here's why: 

1. View Specs are a pain to write, and that means they're expensive.
2. Views change frequently, generally more frequently than your business logic, especially early in development.
3. You have to manually test view changes anyway, to make sure they look OK in the browser. This essentially means view testing isn't automatable, so writing code to automate the task doesn't make sense.

## What if I introduce a bug or parse error or edge case?

Your integration tests should be catching this scenario, so you're already good.

### Example

<pre>
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
</pre>

### OK, but really, when would I use these?

I have run into a couple places where I think they made sense.  In the Retreat Engine, a User progresses through a sign up wizard. We store the "state", or where they are in the sign up wizard, in their user record.

Then, our sign up wizard view renders one of several different partials based on the "state" that user record is in.

In some cases, when we renamed views, or introduced new views, or re-arranged views, the wizard view would try to render a non-existant partial, and error out. We ended up writing a view spec to handle this case, since it was a recurring issue, did not have anything to do with what data we were showing, and was unlikely to need to be re-written later.

### Exercise

Generate a view spec for the `profiles/edit.haml` view. Ensure that if we end up with a non-existant state for our `@user` model, the view handles the error gracefully.

**[Next: RSpec for Helpers](helpers.html)**
