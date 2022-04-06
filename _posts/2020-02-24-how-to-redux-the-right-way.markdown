---
layout: post
title: How To Redux The Right Way
date: 2020-02-24 13:32:20 +0300
description: Tutorial on the basics of Redux with a working example on how to build a to-do list. # Add post description (optional)
img: redux.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Redux, React, JavaScript, Software]
---

Redux is not for every web application, there are certain use cases where it actually makes more sense. For example, imagine you built a small UI and keeping all your state in a top-level component is no longer sufficient. Other reasons why to choose Redux are the need for a single source of truth for your state as well as reasonable amounts of data changing over time.

### What is Redux?

>Redux is a predictable state container for JavaScript applications. [source][redux]

The predictability aspect comes from the fact that the state of the whole application is stored as a tree of plain objects and arrays within a single store. State is read-only and the only way to change the state is to dispatch an action, a plain object describing what happened. All state updates should be made with pure functions called reducers.

Redux's core concepts:
* State - is stored in plain objects. There's no "setters", so that different parts of the code can’t change the state arbitrarily. 
* Actions – used to change something in the state, you need to dispatch an action. An action is a plain JavaScript object that describes what happened.
* Action Creators – encapsulate the process of creating action objects. 
* Reducers - where all the state update logic lives. The functions should have no side effects, but rely entirely on inputs, not affecting anything external. Reducers need to update data immutably.

Example of actions which add or toggle a book:

{% highlight ruby %}
let nextBookId = 0
export const addBook = text => ({
  type: 'ADD_BOOK',
  id: nextBookId++,
  text
})

export const toggleBook = id => ({
  type: 'TOGGLE_BOOK',
  id
})
{% endhighlight %}

The reducer responsible with adding/toggling books on a to-do list looks like this:

{% highlight ruby %}
const books = (state = [], action) => {
    switch (action.type) {
      case 'ADD_BOOK':
        return [
          ...state,
          {
            id: action.id,
            title: action.text,
            completed: false
          }
        ]
      case 'TOGGLE_BOOK':
        return state.map(book =>
          book.id === action.id ? { ...book, completed: !book.completed } : book
        )
      default:
        return state
    }
}
export default books
{% endhighlight %}

### Why do you need Redux?

Redux is very often used with [React][react] (a JavaScript library created by Facebook in 2013). React lets you compose complex UIs from small and isolated pieces of code called “components”. The parent component is responsible for their children’s data needs. Hence, the state is passed down to the children by using props in order to keep the children in sync with each other and the parent. However, React has its limitations. It does do well at state transferring between the components as there is only a one-way data flow. Most of the times unnecessary data ends up being transferred to the components. 

![Redux State Flow]({{site.baseurl}}/assets/img/redux-state-flow.png)

In this situation, Redux saves the day as it solves the state transfer problem. In this way, each component can directly access the store. Since the store is the single source of truth, the code becomes much readable and easier to understand as does the access and transfer of the state between the components.

### What inspired Redux?

In the Model View Controller Pattern, the constant updates between models and views (two-way data bindings system) can lead to cascading updates. This makes it difficult to predict what would change as a result of one user interaction. In this situation it would be best if the data updates would happen in a single round. 

Another source of inspiration for Redux was the [Flux Pattern][flux] for building UIs. 

![Flux]({{site.baseurl}}/assets/img/flux.png)

Both of the problems mentioned previously could be solved by Redux's unidirectional data flow. In the example below, a user performs the action of depositing $10, hence an action is "dispatched". Then the reducer function is called with the current state and the dispatched action as parameters, immutably updates the state by returning a new state object. The store notifies subscribers of the update by running their callback functions. Finally, subscribers (components) can retrieve the updated state and re-render.

![Simple Redux]({{site.baseurl}}/assets/img/simple-redux.gif)

### Advanced Topics

In case you want to add logging to the application, there are some extra steps which need to be performed such as adding a custom middleware when creating the store. Apart from logs, middlewares can be used to set timeouts, make asynchronous API calls, modify, pause or even stop an action entirely. In fact, a middleware is intended to contain logic with side effects. Plus, middlewares can modify "dispatch" to accept things that are not plain action objects.

![Advanced Redux]({{site.baseurl}}/assets/img/advanced-redux.gif)

### Conclusion

Although Redux is a very powerful framework, the entire state of an application can be now managed using [React's Context][react-context]. The Context allows passing the data through the component tree without having to pass props down manually at every level (hence breaking the one directional flow). There might be better tools out there for keeping the single-source-of-truth on the server, more likely to be persisted in a database, hence removing the need for maintaining and synchronizing server derived state with Global State [according to Kolby here][kolby-article]. 

### Github Repo

[React and Redux Example 101][redux-example-101]

[redux]: https://redux.js.org/
[react]: https://reactjs.org/
[flux]: https://facebook.github.io/flux/docs/in-depth-overview/
[react-context]: https://reactjs.org/docs/context.html
[kolby-article]: https://engineering.udacity.com/react-state-management-in-2022-return-of-the-redux-87218f56486b
[redux-example-101]: https://github.com/andreeaionescu/redux-example-101