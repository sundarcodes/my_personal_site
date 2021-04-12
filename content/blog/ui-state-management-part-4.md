---
title: "State Management in Angular 2+ apps - Part 4"
date: 2018-01-04
tags: ["State Management", "Mobx", "Change Detection Strategy", "Angular 2+"]
draft: false
---

In [part-1][part-1] of this series, we got to know what state is and the need to have a state management solution especially in the case of a large web app. In this post, we will be seeing how we can solve our shared state problem using a solution called Mobx. 

Mobx is a non-opionated state management technique which again uses concepts of observable that we had seen in [part-3][part-3]. You could say mobx is a wrapper built on top of observables and does lot of magic under the hood. I won't go in depth on how and what of mobx. There are lots of good tutorials, blogs on this. You could check out the official documention of mobx [here][mobx-documentation].

Before we see how to implement mobx in our good reads app, lets try to understand few terms which are the building blocks of mobx and correlate them to our app.

#### Store
Store is simply a object thats going to hold our state. So in our app, this would just hold the list of books/blogs.

#### Actions
Actions refer to any event that could change the state of our app. In the context our app, actions could be adding a new read,deleting a read or marking a book/blog as read.

#### Derivations
Derivations are values that could be computed or derived from a state. In our app, the total read counter could be derived from the reads collection instead of storing it as a separate entity in the state.

#### Reactions
Reactions are there to handle your side effects like doing a network call, DOM updates.

Mobx is built on the following principle

> Reacting to state changes rather than acting on state changes

To implement this, it makes heavy use of observables and to make it easy for the developers, it has done lot of heavy lifting. To understand and read more on this, please refer to this [blog][mobx-post] from the creator of Mobx, [Michel Westrate][michel-westrate-twitter].

Okay. Its time now to see Mobx in action.We would be using a package called [mobx-angular][mobx-angular] which is an angular wrapper on the mobx. 

The first thing we are going to do is to create the store. Our store is going to hold the reads collection. `actionStatus` will be used to store the status of a network request.
````
@Injectable()
export class GoodReadStore {
  @observable reads: GoodRead[] = [];
  @observable actionStatus: networkRequestState;
  constructor(private backendService: BackendService) {}

````

Next is to handle our actions. For this we would be creating different methods in our Store corresponding to different actions that could be performed in our app.

````
 @action
  addNewGoodRead(read: GoodRead) {
    this.actionStatus = networkRequestState.Pending;
    this.backendService.addNewRead(read).subscribe(
      readRsp => {
        this.reads = [...this.reads, readRsp];
        this.actionStatus = networkRequestState.Success;
      },
      err => {
        console.log(err);
        this.actionStatus = networkRequestState.Failure;
      }
    );
  }
````

The code above handles adding a new good read in our app. `@action` is a decorator that indicates this is an mobx action where state mutation may happen. We are just invoking the backend service and creating a new collection with the newly added item.

Now, it time to see how to implement a derivation. Here in our app, it would be the read counter.

````
@computed
  get readsCounter(): number {
    console.log(`Reading counter @ ${Date.now()}`);
    return this.reads.filter(read => read.isRead).length;
  }
````
We just use the `@computed` decorator to make the `readsCounter` a derivation.

Also if you have noticed, the code that interacts with the service would just simply do one job of interacting with the backend APIs, thus adhering to the Single Responsibility Principle. 

````
  addNewRead(read: GoodRead) {
    const url = `${this.baseAPIRURL}/create`
    return this.http.post<GoodRead>(url, read);
  }
````

Now that we have setup the mobx infra, lets see how to use them from the components. Lets's say the user deletes a read item. All we need to do is call the delete action defined in the mobx store. The below snippet is from `home.component`.

```
  deleteItem(id: number) {
    this.store.deleteRead(id);
  }
```

Now, lets see what are the changes in `home.component.html`.

```
<div class="container"
  *mobxAutorun>
  <div class="create">
    <a routerLink="/new"
      class="btn btn-success">+New</a>
  </div>
  <div class="row">
    <div class="col-sm-4"
      *ngFor="let readItem of store.reads">
      <app-read-card [readItem]="readItem"
        (checkEvent)="toggleItemRead(readItem.id, !readItem.isRead)"
        (deleteEvent)="deleteItem(readItem.id)"
        (editEvent)="editItem(readItem)"></app-read-card>
    </div>
  </div>
</div>
```

We now are directly reading the store `reads` collection. Note that there is no `| async`, instead we are using `*mobxAutorun` directive which automatically takes care of rerendering the list if there are any changes to state. This is the magic that Mobx does out of the box for us. 

We can also mark the home component's change detection strategy to OnPush as Mobx would ensure that it would trigger the change detection whenever it sees there is a change in a state that the component would be interested in. This alleviates the developers from the responsbility of making apps more performant.

````
Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
````

That's all we need to do for Mobx implementation. The code is lot more concise, concerns separated and easy to code with lot of magic happening under the hood through Mobx. 

To summarize, we did the following:

* Created a store to hold our state
* Created actions which updates the state
* Created derivations
* Called the actions methods through the store from the Components
* Added the mobxAutorun directive to the component view (html) to automatically detect changes and rerender the DOM

You could find the complete source code for the Mobx implementation [here][mobx-github].

Mobx implmentation has so far being the most easy and consise. With lot less code, we get lot of value. But sometimes this could also backfire, if you do not understand how it all works under the hood. So please make it a point to read more about how Mobx works in this [post][mobx-post].

See you soon in the next post on Redux !! Till then Happy learning.

[part-1]: {{< ref "ui-state-management-part-1.md" >}}
[part-2]: {{< ref "ui-state-management-part-2.md" >}}
[part-3]: {{< ref "ui-state-management-part-3.md" >}}
[mobx-documentation]: https://mobx.js.org/getting-started.html
[mobx-post]: https://medium.com/@mweststrate/becoming-fully-reactive-an-in-depth-explanation-of-mobservable-55995262a254
[michel-westrate-twitter]: https://twitter.com/mweststrate
[mobx-angular]: https://github.com/mobxjs/mobx-angular
[mobx-github]: https://github.com/sundarcodes/my-good-reads-app/tree/master/frontend/angular/mobx-based-state-management/my-good-reads-app