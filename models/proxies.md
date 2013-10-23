---
layout: default
title: Advanced Ruby on Rails
---

# Understanding association proxy objects

A proxy object is just a *pointer* to the objects that our association references.

## Example

<pre>
class User &lt; ActiveRecord::Base
  has_many :draft_photos do
    def viewable_by?(user)
      user == proxy_owner # the user
    end

    def publish!
      # proxy_target is all of the user's draft photos
      proxy_target.update_all(published_at: Time.now) 
    end
  end
end

@bob = User.first

@bob.draft_photos.viewable_by?(@bob)
# => true

@bob.draft_photos.publish!
</pre>

## Why not just use class methods?

For example, we could also add the `publish!` method to the `Photo` class:

<pre>
class Photo &lt; ActiveRecord::Base
  class &lt;&lt; self
    def publish!
      proxy_target.update_all(published_at: Time.now)
    end
  end
end
</pre>

If there would be a reason to run `Photo.publish!` (to mark all photos in the database as published), then this would make sense. In this case, we probably only have a button for Bob himself to mark all his own photos as published, so it's more a concern of the association.


## More Resources

* [Active Record Associations on RailsGuides](http://guides.rubyonrails.org/association_basics.html)
* [Association Proxies on The Rails Way](http://www.therailsway.com/2007/3/26/association-proxies-are-your-friend/)
* [Using Association Extensions to Build Join Attributes on Has Many Through](http://reefpoints.dockyard.com/ruby/2012/04/03/use-association-extensions-to-build-join-attributes-on-a-hmt.html)
* [Advanced Proxy Usage](http://pivotallabs.com/advanced-proxy-usage-part-i/)


## Next

[Working with eager loading](/models/understanding-eager-loading.html)
