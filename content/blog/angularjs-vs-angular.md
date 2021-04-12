
---
title: "Difference between AngularJS and Angular"
date: 2017-03-25
tags: ["angularJS", "angular"]
draft: false
---

[Angular][angular] the framework as this is what the Angular team would like to call, has evolved(and evolving) from being the most popular front end JavaScript framework over the last 5 years to a framework that has transformed itself to survive and be a leader for years to come. All references to Angular points to the version 2 and above and AngularJS means Angular 1.x.

In this post, we would be seeing how different is Angular and what are the new additions it is bringing to the world of Single Page App development. Before we start with that, lets first understand what triggered the development of Angular which is essentially a rewrite of the AngularJS framework. Lets discuss about the pain points in AngularJS that made the Angular dev Team rewrite the framework from scratch.

## Pain Points in AngularJS

#### Scope pollution
To connect the view with data, we know we have to use the $scope object. It was so easy to add whatever the view needs in the scope object and have it rendered in the DOM by Angular. But what this has lead to was too many objects added to the scope and all these cluttered through our controller code which makes maintaining and debugging the application hard. In addition to the current scope, we could also access parent or root scope freely which again creates havoc when we update data at different places in our code base.  

#### Digest Cycle - Boon becoming a bane
Two-way data binding was one killer feature that made developers go 'wow' and one of main reasons to increase Angular JS adoption. But this also brought in the sometimes vicious digest loop. As we have more expressions, ng-repeat, ng-show/hide etc in our views, the digest loop is going to take more time to complete as we would be adding more watchers to the Angular watcherlist. This is going to impact our page responsiveness. Please refer to this [blog][angularjs-perf] on how to improve your AngularJS App's performance.

#### Communication among different UI View/Screens - Deep Mesh(s)
$broadcast, $emit, $on are used to communicate among sections of your App but all these slow down our App performance as it has to sometimes traverse the entire scope and if its a big complex app, it will have a definite impact on app responsiveness.

OK. Lets now look at whats new in Angular 2 and how it is addressing the above mentioned issues.

## Whats in Angular 2 ?

#### Think in Terms of Components
Angular team realized that the MV* way of building SPA was not fitting into the realm of modern web app development. In came [Web Components][web-components] approach popularized by [ReactJS][react-js]. This is one of the core change in Angular 2, thinking of UI design in terms of components and how they interact and communicate. There is no more concept of global scope as each component has its own scope. This also promotes reusablility of components across the application and even across the organization in the case of an Enterprise.

#### Better Change detection Strategy - Bye Bye Digest Cycle
With the component based approach, the change detection strategy has been well optimized thus eliminating the digest cycle. Dirty checking is still employed to detect the changes, but this happens in just one pass across the Component Tree. Use of immutable objects and observables aids in building highly performant apps. For more info on the change detection strategy, please refer to Victor Savkin's [blog][victor-blog].

#### Embraces uni-directional flow of data
One common problem that we come across when we build large complex SPA is state management. [Redux][redux] like patterns aim at solving this problem. Though Angular still supports one way and 2 way binding, its better to avoid them especially when you are developing large scale apps where managing state could be a big issue. This design philosophy comes from the React world and we have state management containers like [ngrx-store][ngrx-link] helps in building scalable, maintanable and more importantly *predictable* apps with Angular 2+. We also have a [angular-redux][ng2-redux] which has the same API as of Redux.  

#### Platform agonistic - Angular can run on Server now
SPAs are not SEO friendly. With Angular Universal, we can render the first page on the server and send the HTML to the browser thus improving the UX and also making them SEO friendly. As with AngularJS, we can also build Desktop Apps and Hybrid Mobile Apps on top of frameworks like [Electron][electron] and [Ionic][ionic].

#### Embracing TypeScript - Stronger Type Checking
With so much of code written in Javascript both for backend and frontend, the not so good features of the languages was hindering the progress of this massive JS movement. In came [TypeScript][typescript], which is a super set of JS with stronger type checking. Now, you could catch lot of errors at compile time rather than at run time. Angular team themselves have used TypeScript to build lot of Angular 2 framework code if not all and they did acknowledge that it helped them in capturing lot of bugs at development time.

#### Performance Benefits, Tooling etc
Ahead of Time Compilation (AOT), Lazy loading, Smaller bundle sizes are aimed at lesser page load time and improving the UX. With Angular CLI, scaffolding a new Angular App is super easy with all the tools out of the box. 

## Conclusion

If you are at the cross roads and heavily invested on AngularJS, I would recommend to start and adopt the component way of designing and developig UI Apps introduced from AngularJS 1.5. This would be the first step as you slowly migrate to Angular (2 or above). And with Angular [5][ng4-changes] out now, keep looking for the new and cool features getting added to the framework and how you could use them in your apps. Till then, happy learning and coding !!

[angular]: https://angular.io/
[web-components]: https://developer.mozilla.org/en-US/docs/Web/Web_Components
[react-js]: https://facebook.github.io/react/
[victor-blog]: https://vsavkin.com/change-detection-in-angular-2-4f216b855d4c
[typescript]: https://www.typescriptlang.org/
[angularjs-perf]: https://www.alexkras.com/11-tips-to-improve-angularjs-performance/
[redux]: http://redux.js.org/
[ngrx-link]: https://github.com/ngrx/store
[electron]: https://electron.atom.io/
[ionic]: http://ionicframework.com/
[ng4-changes]: https://blog.angular.io/version-5-0-0-of-angular-now-available-37e414935ced
[ng2-redux]: https://github.com/angular-redux/store