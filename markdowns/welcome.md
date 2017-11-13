# Part 2 - CSS-in-JS and Separation of Concerns

In the previous article, I covered how BEM is used to solve the state of CSS specificity. However, JavaScript community have other Ideas and it revolves around CSS in JS. Before I start this, I will be clear even if JavaScript community hates me (mind you, I am from the same JavaScript community) but still I will stick to my viewpoint that CSS in JS can become the **worst** approach if **separation of concerns** is not taken care of. 

We’re in an era of “components”. We are building interactive UI’s based on components. We are no longer structuring entire documents, we are structuring tiny reusable pieces. All of these methodologies (SMACSS/BEM) was invented to make everything work in a component based fashion as it’s essential to use components correctly to build user interfaces. They are just tiny reusable building blocks which are getting plugged together to create your next awesome Interactive Applications. Moreover, If you are understanding those small pieces of the reusable building blocks, you can very easily understand the big picture behind it. 

![Styled Components](https://d2mxuefqeaa7sj.cloudfront.net/s_043E65AF3EF3BB37503CCB5A25DF5E327B4531B8593ACA920CB59E98A1EC0C1A_1506093701900_styled-components.png)

You don’t have millions of lines of templates in a single file instead what you have right now is those tiny pieces of building blocks that will at the end the day will be plugged together. To be very honest, we will have to give credits to things like Sass/LESS and ES6 for making this a reality. As we have built more and more things day by day, we have realized that there are some best practices around it that needs to be followed and Styled Components want to make this one step further. 

We have encapsulated building blocks, here is a **button** example with BEM Notation (with React):

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  render() {
    return (
      <div className="wrapper">
        <button className="button">My Button</button>
        <button className="button button--primary">My Primary Button</button>
      </div>
    );
  }
}
```  

But if you think about these buttons within React (or any framework for that matter), these are just an Implementation detail. No? These components don’t need to be aware of specifically which class names they need to be attached within the DOM to render the content. What Styled components in React do is to encapsulate these class names into the components and abstract them into an API. We can just render a button and attach a property called `primary` whose task is to make the button primary just like above. Here is a simple Implementation for the same

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  render() {
    return (
      <div className="wrapper">
        <button>My Button</button>
        <button primary>My Primary Button</div>
      </div>
    );
  }
}
```

In styled components, we are just having a contract with the component that if we are attaching a property to a button that is `primary`, just style them the same way you were styling in `button--primary`. Within the framework, it shouldn’t matter what that styling does but what is important is the consistency that we need to have with all our components, across all our APP UI.

By encapsulating these tiny pieces called building blocks with their behavior like this is that unless you are the guy who is making those buttons, you don’t have to understand the small pieces to build something. You don’t have to know what that button exactly does or even `primary` property for that matter in terms of styling. As a JS developer (who is not writing that button) should only know that if he is going to add a `primary` property to that `button` tag, the button will become the important button same like the in-built `disabled` property.

