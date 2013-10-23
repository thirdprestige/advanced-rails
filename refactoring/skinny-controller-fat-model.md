---
layout: default
title: Advanced Ruby on Rails
---

# Writing maintainable code

## Skinny Controller, Fat Model

Here is a real refactoring I implemented a couple weeks back. Volunteer opportunities are possible events that the current user can sign up for. 

When they press "Volunteer", we create a "volunteerism" (a record saying "this guy is attending this event, as a volunteer." You could also think of it as a ticket.)

### Code Smells

<pre>
module Dashboard
  class VolunteerOpportunitiesController  Dashboard::ApplicationController

    before_filter :authenticate_user!
    before_filter :load_user!

    helper_method :registered?

    respond_to :html, :js

    def index
      @events = Event.upcoming
      @attendance = Event::Attendance.new
    end

    def create
      @event = Event.find(params[:event_attendance][:event_id])
      @attendance = @event.attendances.build(attendance_params)
      @attendance.attendee_id = current_user.id
      @attendance.attendee_role_id = Role.find_by(name: 'volunteer').id

      if @attendance.save
        redirect_to dashboard_root_url, notice: 'You signed up for volunteer opportunity successfully.'
      else
        redirect_to dashboard_volunteer_opportunities_url, notice: "You're already signed up for this course"
      end
    end

  protected

    def attendance_params
      params.require(:event_attendance).permit(:event_id)
    end

    def registered?(e)
      e.attendances.find_by(attendee_id: current_user.id)
    end
  end
end
</pre>

Let's take a closer look at that create action: 

<pre>
...
    def create
      @event = Event.find(params[:event_attendance][:event_id])
      @attendance = @event.attendances.build(attendance_params)
      @attendance.attendee_id = current_user.id
      @attendance.attendee_role_id = Role.find_by(name: 'volunteer').id
...
</pre>

There's a lot going on there. 5 things, by my count:

1. Load an event
2. Build an attendance
3. Populate it with the event id
4. Populate it with the attendee id (user id)
5. Finally, populate it with the attendee's role (he's volunteering, not attending as a trainee).

How much of this can we push into the model?

### Refactoring into the Model

First, it's interesting that we have the `attendance_params` setup there. Because the attendance model `belongs_to` the `@event`, we really don't need to specify the `event_id` again...

<pre>
...
    def attendance_params
      params.require(:event_attendance).permit(:event_id)
    end
...
</pre>

So we remove the line `def attendance_params` and update our create action:

<pre>
...
    def create
      @event = Event.find(params[:event_attendance][:event_id])
      @attendance = @event.attendances.build do |attendance|
        attendance.attendee_id = current_user.id
        attendance.attendee_role_id = Role.find_by(name: 'volunteer').id
      end
...
</pre>

Alright, saved us a few lines. 

Let's jump over the `attendance.attendee_id =` line for the moment.  That makes sense for now. What about this `attendee_role_id = Role.find_by(name: 'volunteer').id` stuff, though? That seems like a bit much.

#### Add an Association

Here's what we have in the `Event` model: 

<pre>
...

  has_many :attendances,
    class_name: 'Event::Attendance',
    dependent: :destroy

  has_many :attendees,
    class_name: 'User',
    through: :attendances

...
</pre>

Associations can also accept _conditions_ (or filters.)  For example, we could say something like...

<pre>
  has_many :attendances,
    -> { where(active: true) }, # Only select active attendances
    class_name: 'Event::Attendance',
    dependent: :destroy
</pre>

With this condition, any attendances that we later mark as "inactive" (or `active = false`) would be ignored.

**The syntax for adding conditions to your associations is slightly different depending on your Rails version.  The syntax shown here is for _Rails 4)_, but refer to the documentation for your specific version.**

Alright, so let's rethink what we're trying to do here. Really we're creating a specific kind of ticket, right?  A "volunteer's ticket." What if we refactor this into the association?

<pre>
...
  has_many :volunteerisms,
    -> { where(attendee_role_id: Role.find_by(name: 'volunteer').id) },
    class_name: 'Event::Attendance',
    dependent: :destroy

  has_many :volunteers,
    class_name: 'User',
    source: :attendee,
    through: :volunteerisms
...
</pre>

**This does 2 things:**

1. `@event.volunteerisms` will now only retrieve attendances (or tickets) where the user's role is that of a volunteer.
2. `@event.volunteerisms.create` will _automatically_ pre-populate the `attendee_role_id` with the Volunteer role id.

How does this affect our create action?

<pre>
...
    def create
      @event = Event.find(params[:event_attendance][:event_id])
      @attendance = @event.volunteerisms.build do |attendance|
        attendance.attendee_id = current_user.id
      end

      if @attendance.save
        redirect_to dashboard_root_url, notice: 'You signed up for volunteer opportunity successfully.'
      else
        redirect_to dashboard_volunteer_opportunities_url, notice: "You're already signed up for this course"
      end
    end
...
</pre>

Now we're getting somewhere.

#### Refactor into Controller Filters

Let's review the 7 basic controller actions:

* index
* new
* create
* show
* edit
* update
* destroy

What is in common with *show*, *edit*, *update*, and *destroy*?  

They all need to look up a resource by the `params[:id]`.

### Exercise

Refactor the `Event.find` call into a `before_filter` named `load_event`.


**[Next: Presenters](/refactoring/presenters.html)**
