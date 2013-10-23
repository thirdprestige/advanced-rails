---
layout: default
title: Advanced Ruby on Rails
---

# RSpec for Helpers

## Example

A file named "spec/helpers/application_helper_spec.rb" with:

<pre>
require "spec_helper"

describe ApplicationHelper do
  describe "#page_title" do
    it "returns true" do
      helper.page_title.should be_true
    end
  end
end
</pre>

A file named "app/helpers/application_helper.rb" with:

<pre>
module ApplicationHelper
  def page_title
    true
  end
end
</pre>

The key gig here is that magical object `helper`, which allows to access all of our helper methods.

**Bonus trick: You can actually use the same method in the rails console:**

<pre>
rails console
> helper.page_title
=> true
</pre>

## Especially useful for permission-based code

Most complicated apps have different roles and permission levels. It can be helpful to push these flags into helper methods, like so:

<pre>
def may_volunteer_for_event?(event)
  event.upcoming? and event.volunteer_spots_remaining? and not @current_user.already_attending?(event)
end
</pre>

We have at least 3 conditions in this one helper method, now, and we may want to check that we don't break it later on if the logic changes.  So, in `spec/helpers/dashboard/volunteer_opportunities_helper.rb`, we might have:

<pre>
require 'spec_helper'

describe Dashboard::VolunteerOpportunitiesHelper do
  context 'permissions' do
    # Our old friend `let` 
    let(:past_event) do
      Event.make!(ended_at: 3.weeks.ago)
    end

    # Hurray! Callbacks
    before do
      # "assigns" sets up "@current_user", just like if we were 
      # inside our controller or view or helper files
      assigns(:current_user, User.make!)
    end

    it 'should hide the volunteer button for past events' do
      expect(
        may_volunteer_for_event?(past_event)
      ).to be_false
    end

    # ... more tests here
  end
end
</pre>


## Exercise

Add 2 more specs for different VolunteerOpportunitiesHelper scenarios.


**[Next: Mocks](/rspec/mocks.html)**
