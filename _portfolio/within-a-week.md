---
layout: post
title: WithinAWeek
thumbnail-path: "/img/waw-faker.png"
short-description: WithinAWeek is a simple to-do list application for short-term goals. Entries expire seven days after creation.

---
{% highlight ruby %}
def create
  @goal = current_user.goals.build(goal_params)

  if @goal.save
    flash[:notice] = "Goal saved"
    redirect_to user_path(current_user.id)
  else
    flash[:error] = "There was an error saving your goal"
    redirect_to user_path(current_user.id)
  end
end
{% endhighlight %}
