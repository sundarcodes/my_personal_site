---
title: "State Management in Angular 2+ apps - Part 5"
date: 2018-01-28
tags: ["State Management", "Redux", "ngRx", "Angular 2+"]
draft: false
---

In this post of our series in state management, we will be seeing how we can solve our state problem using a solution called Redux. We will be use the angular variant of Redux called [ngRx][ngrx].

Redux has been inspired by [elm][elm-tutorial] and [Flux][flux]. Elm is a pure functional programming language used for developing the front end of modern day web apps. Flux is a stage management solution. If this is first time you are hearing about Redux, please do watch [these][dan-abramov-egghead] videos from the co-creator of Redux, [Dan Abramov][dan-abramov-twitter].

Redux works on 3 core concepts, lets try to understand each of them in the context of our goodreads app.

## Action

Actions are events that could update the state of your application. This could be user generated events like mouse clicks, keypress, scroll etc, network events like XHR request, response, websockets or timer events like setTimeout() and setInterval(). In the context of our app, the actions could be clicking on a good read tile, creating a new good read, deleting a good read etc..

## Store

This is the place where the entire state of your application is maintained. Simply put it is one big JavaScript object (sometimes nested) which is going to represent the state across all pages in our application. In the context of our app, the store is just going to be a list(or array) of good reads.

## Reducer

Reducers are pure functions which takes 2 parameter namely the current state reference and the action type as input and returns a new state based on the action that had happened. In the context of our app, when we want to delete a good read, the current state would have original list of good reads but the reducer function would return a new state with the deleted item removed from the array as it was a delete action.

## Effects

Though reducers are the place where most of your business logic may reside, but we cannot do any thing that may cause side effects inside them - which means we cannot do a network request to fetch some data from backend. But we cannot build apps which doesn't have any network requests. This is where the effects come into play. It is the effects which handles codes that produces side effects and convert them to actions which gets sent to the reducer to yield a new state.

I know it's a bit vague now. Let's look at some code to get a better picture. The code below uses ngRx version 2. Please note that the latest version of ngRx has moved into a mono repo and has a slightly different API. However, the concept remains the same.

First, we would be listing down all the actions in our app that could possiblly change the state in a `actions.ts` file.

```
    static FETCH_ALL_READS = 'FETCH_ALL_READS';
    static FETCH_ALL_READS_SUCCESS = 'FETCH_ALL_READS_SUCCESS';
    static FETCH_ALL_READS_FAILURE = 'FETCH_ALL_READS_FAILURE';

    static MARK_AS_READ = 'MARK_AS_READ';
    static MARK_AS_READ_SUCCESS = 'MARK_AS_READ_SUCCESS';
    static MARK_AS_READ_FAILURE = 'MARK_AS_READ_FAILURE';

    static MARK_AS_UNREAD = 'MARK_AS_UNREAD';
    static MARK_AS_UNREAD_SUCCESS = 'MARK_AS_UNREAD_SUCCESS';
    static MARK_AS_UNREAD_FAILURE = 'MARK_AS_UNREAD_FAILURE';

    static ADD_NEW_READ = 'ADD_NEW_READ';
    static ADD_NEW_READ_SUCCESS = 'ADD_NEW_READ_SUCCESS';
    static ADD_NEW_READ_FAILURE = 'ADD_NEW_READ_FAILURE';

    static EDIT_READ = 'EDIT_READ';
    static EDIT_READ_SUCCESS = 'EDIT_READ_SUCCESS';
    static EDIT_READ_FAILURE = 'EDIT_READ_FAILURE';

    static DELETE_READ = 'DELETE_READ';
    static DELETE_READ_SUCCESS = 'DELETE_READ_SUCCESS';
    static DELETE_READ_FAILURE = 'DELETE_READ_FAILURE';
```

Then we define the state of our application and we also initialize it.

```
export interface GoodReadState {
    readsCollection: GoodRead[];
    operationMsg: string;
}

export const initialState: GoodReadState =  {
    readsCollection: null,
    operationMsg: null
}
```

Then we define our reducer which is just a pure function and returns a new state, given the current state and an action.
I have used the lodash cloneDeep function to create a new instance of the state and update it. I could have also used ES6 features to achieve the same thing. Remember pure functions should not alter the input passed and also should not cause any side effects.

```
export function reducer(state: AppState = initialState, { type, payload }: Action) {
    let newState: AppState;
    let findIndexToReplace;
    switch (type) {
        case GoodReadActions.FETCH_ALL_READS_SUCCESS:
            newState = _.cloneDeep(state);
            newState.readsCollection = payload;
            break;
        case GoodReadActions.FETCH_ALL_READS_FAILURE:
            newState = state;
            newState.operationMsg = 'Fetching reads from backend failed';
            break;
        case GoodReadActions.LOAD_NEW_READ:
            newState = state;
            newState.selectedRead = null;
            break;
        case GoodReadActions.ADD_NEW_READ_SUCCESS:
            newState = _.cloneDeep(state);
            newState.readsCollection.push(payload);
            newState.selectedRead = null;
            break;
        case GoodReadActions.LOAD_READ:
            newState = state;
            newState.selectedRead = _.cloneDeep(payload);
            break;
        case GoodReadActions.EDIT_READ_SUCCESS:
            newState = _.cloneDeep(state);
            findIndexToReplace = _.findIndex(newState.readsCollection, (read) => read.id === payload.id);
            newState.readsCollection[findIndexToReplace] = payload;
            break;
        case GoodReadActions.MARK_AS_READ_SUCCESS:
            newState = _.cloneDeep(state);
            findIndexToReplace = _.findIndex(newState.readsCollection, (read) => read.id === payload.id);
            newState.readsCollection[findIndexToReplace] = payload;
            break;
        case GoodReadActions.MARK_AS_UNREAD_SUCCESS:
            newState = _.cloneDeep(state);
            findIndexToReplace = _.findIndex(newState.readsCollection, (read) => read.id === payload.id);
            newState.readsCollection[findIndexToReplace] = payload;
            break;
        default:
            newState = state;

    }
    return newState;
}
```

