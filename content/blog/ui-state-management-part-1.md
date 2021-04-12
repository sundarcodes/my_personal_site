---
title: "State Management in Angular 2+ apps - Part 1"
date: 2017-12-01
tags: ["State Management", "RxJS", "Mobx", "Redux", "ngRx", "Angular 2+"]
draft: false
---

This is part-1 of the blog series where I would be trying to explain state management in Angular 2+ app using different strategies along with their pros and cons.

In this part, I will try to demystify the term state, how to identify state and why do we need to manage state. So lets get started...

### How do you define state or what is state of an app ?

I have seen junior developers who just have Angular(1 & 2+) background struggle to answer this question as they haven't encountered the term **state** while building apps. This is where getting to know different alternative like React or Vue helps. React is built on the philosophy of UI is a function of state. Please read [this][ui-function-of-state] to understand more about the philosophy.

Coming back to the question, a state as per the definition represent an app's data at a given point of time.
Programatically speaking, a state is nothing but the variables - objects, number, arrays that you use in your JS code to represent the data to be displayed in the UI. So now you can relate to the `UI=f(state)` principle which implies UI changes as state changes.

Lets take a sample app and try to decipher the state based on the above lines. Incidentally, we would be also using same app to apply different state management strategies. It is a simple CRUD app which has a collection of my good reads (books or blogs) and you could manage it meaning - add/edit/delete. Also if you notice there is counter which indicates the number of books/blogs that I have read. So thats about the app's functionality.

![alt text][good-reads-landing]

Before we try to identify the state, lets look at how I have broken the page into components. 

#### Home Component
Below is the home component.It hosts the list of reads. Basically its the container.  

![alt text][good-reads-home-component]

#### Card Component
Below is the one of the card component. These are presentational components which takes inputs and emits events as outputs whenever there is an event like marking the checkbox, deleting etc. 

![alt text][read-card-component]

These are also called as dumb components as they do not do anything apart from I/O and its responsibility of the parent component containing or hosting these dumb components to handle events and react to them. So the home component hosting these components is called a Smart component as it knows and decides what to do on getting an event. Dumb component promotes reusability as they could be fitted in any other component or page with a similar requirement.

#### Navbar Component

Next is the navbar component. It's our header for all our pages and has a counter which tracks the book/blogs that I have read. It has to get incremented whenever I mark an item as read and decremented whenever I uncheck the checkbox.

![alt text][good-reads-navbar-component]

#### Form Component

And finally we have the form component, which would be used to add new blog/book or edit an exiting item.

![alt text][good-reads-form-component]

To help us decipher the state of the app, lets try to understand the different kinds of state.

### Local State
Local State refers to the set of data stored in your variables that pertains to just the component, meaning the set of data that are not exposed or shared by some other components. An example could the state in the form component when you are creating or editing a new item. You would be need to store the form data as local variables to do validation before saving it.

### Shared State
Shared state refers to the set of data that is being shared by more than 1 component meaning 1 or more component is directly or indirectly being dependent on the data.

So in our app, the shared state would be the list of items as this is being shared by the home component and also the navbar component which tracks the number of reads.


### Now the next question, why do we need to manage state ?

Okay thats a very valid question. Managing local state is easy as it is just going to reside within a single component, so we don't really have to care about managing it as we know whatever changes that happens to the data would only affect the component and nothing else. But when it comes to shared state, the same argument would not hold good. As the state is being shared, we would need to find a way to manage this sharing. Picture the following :

![alt text][state-management-compexity]

The above picture is a Venn diagram representation of apps where the circles represent components in the app. The intersection points represent the shared states amongst the components. As you could see, larger the app, more the components, more the intersections and more is the complexity of maintaining the state. Hence we need a state management solution !!!

There you go, that brings to us the end of this post. Hope you got a bit of understanding on state and what is the need to manage them. In the next set of posts, we will be seeing each one of the different technique in detail. The entire source code for this blog series is available in github. You could find it [here][github-repo].

* Managing state using Angular services - [Part 2][part-2]
* Managing state using RxJS (Subjects n Observables) - [Part 3][part-3]
* Managing state using Mobx via angular-mobx - [Part 4][part-4]
* Managing state using Redux via ngRx - Part 5


[ui-function-of-state]: http://beletsky.net/2016/04/the-functional-approach-to-ui.html
[good-reads-landing]: /img/my-good-reads-landing.png
[good-reads-state]: /img/my-good-reads-with-state.png
[good-reads-home-component]: /img/home-component.png
[good-reads-navbar-component]: /img/navbar-component.png
[good-reads-form-component]: /img/form-component.png
[read-card-component]: /img/read-card-component.png
[state-management-compexity]: /img/state-mgmt-complexity.png
[github-repo]: https://github.com/sundarcodes/my-good-reads-app
[part-2]: {{< ref "ui-state-management-part-2.md" >}}
[part-3]: {{< ref "ui-state-management-part-3.md" >}}
[part-4]: {{< ref "ui-state-management-part-4.md" >}}



