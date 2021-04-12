---
title: 'Angular vs React'
date: 2018-09-01
tags: ['Angular', 'React']
draft: false
featured: true
---

Ahh..I am sure you would have read umpteen number of blogs/articles comparing React, Angular (and Vue). Well this is going to be my version of it after having had a chance to work on both of them. I have developed a decent amount of projects in Angular and in React and kind of got a good grasp on both of these to write about their similarities/differences.

For starters, both of them are used to build SPAs and good at what they are supposed to. Lets' consider what are the elements that goes behind in building a modern day web app and lets try to compare and contrast based on those elements.

<!-- For those who are not interested in reading lot of theory, you could jump to this section where you could get a gist of it pretty quicky. :) -->

## Element # 1 - Building the User Interface

Both Angular and React enforce the Component based design when it comes to building the UI. You basically see a Page as a tree of components. React was a pioneer in this and Angular (v2+) adopted it. Both are in line the native [web component spec][web-components].

#### **_Verdict_**

Angular has a nose ahead here as the Angular (v6 and above) has started supporting Web Components which means any component developed using Angular(v6 and above) could be used as a stand alone Web component and could be used across any frameworks/library. So its 1-0 for Angular.

## Element # 2 - Fetching data from the backend

To facilate fetching/posting data from the backend, Angular has built in modules (Http/HttpClient) which is bundled as a part of framework. In the React world, Axios is a popular package which could used for this. React also have a polyfill for native fetch call which comes bundled.

#### **_Verdict_**

There is nothing to choose here as both offer same capabilities.Its a tie with each getting a point. So its 2-1 Angular.

## Element # 3 - Handling Form Validation

Form validations are a must in any client side application. Angular has build in Form Modules and provides 2 approaches - Template driven and Model driven. The form modules are built in as a part of the frameworks. React on the other hand leaves us in the dark here as it leaves it to the developer on how to implement it. I had a tough time in choosing the alternatives when it comes to Form validations in React until I stumbled upon [Formik][formik].

#### **_Verdict_**

Though I liked Formik very much and used it but just because I had to spend quite some time in choosing form validation libraries, I have to give this to Angular. So Angular leads 3-1.

## Element # 4 - Routing/Navigation

Routing is an integral part of any web application. In Angular, you have a separate module (Router Module) to handle routing and all of this go into a separate route files. React doesn't support routing out of the box but the most popular one is React Router. Prior to v4 of React Router, routes in react would need to be in separate(static) files but from v4 the Router got alligned with React's component philosophy and Routing happens dynamically as the component renders as the Router is treated as a component. You could read more of this [here][react-router].

#### **_Verdict_**

I have got nothing to choose amongst them so its a tie again. Its 4-2 for Angular.

## Element # 5 - State Management

Well, what I have observed from my experience so far is most of developers who just have angular background, the term _state management_ seems foreign to them. Though we have the concept of services in Angular which could be used to share state across components, this is something that is not built into the framework. React on the other hand is built on the core concept of state and how a state change could change the UI. This is one core thing that I think React got it really right.

#### **_Verdict_**

Though we have state management solutions like [ngRx][ngrx] and [mobx-angular] which are inspired from their React world counterparts of [Redux][redux] and [mobx][mobx], I would like to give this hands down to React for imbibing the philosophy of UI as function of state. **UI = f(state)**.That makes the scoreline 4-3 in favour of Angular.

## Element # 6 - Tooling

The tooling for a framework/technology is one that gets you started and going with a framework/technology as quickly as possible. For Angular we have [angular-cli][angular-cli] and React has [create-react-app][create-react-app]. I am also a big fan of TypeScript as I like type checking and the static analysis it provides when building app. create-react-app has a [starter][create-react-app-ts] for TypeScript too.

#### **_Verdict_**

I do not have anything here too as I found the experience similar when it comes to tooling for both React and Angular. Angular has a GUI wrapper for Angular cli called [angular console][angular-console]. I am pretty much sure React also has something similar and would come up with one if not available still. Its a tie still. Scoreline is 5-4 Angular.

## Element # 7 - Server Side Rendering

Server side rendering is also a key aspect when developing SPAs. You would want to have your landing/home page reach as fast as possible to your user and also have provision for SEO. Angular has Angular Universal which is again built into the framework for supporting SSR. React has built in support for SSR and for more advanced features we have the popular [next-js][nextjs].

#### **_Verdict_**

Again, I do not have anything to pick here, so another tie. Scoreline is 6-5 Angular.

## Element # 8 - Change Detection

Change detection is the mechanism by which the framework makes the browser DOM in sync with application state/data automatically by reacting to events. React uses the concept of [Virtual DOM][react-virtual-dom] to achieve this and Angular has its own [Change Detection algorithm][angular-change-detection] to make this work.

#### **_Verdict_**

Again, I do not have anything to pick here. The new change detection algorithm in Angular (2+) is a much much improved one over the Digest cycle implementation in AngularJs(v1.x). So its another tie. Scoreline is 7-6 Angular.

## Element # 9 - Programming Paradigm

React in general tries to bring in few functional programming aspects when building the UI - Immutable states, pure functions (through render function & reducers in Redux). Angular favours a little object oriented style of programming through - classes, services. But the developer could mix and match the programming style that he/she likes and feels appropriate to the project needs.

#### **_Verdict_**

As I like the functional way of thinking and solving problems, I would give this to React so that makes the scoreline 7-7

## Element # 10 - Testing

Finally, we get into how it is to write spec in Angular and React. Angular comes built in with Jasmine framework and its own testbed utilities. React comes with Jest as it framework and enzyme for rendering component.

#### **_Verdict_**

Both have their learning curves. Though I felt Jest+enzyme was easier to learn, I would consider them equals here. So the scoreline is 8-8.

## Conclusion/Disclaimer

The final scoreline gives you an indication on how Angular and React share lot in common. Again all these are based out of my little experience on working on these 2 lovely framework/library. I liked learning and working on both of them. Thanks to both the Angular and React developers community for helping us build awesome apps and helping the Web move forward.

P.S: I am yet to get my hands dirty on Vue. As soon as I get an opportunity to work on Vue, I would update my experience with Vue too. :)
Till then happy coding and learning !!

<!-- <table>
<thead>
<tr>
<td>
Element compared
</td>
<td>
Angular
</td>
<td>
React
</td>
<td>
Verdict
</td>
<td>
Scoreline
</td>
</tr>
</thead>
<tbody>
<tr>
<td>
Building the UI
</td>
<td>
Component based Design
</td>
<td>
Component based Design
</td>
<td>
Tie
</td>
<td>
1-1
</td>
</tr>
</tbody>
</table> -->

[formik]: https://github.com/jaredpalmer/formik
[react-router]: https://reacttraining.com/react-router/web/guides/philosophy
[web-components]: https://github.com/w3c/webcomponents
[ngrx]: https://github.com/ngrx
[mobx-angular]: https://github.com/mobxjs/mobx-angular
[redux]: https://redux.js.org
[mobx]: https://mobx.js.org/getting-started.html
[create-react-app-ts]: https://github.com/Microsoft/TypeScript-React-Starter
[angular-cli]: https://cli.angular.io/
[create-react-app]: https://github.com/facebook/create-react-app
[angular-console]: https://angularconsole.com/
[nextjs]: https://github.com/zeit/next.js/
[angular-change-detection]: https://vsavkin.com/change-detection-in-angular-2-4f216b855d4c
[react-virtual-dom]: https://reactjs.org/docs/reconciliation.html
