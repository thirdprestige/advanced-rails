---
layout: default
title: Advanced Ruby on Rails
---

# Concerns

## Example: Sluggable

In Rails 4, `app/models/concerns/sluggable.rb`: 

<pre>
module Concerns::Sluggable
  extend ActiveSupport::Concern

  included do
    raise "Requires column 'slug'!" unless self.class.column_names.include?('slug')

    before_validation on: :create, if: :name?, unless: :slug? do
      self.slug = name.parameterize
    end

    delegate :to_param, to: :slug, allow_nil: true

    validates :slug, presence: true, uniqueness: true
  end
end
</pre>

And `app/models/page.rb`:

<pre>
class Page &lt; ActiveRecord::Base
  include Concerns::Sluggable

  # ...
end

</pre>

## Breakdown

Concerns are Ruby modules, but with a bit of extra flavor. All the magic happens in `extend ActiveSupport::Concern`.

Once we call that, `ActiveSupport` gives us 2 features:

1. `included do...end`, which is code that is run in the context of the class including it, and only once that class has included it.  Basically, we can extract class-level setup into the `included` block.
2. A magical `ClassMethods` module, which is injected into the class level.

## `module ClassMethods`

<pre>
module Concerns::Sluggable
  extend ActiveSupport::Concern

  included do
    raise "Requires column 'slug'!" unless self.class.column_names.include?('slug')

    before_validation on: :create, if: :name?, unless: :slug? do
      self.slug = name.parameterize
    end

    delegate :to_param, to: :slug, allow_nil: true

    validates :slug, presence: true, uniqueness: true
  end
end
</pre>


## Exercise:

Rework the Sluggable concern to support different column names.  We should do this by definining a class method named `acts_as_sluggable_using`, and it should accept 2 arguments: `title_column`, and `slug_column`.

When we're done, this code should run:

<pre>
class Page &lt; ActiveRecord::Base
  include Concerns::Sluggable

  acts_as_sluggable_using :title, :slug
end

</pre>

<!-- <pre>
module Concerns::Sluggable
  extend ActiveSupport::Concern

  module ClassMethods
    def acts_as_sluggable_using(title_column, slug_column)
      raise "cannot find column '#{title_column}' unless column_names.include?(title_column.to_s)
      raise "cannot find column '#{slug_column}' unless column_names.include?(slug_column.to_s)

      before_validation on: :create, if: "#{title_column}?", unless: "#{slug_column}?" do
        write_attribute slug_column, send(title_column).parameterize
      end

      delegate :to_param, to: slug_column, allow_nil: true

      validates slug_column, presence: true, uniqueness: true
    end
  end
end
</pre> -->


## Extension: 

Alright, that's great, but now we just have a module with a couple layers of indirection. Let's tweak the Concern to look for the magical columns `title` and `slug`, and if they both exist, set up the `acts_as_sluggable_using` automatically.

When we're done, this code should run again: 

<pre>
class Page &lt; ActiveRecord::Base
  include Concerns::Sluggable
end
</pre>


**Next: Refactoring in the Real World**
