---
title: 'How did I go about choosing my techstack for my side web app project?'
date: 2018-12-20
tags: ['Tech stack', 'Backend', 'Frontend', 'Data Store']
draft: false
featured: true
---

I find it its always good to work on a pet/side project outside my official dev work as it gives me room to explore new stuffs and in that process help in analyzing the pros/cons of choosing different technologies/frameworks for a use case. I am documenting my thoughts on how I went about choosing the stack (front end/back end technologies, code organization, devops workflow) for one of my side project which was a web app.

## Backend

### Architecture

1.  Choosing a backend framework for building the API - Rails vs **express** or Ruby vs **JavaScript**
    As I had worked on Rails before I was tempted to go with Rails to build my backend knowing its capabilities and the awesome community. But I decided to go with express as wanted to have one language JavaScript/TypeScript all the way just to be more productive and avoiding the petty issues with the language syntax and not to toggle **language switch** from Ruby to JS/TS and vice versa often. Also, I wanted to explore and learn express/NodeJs capabilities of building a RESTFUL API.

2.  Choosing a NodeJS framework - **express** vs HAPI vs SAILS
    After deciding to go with a JS based framework for building the backend, the next task was to pick one. Sails was a big contender as it was inspired from Rails and would make me comfortable working on it as I have got used to the conventions of Rails. But again I wanted to build something from scratch as it would give me a better understanding of express and middlewares and [this][node-js-udemy] particular course from Udemy just made me to take my decision easier. So I decided that I will be starting with express as it would be give me more control of my code.

3.  Choosing a Database - **NoSQL** vs RDBMS
    Now, it was time to pick a database. Again, decisions had to be made. I picked up a NoSQL datastore as it would be more productive for me and my app requirements could change down the line so a NoSQL datastore would cater to it. Also I wanted to explore and learn working on one.

4.  Choosing a NoSQL Database - **MongoDB** vs Cassandara
    Again we are spoilt for choices a here. I picked up MongoDB, a Document based NoSQL store as it was mature amongst the other offerings. Cassandara was another contender but I stuck to Mongo.

### Code Infrastructure setup

After finalizing the framework and the database, it was time to get my hands dirty by setting up the code skeleton and architecture. I did the following.

1. Dockerization
   Decided to dockerize the entire setup as it would be easy while deploying and environment setup. I had docker containers for

   - Mongo DB
   - Backend
   - Frontend

2. TypeScript (TS)
   As I like strong typing, so integrated typescript as all my backend code would be using TypeScript.

3. Testing Framework
   I chose [Jest][jest] here as it was the latest amongst the popular JS testing frameworks. Also (spilling the beans here), as I wanted to use React for the front end, so using Jest would make it a consistent test framework all across.

4. Setting up Authentication
   Though [passport][passport] should have been a no-brainer for this, I decided to implement my own way of implementing auth using jwt tokens.

5. Setting up Authorization
   I am big fan of Ruby Authorization gems like [CanCanCan][cancancan] and [pundit][pundit]. So I started looking for similar packages in npm. I landed up with [CASL][casl] which is closest to what their more mature and powerful Ruby cousins could offer.

6. Setting up Logging library
   Here, [winston][winston] was quite dominant, so I had used it. Not much of decision making here.

7. Setting up Swagger
   As swagger gives beautiful API documentation, I took the help of a library called [tsoa][tsoa] which relies on few annotations/decorators in our controller code and the produces routes & generates the swagger.json

8. Setting up data validatons
   used [joi][joi] for input data/screening validations. I liked the declarative approach it gives.

9. Setting up emails
   For this I used [nodemailer][nodemailer] for sending emails and [email-templates][email-templates] for customizing the mail text.

10. Setting up CI
    Next, as I was using gitlab as my git repo, I thought of using gitlab's built in CI. It was easy peasy stuff to setup up the automatic build and running of tests whenever a merge happens to the master branch.

## Frontend

### Architecture

Again, just like I had to make decisions before I started implementing the backend, I had a similar time this time with the frontend.

1.  Choosing a frontend framework - Angular vs **React** vs Vue

    I was very tempted to use Angular as thats what I have been working on from beta version yup from 2016 June. But things changed when I started learning React to deliver a basic training on React just before I started with this app. I started liking the mental model that React brings to the table when designing a front end app. Though React is just a view library, the community was very mature and it offers a plethora of libraries to suppliment and make React a complete framework.
    I like Vue too as it has taken the best parts from both Angular and React but I just went with React as it was more mature.

2.  Choosing a CSS Framework - Bootstrap Vs **Material Design**

    I just went for Material Design as I liked its principles of UI & UX. I like bootstrap too but just gave in to Material Design.

3.  Choosing a Material Design React Component Library - React toolbox vs **React Material-UI**

    Here too, I had to make a choice of the component library that I had to use. Went in for react material-ui based on github popularity.

4.  Choosing a state management solution - Mobx Vs **Redux**

    As I know this will turn out into a large scale app, I wanted to go with Redux as it is opinionated and we could build more maintainable and more predictable apps. I have used ngRx in Angular projects too so I knew what Redux principles could bring to the table.

### Code Infrastructure setup

Now it was time to setup the skeleton for the front end architecture. I started with the authentication feature and also added CRUD actions for a feature.

1. TypeScript (TS)
   Just as I wanted to have strong typing in the backend, I wanted to have strong typing on the front end too. Though React goes well with Flow, I preferred TypeScript to go hand in hand with backend code. This will also promote sharing code across backend and front end if needed. I used the create-react-app typescript starter.

2. Setting up Redux
   I heavily borrowed lot of concepts/steps from [this][react-redux-typescript] page for setting up redux with epics and also using TS.

3. Setting up Router
   I used [react router v4][react-router] as the latest version (v4) has aligned itself to the mental model required when building React apps. Treating route itself as another component was a killer move.

4. Setting up Form Validation
   For form validation, I used [formik][formik]. It creates a Higher Order Component (HoC) over our form component and exposes a validation schema API through [Yup][yup] a light weight clone of Joi which I had used in the backend.

5. Setting up E2E Testing framework
   I had used [cypress][cypress]. Absolutely loved the developer experience and the speed and ease with which I could write test specs.

There goes my frontend and backend stack.. With the tech evolving at a rapid pace, I am sure this stack would be obselete more sooner than later. The important take away should be how we need to understand the fundamentals behind how things work as they may not change in a short span of time. But one thing we must remember is we being in the tech space must keep learning !!

[node-js-udemy]: https://www.udemy.com/nodejs-master-class/
[jest]: https://jestjs.io/
[passport]: http://www.passportjs.org/
[cancancan]: https://github.com/CanCanCommunity/cancancan
[pundit]: https://github.com/varvet/pundit
[casl]: https://github.com/stalniy/casl
[winston]: https://github.com/winstonjs/winston
[tsoa]: https://github.com/lukeautry/tsoa
[mongoose]: https://github.com/Automattic/mongoose
[joi]: https://github.com/hapijs/joi
[nodemailer]: https://github.com/nodemailer/nodemailer
[email-templates]: https://github.com/niftylettuce/email-templates
[jest]: https://jestjs.io/
[react-router]: https://github.com/ReactTraining/react-router
[react-redux-typescript]: https://github.com/piotrwitek/react-redux-typescript-guide
[formik]: https://github.com/jaredpalmer/formik
[yup]: https://github.com/jquense/yup
[cypress]: https://www.cypress.io/