![](https://d2mxuefqeaa7sj.cloudfront.net/s_043E65AF3EF3BB37503CCB5A25DF5E327B4531B8593ACA920CB59E98A1EC0C1A_1505387569713_styled-components.jpg)

The thing with React (or Vue or Angular) is that you have something that’s more powerful than CSS class names and that is Components and thus the JS community wants to move **CSS-in-JS** because for a simple reason, We have JavaScript and it has all the powers to do that then why you shouldn’t use that for styling as well. Understandable! JS community feels that there isn’t any reason to be limited to an another language if JS has all those powers to do the same. 

I would agree with JS community (which includes me too) that if their boss is saying “**Do that by tomorrow: Hook or Crook**”, we will not think twice to flush those best practices that traditional methodologies provide us. Styled components are the good option regarding this because they allows enforcing best practices. Yes, it does!  

What Styled Components has done is to remove the mapping between styles and components. This is no magic but very normal stuff where you are attaching styling to a component through a variable, adding those styling with template literals and then simply rendering that HTML within the DOM. This is how a styled component looks like:

```jsx
// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// Create a Wrapper component that'll render a <section> tag with some styles
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// Use Title and Wrapper like any other React component – except they're styled!
render(
  <Wrapper>
    <Title>
      Hello World, this is my first styled component!
    </Title>
  </Wrapper>
);

// Original Source: https://www.styled-components.com/docs/basics#getting-started
```

The thing with Styled components is that there is nothing like inline Styling but instead an Actual CSS where you are teaching JavaScript to learn the CSS, it lets you add those styling, compile them and then put those styles within the `<style></style>` tag into the DOM.  Whether it’s Media Queries or Sass Nesting, everything can just happen with ease. It helps us to reduce development time by co-locating styles within the components and works ***excellently*** for encapsulating UI components and also for handling all the related concerns within a single file.

## **Downsides of Styled Components**

The biggest downside I like is that apart from `font-size`, you can also write `fontSize` within your JavaScript. This is not great because you are killing a way to declare syntactically and thus making it less portable between projects. Moreover, you will also lose the ability to cache CSS within the browser, which by then results in possibly longer loading times for your web page. There is also a possibility for styles and components that are going in different directions from easy other. You can’t ignore the fact that because it's JavaScript, whatever you will do, it will never handle those adaptive/responsive design well.

![Separation of Concerns](https://d2mxuefqeaa7sj.cloudfront.net/s_043E65AF3EF3BB37503CCB5A25DF5E327B4531B8593ACA920CB59E98A1EC0C1A_1505385060162_separation-concerns.jpg)

The thing with JSX is that HTML is the structure and JS is the behavior, and as humongous amount of JavaScript logic is tightly linked with manipulating the HTML structure, thus it’s fine to keep them together and will surely improve productivity. They are very tightly coupled with each other after all. But when it comes to graphical properties, most of the thing (if not everything) is getting manipulated by CSS and is loosely coupled with both structure, logic, behavior and even functionality (though functionality is getting more and more tightly coupled day by day).

Within your JavaScript, you want to make every component Isolate (Independent and far from each other), but if you try to do the same with your CSS, everything will just become a mess. Believe it or not but you need the so called Visual consistency within your design, and if you are moving your components far from each other, in the long term your site design will give a feeling that different designers made different pages of the site. As a designer, this is the last thing you want (even if half the site was made by some another guy before you) with your design.  

Code Reuse and Duplication is one more big concern with Styled Components. If every component by itself is responsible for their own styling then good luck. You will end up with a lot of duplicate code, and in the longer term, it will become harder to maintain day by day. 

![](https://d2mxuefqeaa7sj.cloudfront.net/s_043E65AF3EF3BB37503CCB5A25DF5E327B4531B8593ACA920CB59E98A1EC0C1A_1505385856260_let-there-be-peace-on-css-50-638.jpg)

Another concern is the Cascade, I would though argue that Cascade and its specificity headaches are the only reason why CSS methodologies are born at first place but you can’t just throw the cascade away. It’s inability to affect the children by parent classes which are core parts of Cascade. That said, for this you may come up with something that can help you do that, but I am unsure about it, especially with my current React/JS knowledge.

But the biggest concern to me is the CSS designer itself. A designer who is getting into CSS (or even experienced designers) is asking “What does he do wrong?”. You told me that in this App era, the normal CSS markup isn’t enough and you need to learn programming on your CSS, and thus I learned Sass(or LESS), but now you are telling me to never code again? 

You might be thinking that this is related to the job concern, but it’s not only about that. Believe it or not, I fear that creativity is in serious danger because of this methodologies as sometimes it’s out of the scope for many designers to learn JavaScript. A designer might not be good in maths, but that doesn’t mean that he is not bringing creativity to the table? 

## **Vue Single File Components** 

Before moving further, I can’t ignore the fact that Vue.js has done it right here and the below code makes sense to me rather. This is also **CSS-in-JS**, but I feel that this is a better way. Highly opinionated but I have used it, and I love it. This is **THE** way, and in fact, you can add Sass within this too. Moreover, you can’t ignore the fact that String templates lack syntax highlighting and Single File Components solves everything. Moreover, you aren’t killing creativity of a CSS designer who can easily work on this system without any need to learn JavaScript.

Here’s what official [docs](https://vuejs.org/v2/guide/single-file-components.html#What-About-Separation-of-Concerns) says:

> **What About Separation of Concerns?**
> One important thing to note is that **separation of concerns is not equal to separation of file types.** In modern UI development, we have found that instead of dividing the codebase into three huge layers that interweaves with one another, it makes much more sense to divide them into loosely-coupled components and compose them. Inside a component, its template, logic and styles are inherently coupled, and collocating them actually makes the component more cohesive and maintainable.

```vue
<template>
  <p> {{ greeting }} World </p>
</template>

<script>
  module.exports = {
    data() {
      return {
        greeting: 'Hello'
      }
    }
  }
</script>

<style scoped>
  p {
    font-size: 2em;
    text-align: center;
  }
</style>

<!-- Source: https://vuejs.org/images/vue-component.png -->
```   

Though I am serious about separating files but honestly I generally don’t get bothered by Vue method much and use it as is. That said you can simply do:

```vue
<template>
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css"></style>
```

You can still leverage Vue’s hot-reloading and pre-compilation features by separating your JavaScript and CSS into separate files.

## **Further Reading**

  - http://cssinjs.org/
  - https://www.styled-components.com/
  
## **Wrapping Up**

Hope you liked my viewpoint on CSS in JS, especially . This is just the second part of the story, next up I will cover up new boy in the mix, ECSS which according to me (yes, opinionated) is better approach then both BEM or CSSinJS.

**Thanks a lot**, If you liked my article and also my passion for teaching and want to say hello… my twitter handle is [@harmanmanchanda](https://bit.ly/tw-harry). My DM’s are open to the public so just hit me up.
