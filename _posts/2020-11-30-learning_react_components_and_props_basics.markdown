---
layout: post
title:      "Learning React: Components and Props Basics"
date:       2020-11-30 16:07:44 +0000
permalink:  learning_react_components_and_props_basics
---



Components can be compared to JS functions. They can take inputs (props) and return react elements that will be displayed on the web. Props or properties is a JS object with attributes data needed by the component.

<h3 style="color:blue">Function Component Example</h3>

![](https://miro.medium.com/max/755/1*aVENiwshEceUfXD4AYBfoA.png)


<h3 style="color:blue">Class Components Example (ES6)</h3>

![](https://miro.medium.com/max/824/1*obYxiZmFdY2EbJdOOb3xiw.png)


From React point of view, these two components are equivalent.



  <span style="color:red">// Note: **Always start component names with a capital letter.** </span>

Props are read only inputs and should never be modified by the components. Components act like pure functions (function which output is predictable because they never attempt to modify the inputs and always return the same result given the same inputs).


<h3 style="color:blue">Rendering a Component</h3>

A component can be rendered like a react element (after all it is composed of elements).

![](https://miro.medium.com/max/674/1*HVzgcvKwnS2sM_jKd1LqBw.png)

A component can also be rendered in another component.

![](https://miro.medium.com/max/837/1*azOAjj6NTr5vZBB05xrblA.png)

That’s all I have for the basics of React Component.

You can always learn more on <a href='https://reactjs.org/docs/react-component.html'>react doc page</a>



Until next time,

Keep up the momentum
