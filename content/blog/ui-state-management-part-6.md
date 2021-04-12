---
title: 'State Management in Angular 2+ apps - Part 6 - Conclusion'
date: 2018-03-01
tags: ['State Management', 'Redux', 'ngRx', 'Angular 2+', 'what to choose']
draft: false
---

Over the last 5 posts in this series, we have seen various ways of managing state in a Angular application. In this post, we would conclude by giving some pointers on which technique to choose based on the use cases.

### When to go for a Service based approach ?

This is most easiest and the simplest approach to share/manage data across components/pages in your App. But this could quickly go haywire if not used in the right way and with proper encapsulation techniques.

- Use for a very small app (having 3 to 5 pages)
- Take into consideration the automatic change detection strategy that Angular employs and the cost of it in your App

### When to go for an Observables based approach ?

This approach is good if you first have a thorough understanding of how observables work and if you are going to build a small app which has lot of moving parts and state getting shared all over. Also if you are handling server push events through web sockets, hooking on to an observable and updating the DOM whenever you get data from the server, then observables would be a good choice.

- Use RxJS Observables when you first have a solid understanding on how Observables work
- Use for a very small to medium app (having 5 to 10 pages)
- Use OnPush Change Detection Strategy to optmize performance

### When to go for a Mobx based approach ?

Mobx is a great solution for state management that does lot of magic out of the box. With less code, you get more things done.

- Use Mobx when you have a small team of developers working on a large app with each of them understanding how Mobx works and all working on agreed coding guidelines.
- Mobx is more of a syntactic sugar for Observables based approach with Mobx library doing lot of heavy lifting.
- Use Mobx when you want to quickly get started with a state management solution
- Use OnPush Change Detection Strategy to optmize performance

### When to go for a Redux based approach ?

- Use Redux when you are going to develop a large scale app with a big team of developers.
- Use Redux when you do not mind writing more code to wire up things and
- Use OnPush Change Detection Strategy to optmize performance

I personally prefer Redux as it gives you the feel that you are managing state the right way and I am personally a big fan of the redux dev tools.

Thanks for reading this far !!!
