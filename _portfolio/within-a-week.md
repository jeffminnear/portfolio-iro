---
layout: post
title: WithinAWeek
thumbnail-path: "/img/waw-faker.png"
short-description: A simple to-do list application
feature-img: "img/geometry.png"
---

###Summary

Within-A-Week (WAW) is a self-destructing to-do list application built in Rails. Users can create goals for themselves and then have seven days to complete those goals before they expire.

As a student, I wanted to learn the fundamentals of back-end web development and create a project that people could actually use.

###Development

To build the application, I utilized several industry-standard gems. I used Devise to provide users a simple and reliable way to sign up and confirm with an email address, and then sign in to the service. I also used Twitter Bootstrap to style the layout into something that would be familiar and easy to navigate.

Functionality is based around CRUD operations performed on the Goal model both by users through the interface,

![interface](/img/waw-interface.png)

and a custom rake operation performed once a day via Heroku Scheduler to delete any expired goals.

I decided to add an <strong>urgency</strong> attribute to Goal that changes the color of the entry (from green to orange, and then to red) as time runs out to complete the goal.

{% highlight ruby %}
  def urgency(goal)
    time_elapsed_in_days = (Time.now - goal.created_at) / 1.day
    if time_elapsed_in_days > DANGER_THRESHOLD_IN_DAYS
      return "danger"
    elsif time_elapsed_in_days > WARNING_THRESHOLD_IN_DAYS
      return "warning"
    else
      return "ok"
    end
  end
{% endhighlight %}

This is then called by the partial that renders each goal entry, making use of the custom CSS selectors I created.

{% highlight html %}
  <div class="goal goal-<%= urgency(goal) %>">
{% endhighlight %}

###Testing

I used Test Driven Development to build WAW, incorporating several other gems (including RSpec, Capybara, Factory Girl, and Shoulda) to test not only low-level functionality,

{% highlight ruby %}
  it "assigns the goal with the correct attributes" do
    post :create, user_id: my_user.id, goal: { name: "do the laundry" }
    my_goal = my_user.goals.first
    expect(my_goal.name).to eq("do the laundry")
  end
{% endhighlight %}

but also actual (simulated) user experience at the highest level.

{% highlight ruby %}
  visit "/users/sign_in"
  fill_in "Email", with: user.email
  fill_in "Password", with: user.password
  click_button "Log in"
{% endhighlight %}

![Feature Spec](/img/feature_spec_optimized.gif)

###Conclusion

I am quite happy with the way WAW turned out. However, I would like to add a more responsive (and attractive) front-end interface, and am currently in the process of embedding JavaScript calls into the applications actions to connect WAW with [Do-Tell](do-tell.html), one of my other student projects.
