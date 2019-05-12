---
layout: post
title:      "React and Redux - Summary of What I've learned "
date:       2019-05-12 16:18:10 -0400
permalink:  react_and_redux_-_summary_of_what_ive_learned
---

TLDR; 
-  React is a Javascript library for building user interfaces
-  React divides UI down to pieces of code called components, which take props and have a state
-  Redux is library for application state management, takes React local states and put them in one store
-  Redux has a store, actions, and reducers, and it separates data management from the UI
-  Redux has many benefits when building complex applications, however for smaller ones it's less needed

_____________________________________________________________________________

Adding React and Redux to my final project was something I struggled with a lot. This lead to a lot of re-reading Flatiron School Curriculum, and googling. In this blogpost I will define some of the key takeaways I have learned about React, Redux, and Redux thunk.

**React Basics**
React is a Javascript Library for building user interfaces (UI). In essence, React lets you create advanced UIs through small and isolated pieces of code, refered to as *components*. Components are used to tell React what to display on the screen.

The most common type of components are `class components`. They take in parameters, which are refered to as `props`, and then returns a views to display with the render method.

Another important object in React are `states`. States within component are quite similar to variables within functions, and they are used by components to keep track of information. In 'vanilla' React, component state is stored locally within a component and are not accessible by other components unless explicitly told so (passed as props). 

However, when the number of components increases, passing props back and forward becomes more and more tricky. This is when react comes in. 

**Redux Basics**
Redux is a library for application state management. That is, it can be used to manage React componenets 'states'. In essence, Redux state is a centralised global store that is accessible to any component that has imported it. Redux change the state of components through `dispatch`, actions and reducers. 

*Dispatch* is a function of the Redux store and we call `store.dispatch` to dispatch an action. This is the only way to trigger a state change. By declaring a `mapDispatchToProps` constant it allows us to specify which actions our compmonent might need to dispatch. It lets us provide action dispatching function as props.

*Actions* are payloads of data that get sent from the application to the store, and as mentioned above, we send them using `store.dispatch()`. In other words, actions are sent when a user interacts with the UI, such as clicking a button.

*Reducers* are what listens for action. They update the state when they hear that an action has been sent.

*The Store* is the middleman between actions and reducers, and it holds the Redux state and allows access and modifications to it. 

**Pros of React + Redux**

1. Maintainability - with a central store, any component can access any state within it. There no need of passing props back and forth.
2. Easy testing - since the data and the UI are seperate, it allows for testing easily. 
3. Prevents unnessecary re-rendering - beacause when the state changes in Redux, it returns a new state that uses a shallow copy. 
4. Unmonted components still have a state - a centralised store persists the state of a component even efter it has unmounted.

**Cons of React + Redux**
1. Exessive memory use - since a state is immutable, and each state change returns a shallow copy, it can lead to more memory being used than necessary.
2. Restricted design - when implementing Redux, there is only one way to do it. Therefore, it's less flexible than other way of managing states. 


**Conclusion**
Redux is a great framework for more complex applications. For my final project, the pros of implementing Redux isn't that significant, since the project is fairly light weight. However, there are definitely some advantages to seperating the UI and data handling when making bigger ones. 



