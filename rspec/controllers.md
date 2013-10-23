---
layout: default
title: Advanced Ruby on Rails
---

# RSpec for Controllers

Generally, I'm not a fan of creating controller specs. Your integration tests (Cucumber files) should catch 

<pre>
describe PagesController do
  describe "GET index" do
    it "assigns @pages" do
      team = Page.create
      get :index
      expect(assigns(:pages)).to eq([page])
    end

    it "renders the index template" do
      get :index
      expect(response).to render_template("index")
    end
  end
end
</pre>

This is just testing basic Rails functionality right here, and they're time-consuming to write.  Where these might make sense is if a specific controller action has a lot of branches (callbacks, or places to error out), and you want to cover all of those branches in your tests, but generally speaking, you're better off just refactoring your complicated controller logic into your models (which we'll cover more on later.)

I would go so far as to say that a controller spec is almost always either a waste of time OR a side-effect of a fat controller that should be refactored.

**[Next: RSpec for views](views.html)**
