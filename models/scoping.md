---
layout: default
title: Advanced Ruby on Rails
---

# Scoping

Scopes are just DSLs for creating named, chainable class-level methods that piece parts of an ActiveRecord query together.

So, I could ask the Video model for all the important, recent, unpublished videos:

<pre>
Video.unpublished.uploaded_recently.important

class Video &lt; ActiveRecord::Base
  scope :unpublished, -> { where(published_at: nil) }

  scope :uploaded_recently, -> { where('created_at &lt; ?', 30.minutes.ago) }

  scope :important, -> { where('importance > 2') }
end
</pre>

According to the Rails Guides, 

> **This is exactly the same as defining a class method, and which you use is a matter of personal preference.**  

It's definitely more verbose: 

<pre>
Video.unpublished.uploaded_recently.important

class Video &lt; ActiveRecord::Base
  class &lt;&lt; self
    def unpublished
      where(published_at: nil)
    end
    
    def uploaded_recently
      where('created_at &lt; ?', 30.minutes.ago)
    end

    def important
      where('importance > 2')
    end
  end
end
</pre>

## Scope Arguments

Scopes are just lambdas, and they accept arguments like any lambda:

<pre>
Video.unpublished.uploaded_recently.important

class Video &lt; ActiveRecord::Base
  scope :rated, ->(rating) { 
    where('rating >= ?', rating)
  }
end

Video.rated(3) # => only videos rated 
</pre>

## Maintainable Code

Scopes are essentially just another strategy for 'extract method'. In your views, it's a best practice to name "what you want", and push the logic for the query into a scope in the model.

<pre>
# app/views/videos/trending.html.erb

&lt;%= Video..each -%&gt;
  # do stuff
&lt;% end %&gt;
</pre>

**This can be replaced with:**

<pre>
class Video &lt; ActiveRecord::Base
  scope :trending, -> { 
    where('rating > 4').
      where('created_at >= ', 2.days.ago).
      where('published_at IS NOT NULL')
  }
end
# app/views/videos/trending.html.erb

&lt;%= Video.trending.each -%&gt;
  # do stuff
&lt;% end %&gt;
</pre>

Also, now we can use this logic elsewhere.  Say we need to email the CEO a list of trending videos every morning:

<pre>
class DailyReportMailer &lt; ActiveMailer::Base
  def trending_videos
    @videos = Video.trending

    mail to: 'ceo@example.com'
  end
end
</pre>

We can reference the concept of a "list of trending videos" without duplicating the business rules behind it.

## Exercise 

> Find 3 places in your views that describe the _how_ instead of the _what_, and refactor them into model scopes

## More Resources
* [Scopes in ActiveRecord](http://guides.rubyonrails.org/active_record_querying.html#scopes)
* [Scopes API dock](http://apidock.com/rails/ActiveRecord/NamedScope/ClassMethods/scope)

## Next

[Using association callbacks](/models/association-callbacks.html)
