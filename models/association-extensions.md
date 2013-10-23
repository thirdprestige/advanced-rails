---
layout: default
title: Advanced Ruby on Rails
---

# Association Extensions

We can also refactor association extensions into modules. **Let's go back to our Photo &amp; Video example:**

<pre>
module Publishable
  def publish!
    proxy_association.target.update_all(published_at: Time.now)
  end
end

class User &lt; ActiveRecord::Base
  has_many :draft_photos, 
    conditions: { published_at: nil },
    dependent: :destroy,
    extend: Publishable

  has_many :draft_videos,
    conditions: { published_where(published_at: nil).where('processed_at IS NOT NULL') }, 
    dependent: :destroy,
    extend: Publishable
end
</pre>

## Cleaning up Association Definitions with `with_options`

There's a bit of repeated code there, which can happen pretty frequently with association definitions. Let's use `with_options` to refactor it a bit.

<pre>
class User &lt; ActiveRecord::Base
  with_options(
    conditions: { published_at: nil }, 
    dependent: :destroy, extend: Publishable) do |draftable|

    draftable.has_many :draft_photos

    draftable.has_many :draft_videos
  end
end
</pre>

`with_options` just sets up the "default options" for every method called off of `draftable`.

## More Resources

* [`with_options` on APIdock](http://apidock.com/rails/Object/with_options)
* [`with_options` RailsCasts](http://railscasts.com/episodes/42-with-options)

## Next

[Single-Table-Inheritance (STI)](/models/sti.html)
