---
layout: post
title: Discovering MVP
description: Discovering the MVP architecture for Android
excerpt: Discovering the Model View Presenter architecture.
permalink: /blog/discovering-mvp/
---
Now that I'm transitioning from building toy apps to more complex ones, I found myself looking up best practices in architecting application code. That's when I came across the MVP architecture and a pretty solid implementation in this [repo](https://github.com/ribot/android-boilerplate){:target="_blank"}. Here are some of my notes.

### Model-View-Presenter
This architecture like most others focuses on maximizing [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns){:target="_blank"}. Essentially what this means is that code is broken down into components that serve their specific purpose without knowledge of the implementation details of other components.

**Key fundamental:** The Model provides data classes to the View and does the heavy lifting of API calls/database opeations. The View is 'dumb' and just displays content to the user and accepts user interactions which are relayed to the Presenter. The Presenter hosts business logic, organizes and prepares data for the view to display. Very intuitive.

For example, if the app fetches a list of users from an API, the View would simply accept a list of users and display them in a `RecyclerView` (say), the Presenter would request and collect data from the model and organize it in a way that the View can display it, and the model and its classes would perform network accesses and database operations. The Presenter has no idea how the View displays information or what the UI looks like. It just requests data, processes it and passes it on to the View. The View has no idea how the data was obtained or how it was processed, it just knows how to display it. Separation of concerns.

### Implementation
A beautiful usage of Java interfaces is at the heart of this implementation. All View components (Activity, Fragment etc.) implement an interface that defines their functionality. For example the `MainActivity` here displays a list of users, an empty state or an error state. Similarly, Presenters also implement an interface that defines their functionality.

{% highlight java %}
public interface MainMvpView extends MvpView {
    void displayUsers(List<User> users);
    void displayEmptyState();
    void displayError();
}

public interface MainMvpPresenter extends BasePresenter {
    void fetchUsers();
}
{% endhighlight %}

This serves as a contract between the Presenter and the View. Each Presenter holds a reference to the View interface and each View contains a reference to its Presenter interface. It is important that they depend on interfaces and not concrete classes. According to the Dependency Inversion principle, *“Depend upon Abstractions. Do not depend upon concretions”*. Dependencies are implementation agnostic. The Activity (View) delegates fetching of data to its Presenter and when the Presenter is done preparing it, it simply calls `getMvpView().displayUsers(List<User> users)`.

What I think is most beautiful about this is the decoupling of components. This makes it very easy for them to be developed and tested independently and parallelly. Changes in one don't affect the other. Heck, you could even completely switch out an implementation for another as long as the same interface is implemented. It also becomes easier to test app components separately with mock data.

Notably, all Presenters hold a reference to their Views, which are very volatile in Android apps. Even though the View is detached in `onDestroy()`, the system may choose to kill the app without calling `onDestroy()`. This could cause memory leaks. Some implementations use `WeakReference` as a solution but it seemed like a bandaid to me.

Discovering MVP and implementing it has been a solid experience. Among all the different architectural patterns out there, I suppose I picked MVP because it's popular and felt familiar to the MVC pattern I had learnt in school. It'd certainly be rewarding to explore other architectural patterns, especially MVVM whose reactive nature interests me. But for now, let's upgrade all projects to conform to this beautiful architecture.
