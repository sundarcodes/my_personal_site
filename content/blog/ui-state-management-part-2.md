---
title: "State Management in Angular 2+ apps - Part 2"
date: 2017-12-11
tags: ["State Management", "Services", "Change Detection", "Angular 2+"]
draft: false
---

In [part-1][part-1] of this series, we got to know what state is and the need to have a state management solution especially in the case of a large web app. In this post, we will be seeing how we can solve our shared state problem using plain Angular services.

In the last post we identified that the good reads collection is the data that is being shared by the home component and the navbar component. So we know that when we need to share data between components, we just need to introduce a service, place the data in the service and inject the service in components that needs it. This is the approach which we would have taken if we are working in AngularJS(Angular 1.x) also this is precisely what we will be doing here.

```javascript
@Injectable()
export class BackendService {
  baseAPIRURL = 'http://localhost:3000/api';
  reads: GoodRead[];
  subs: Subscription;
  constructor(private http: HttpClient) {
    this.reads = [];
  }
```

We have a `BackendService` which is going to hold our collection in the reads variable. `GoodRead` is our model class which is going to have all attributes of blog/book like title, description, catergory etc.

```javascript
export class GoodRead {
    constructor(public title: string, public description: string, public category: string,
        public url: string, public isRead: boolean, public id?: number) {

    }
}
```

The service is going to talk with the backend and get the data and store it back in the `reads` variable.

```javascript
 fetchAllReads() {
    const url = `${this.baseAPIRURL}/index`;
    this.subs = this.http.get<GoodRead[]>(url)
      .subscribe(data => this.reads = data);
  }
```

And we would be exposing this `reads` array using a getter.

```javascript
get allReads(): GoodRead[] {
    return this.reads;
  }
```

Now in our home.component.ts, we would inject this service and in the home.component.html, we would be using this the service in our html to render the items in the DOM.
```
    <div class="col-sm-4"
      *ngFor="let readItem of backendService.allReads">
      <div class="card">
        <div class="card-body">
```

Now to display the number of items read, we expose a getter in the `BackendService`. I will come to the console.log statement in just a while.

```
  get allReadsCount(): number {
    console.log(`Reading counter at ${Date.now()}`);
    return this.reads.filter(read => read.isRead).length;
  }
```

We would be using this method in the navbar component to display the number of reads.
```
  <ul class="nav">
    <li class="nav-item">
       Read
      <span class="badge badge-primary badge-pill">{{backendService.allReadsCount}}</span>
    </li>
  </ul>
```

Please look [here][service-mgmt-code-base] for the complete code base.

We have achieved what we wanted but is this the best solution. This offcourse is the simplest and more importantly it works!! But it has its own pitfalls. Lets see what are those.


#### Pitfall # 1 - Relying on Angular's Change Detection
Here, you have to understand how Angular magically updates the __DOM__ whenever the `reads` variable in the `BackendService` gets updated. This happens through a change detection mechanism that gets triggered on every event that happens and Angular checks all the variables referenced in the HTML to see if its reference or value has changed. If there is a change, Angular rerenders the correspoding part of the DOM.

The following line in the `navbar.component.html` is supposed to show the value of the total number of read items. I have added a console log inside the allReadCount getter, just to show the number of times the getter function is called.

```
<span class="badge badge-primary badge-pill">{{backendService.allReadsCount}}</span>
```

You could observe from the below snapshot that for every event that happens in the UI, keypress, mouse scroll etc, Angular triggers the change detection mechanism to check any value has changed. This is a costly operation and for large scale apps with lot of component, this will cause performance issues. So this also help us understand how Angular does this magic of updating the DOM when we change anything in our state in the service.

![alt text][change-detection]

The read counter gets called for every keypress that I do in the form which is not required.

#### Pitfall # 2 - Exposing the state data
If you notice, although we have a getter method to fetch all the `reads` from the `BackendService`, we are exposing the entire array out to the public. Any component, service which has access to `BackendService` could potentially alter/update the array. Although we can make it private, we still need to have a getter which would return the reference to the array thereby exposing the array outside and breaking the encapsulation. This would make trouble shooting and would create a maintenance nightmare when you don't know which component is consuming and updating the array inadvertently when the app gets bigger.


#### Summary
This approach of managing state through Angular Services turns out to be a [anti-pattern][anti-pattern]. Please do not use this technique. In the next [post][part-3], we would be seeing a better approach using Observables.

[part-1]: {{< ref "ui-state-management-part-1.md" >}}
[part-3]: {{< ref "ui-state-management-part-3.md" >}}
[service-mgmt-code-base]: https://github.com/sundarcodes/my-good-reads-app/tree/master/frontend/angular/services-based-state-management/my-good-reads-app
[change-detection]: /img/change-detection.png
[anti-pattern]: https://en.wikipedia.org/wiki/Anti-pattern