And finally to handle our network request, we need to wire up the effects part. The effects are going to intercept the actions that needs a call to backend, does the actual call through a service and finally dispatches an action with the resultant data or server response.

```
@Injectable()
export class GoodReadEffects {

    constructor(private action$: Actions, private backendService: BackendService,
    private goodReadActions: GoodReadActions) {

    }

    @Effect()
    fetchAllReads$ = this.action$
    .ofType(GoodReadActions.FETCH_ALL_READS)
    .switchMapTo(this.backendService.fetchAllReads())
    .map((reads: GoodRead[]) => this.goodReadActions.fetchAllReadsSuccess(reads));

    @Effect()
    markItemAsRead$ = this.action$
    .ofType(GoodReadActions.MARK_AS_READ)
    .switchMap((action: Action) => this.backendService.markItem(action.payload, true))
    .map((read: GoodRead) => this.goodReadActions.markAsReadSuccess(read));

    @Effect()
    markItemAsUnRead$ = this.action$
    .ofType(GoodReadActions.MARK_AS_UNREAD)
    .switchMap((action: Action) => this.backendService.markItem(action.payload, false))
    .map((read: GoodRead) => this.goodReadActions.markAsUnreadSuccess(read));

    @Effect()
    editRead$ = this.action$
    .ofType(GoodReadActions.EDIT_READ)
    .switchMap((action: Action) => this.backendService.editItem(action.payload))
    .map((read: GoodRead) => this.goodReadActions.editReadSuccess(read));

    @Effect()
    addRead$ = this.action$
    .ofType(GoodReadActions.ADD_NEW_READ)
    .switchMap((action: Action) => this.backendService.addNewRead(action.payload))
    .map((read: GoodRead) => this.goodReadActions.addNewReadSuccess(read));
}
```

Now that we have tough part out, lets look at how we are going to utilize this redux setup. Let's assume the user clicks on the read checkbox. Below is the code in the `home.component.ts` that handles this functionality. It just simply dispatches an action to mark a particular item as read/not read. This would be handled in the reducer and

```
  toggleItemRead(id: number, isRead: boolean) {
    if (isRead) {
      this.store.dispatch(this.goodReadActions.markAsRead(id))
    } else {
      this.store.dispatch(this.goodReadActions.markAsUnread(id))
    }
}
```

Now that we have seen how to dispatch/trigger the action through the store, we need to know how the view/DOM gets updated once there is a change in the state. For this we hook up an observable to the store. Here below is the code in `home.component.ts` where the `allReads$` variable is hooked on to the state's readCollection property.

```
ngOnInit() {
    // this.allReads$ = this.store.select(selectAllReads);
    this.allReads$ = this.store.select((state: AppState) => state.readsCollection)
}
```

Any changes to the readCollection would trigger a re-render to the DOM as shown below.

```
 <div class="col-sm-4"
      *ngFor="let readItem of allReads$ | async">
      <app-read-card [readItem]="readItem"
        (checkEvent)="toggleItemRead(readItem.id, !readItem.isRead)"
        (deleteEvent)="deleteItem(readItem.id)"
        (editEvent)="editItem(readItem)"></app-read-card>
</div>
```

This is how we setup Redux to our Angular project. We have done lot of things/wiring up to get it working.
To summarize, we did the following:

- Created a set of actions which are going to modify the state
- Created an interface for the store
- Created an reducer function to handle state changes on various actions
- Created a set of effects to handle side effects and transform them to actions
- Hooked up the store in the components to observe for state changes and also to trigger/dispatch actions based on various events

You could find the complete source code for the Redux implementation [here][redux-github].

See you soon in the last post on this series where we conclude on what state management solution to choose based on the needs !! Till then Happy learning.

[part-1]: {{< ref "ui-state-management-part-1.md" >}}
[part-2]: {{< ref "ui-state-management-part-2.md" >}}
[part-3]: {{< ref "ui-state-management-part-3.md" >}}
[part-4]: {{< ref "ui-state-management-part-4.md" >}}

[ngrx]: https://github.com/ngrx/platform
[redux-github]: https://github.com/sundarcodes/my-good-reads-app/tree/master/frontend/angular/redux-based-state-management/my-good-reads-app
[dan-abramov-egghead]: https://egghead.io/series/getting-started-with-redux
[dan-abramov-twitter]: https://twitter.com/dan_abramov
[elm-tutorial]: http://elm-lang.org/
[flux]: https://facebook.github.io/flux/docs/overview.html
