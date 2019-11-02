# 100 Days Of Code - Log

### Day 15
**Friday, November 1, 2019**

Oof. I feel so wiped from working on stuff all day. Seriously spent the whole day coding today. Made a lot of progress on the React Native project but also spent so many hours grappling with some silly things that hopefully will wind up sticking with me. Here's a little list of the day's accomplishments:

- Wrapped up/commited refactoring work from yesterday having to do with the object restructure/section key stuff
- Added ternary that swaps the section header icon when collapsed vs. expanded
- Refactors and styles the Checklist screen (spent a couple hours really digging into styled-components stuff this morning)
- And the big finale for the day was deciding to go in the other direction with the application global state (order) and change it from an object to an array, and to use useReducer/dispatch calls to manage changes instead of useState/setOrder. This was another good exercise but took me way too long. I wanna get better at figuring out how to break these things down faster and not feel like i'm stumbling through them for hours.

And some take aways:

- When using ``import * as types from './types'`` turns out you actually have to make sure to export the variables in types.js.. whoops!
- Can anyone write nice looking reducer files or do they all just look crazy?
- Don't forget, that property you're trying to access might be on the action object 🙃
- It's seriously so rewarding figuring these problems out. Keep it up buddy.


### Day 14
**Thursday, October 31, 2019**

Got to work on things for a couple hours after work today. Felt lots better than yesterday thank goodness.

Made some structural changes to the order object in Restock. Ended up going with the idea of creating individual section keys that then contain an array of all of the items and amounts for that section/category. The function for updating the order object became much more complex when introducing the extra section key layer, but was a really good exercise for me. Here are the notes I took to help me visualize what I needed the function to do..

```
DOES SECTION KEY EXIST?
-- YES: IS AMOUNT > 0?
  -- YES: ADD ITEM KEY!
  -- NO: IS THIS THE LAST ITEM IN THE SECTION?
    -- YES: REMOVE SECTION KEY!
    -- NO: REMOVE ITEM KEY!
-- NO: ADD SECTION KEY!
```

A few other fun things I learned/solidified today:
- Object dot notation ( `object.property` ) requires that property is a valid JavaScript identifier
	- An **identifier** is a sequence of characters that identifies a variable, function, or property. In JavaScript, identifiers are case-sensitive and can contain Unicode letters, $, _, and digits (0-9), but may not start with a digit.
- Object bracket notation ( `object[property]` ) does not require property to be a valid identifier — it can have any value, e.g. "1foo", "!bar!", or even " "
- To remove the last letter of a string you can use `str.slice(0, -1)`

Links:
- [Objects](https://javascript.info/object)
- [Property accessors - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors)
- [Identifier - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/identifier)
- [String.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

### Day 13
**Wednesday, October 30, 2019**

Made a few commits today on Restock.. got the most basic version of the Checklist screen happening and rendering ChecklistItem components for each item in the order. After that explored some other options for data structure (seeing if might make more sense to store the order data as an array rather than an object, and also if it might be worth it to store items on a section object so that items can be organized by section/headers can be rendered on the Checklist screen for each group of items).

Don't know why but this second part of the day felt really tough. Was one of those days where I felt like I didn't know anything about JavaScript, or like I'd never seen or heard of any of the built-in Object or Array methods. Starting to feel kinda under the weather so maybe it has something to do with that. 🤒

Either way, better luck tomorrow

Ciao!


### Day 12
**Tuesday, October 29, 2019**

Made a bunch of commits for the project today! Merged all of my changes from the last few days — completing my work on the section feature for now — and improved the function managing the app's global order state. (It now removes a key from the order object if the amount for an item is 0).

Also merged a bunch of automated pull requests this morning for a handful of github repos with high severity security vulnerabilities. Was my first time merging pull requests that weren't just me merging a branch, so that felt cool.

My interview for 8th Light is tomorrow, so spent a little time getting ready for that as well!

**A useful link from the day:**
- [Color Name & Hue – Colblindor](http://www.color-blindness.com/color-name-hue/) — *great tool for looking up the specific name of colors*


### Day 11
**Monday, October 28, 2019**

Didn't have a ton of time to work on things today but did get to play around with styled-compnents for an hour or so. Got the counter component looking/feeling a little better and was able to get rid of some unnecessary css.

A few questions/thoughts from the day:
- Getting used to the the fact that the default value for ``flex-direction`` in React Native is ``column`` and not ``row`` like it is for web stuff..
- In React Native, do you have to set ``display: flex`` in order to use ``align-items``/``justify-content`` or can you just use them?

### Day 10
**Sunday, October 27, 2019**

Had the day off today and got to spend most of it coding. Feel like I covered a lot of ground.

This morning I worked on getting Context all set up for the Restock app. Renamed some things for clarity (AppContext becomes OrderContext, count gets swapped out for amount) and replaced instances of Consumer with useContext calls. I think the changes made things much cleaner and all seems to be working pretty well.

After that I got to learn about React Native's ScrollView, how styling a ScrollView with styled-components requires a slightly different syntax, and how flex in React Native differs from flex in the browser. Links below.

Lastly, spent a couple hours working on setting up and styling a Counter component that manages the current amount for each SectionItem. Was fun playing around with dynamic styling in styled-components 💅

Here are a few links from the day:
- [Taming React Native’s ScrollView with flex - Peter Piekarczyk - Medium](https://medium.com/@peterpme/taming-react-natives-scrollview-with-flex-144e6ff76c08)
- [Layout Props · React Native](https://facebook.github.io/react-native/docs/layout-props.html#flex)
- [wesbos/css-colours-sorted: CSS Colours Sorted By](https://github.com/wesbos/css-colours-sorted)


### Day 9
**Saturday, October 26, 2019**

Spent a few hours this morning playing around with Restock and trying to think through the best way to handle sharing state between the Form and Checklist screens. Looking into params in React Navigation and Context in React. Thinking it might be Context, but we'll see what tomorrow brings.

Also, realized today that I could for sure spend some more time committing to memory the built-in Array methods and how they work/what they return.

Links:
- [Passing parameters to routes · React Navigation](https://reactnavigation.org/docs/en/params.html)
- [Context – React](https://reactjs.org/docs/context.html)
- [Array - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)


### Day 8
**Friday, October 25, 2019**

Spent today playing around with Restock some more, mostly just continuing from where I left off yesterday. Sorted out what actually needs to be included for the babel-plugin-module-resolver to work (seems like just an alias prop containing an object with the paths you'd like to create aliases for), so feeling pretty comfortable/confident with that now. Got to use ``Shift + R`` a fair amount while getting all my new paths set up 😉

Also got my two views/screens set up (Form & Checklist) and basic navigation between the two is working. Currently that's being done by using the navigate function on the navigation prop in React Navigate. Lots of that word today. A note on the navigation prop..

- Each ``screen`` component in your app is provided with the ``navigation`` prop automatically. The prop contains various convenience functions that dispatch navigation actions on the route's router. https://reactnavigation.org/docs/en/navigation-prop.html

Beyond that just started setting up a Section component to be used inside of Form. Each Section will manage it's own expanded/collapsed state, which in turn will decide whether any of the items belonging to that section are rendered. Got the basics working but excited to keep exploring.

Next steps (or maybe not next, but steps to take at some point soon):
- add tests to the project
- try adding animations for expanding/collapsing menus
- work on taking more breaks

Alright, see ya tomorrow!


### Day 7
**Thursday, October 24, 2019**

Started setting up the basic file structure and navigation for Restock app. Got a little sidetracked by wanting to use babel-plugin-module-resolver for absolute pathnames in my imports, so didn't end up getting as far as I wanted to with the set up process. Did eventually figure out the absolute pathname thing (turns out ``Shift + R`` clears the cache and forces a reload of the babel.config.js file in Expo) but was definitely a little fried by the time I got there.

Here are some notes I took on setting up an Expo project with React Navigation:

**Initializing an Expo project**
- From the directory where you’d like to create a project run
	``expo init project-name`` or simply ``expo init``
- Choose a template/workflow:
    * blank — a minimal app as clean as an empty canvas
    * blank (TypeScript) — same as blank but with TypeScript configuration
    * tabs — several example screens and tabs using react-navigation
    * minimal — bare and minimal, just the essentials to get you started
    * minimal (TypeScript) — same as minimal but with TypeScript configuration
- Enter a name (the name of your app visible on the home screen) and a slug (a url friendly name for your app — this will also be the name of the project folder if none was specified when running ``expo init``)
- Choose whether or not to use yarn to install dependencies

**Starting the project**
- From the project directory run either ``yarn start`` or ``expo start``

**Adding react-naviagation**
- From the project directory run ``expo install react-navigation react-native-gesture-handler react-native-reanimated react-native-screens``

**Setting up react-navigation**

React Navigation's stack navigator provides a way for your app to transition between screens and manage navigation history. To use React Navigation’s stack navigator:
- ``yarn add react-navigation-stack``
- ``import { createStackNavigator } from ‘react-navigation-stack’``
- ``import { createAppContainer } from ‘react-navigation’``

**createStackNaviagtor** is a function that returns a React component. It takes a route configuration object and, optionally, an options object.

**createAppContainer** is a function that returns a React component to take as a parameter the React component created by the createStackNavigator, and can be directly exported from App.js to be used as our App's root component. *(???)*

And lasty...

A question from the day...

**Q:** Do components in React Native have to be class components?

Find out next time on... #100DaysOfCode 😜


### Day 6
**Wednesday, October 23, 2019**

Today was a really fun day of exploring. I got to spend pretty much the entire day working on coding, and I feel really proud of where I'm at today. There were some definite moments throughout the day that I noticed a new level of comfort — an almost second nature kind of thing — with certain tasks (using nvm, setting up new repos on github and sourcetree) and a collectedness that I don't always feel when running into issues or error messages. Definitely feeling back on track a little more after today.

Here are the day's takeaways..

- Started out the day by doing some reading/exploring about React Native (been thinking about building my next project with that) and through that exploration found Expo, which seems really cool. Spent a good part of the day playing around with the Expo CLI/DevTools and Expo Client for iOS. Ran into lots of new errors I've never seen before, but worked through all of them and everything seems to be up and running.
    - https://expo.io
    - https://github.com/expo/expo
    - https://github.com/expo/expo-cli
    - https://blog.expo.io/expo-cli-2-0-released-a7a9c250e99c
    
- Learned about using absolute paths as opposed to relative paths for imports (ex: ``import Header from 'components/Header'`` instead of ``import Header from '../../components/Header``) using babel-plugin-module-resolver.
    - https://github.com/tleunen/babel-plugin-module-resolver

- Brushed up on some terminal commands for nvm and learned how to set the default version of node by using ``nvm alias default 12.13.0``.

- Did a short React Native/Expo tutorial where we built a little Netflix clone. Apparently some breaking changes with react-navigation (a package used in the tutorial) have occurred since it was written, so got to do some doc reading/problem solving to get everything working as expected.
    - https://codeburst.io/getting-started-with-expo-react-native-and-styled-components-using-a-netflix-clone-example-652c7cb2a794
    
- Lastly, found out installing Xcode takes a really long time (but that the iOS simulator built into it is pretty fun) 📱

Oh, and one last thing.. found out I got a phone interview with 8th Light! 😄

Quite a day. Excited to keep at it. See ya tomorrow!


### Day 5
**Tuesday, October 22, 2019**

Ladies and gentlemen, he's finally done it.. he finally reached the end of The Complete React Developer Course 🎉

The last few lessons in the course focused on the useReducer Hook, Context and the useContext Hook, Fragments, and creating custom React Hooks. Here are a few take aways..

``useReducer``
- awfully similar to the idea of reducers in Redux 🤔
- takes (2) arguments ➡️ a reducer function and the initial state
- returns an array containing (2) things ➡️ state and a dispatch function
- ultimately allows us to remove complex state logic from our component and store it in a separate function, making our component easier to manage and making the reducer easier to reuse

``useContext``
- takes (1) argument ➡️ a context object
- returns ➡️ the value being shared via Context (ie. whatever was passed as the value prop to Provider)
- you can destructure just the things you need from the return value of useContext like so: ``const { notes } = useContext(NotesContext);``

Also, got a little refresher on event listeners during the lesson on creating custom hooks.. ``document.addEventListener()`` and ``document.removeEventListener()`` both take (2) arguments ➡️ the event to listen for and the function to run when the event occurs. I gotta go back and spend a little time playing with event listeners again, as well as explore what other fun things can be done with event targets like document and window.


### Day 4
**Monday, October 21, 2019**

Completed 3 lessons on useEffect. Turns out it's pretty 🆒. Here are some take aways..

- useEffect is kind of like a replacement for lifecycle methods in our class-based components
- useEffect takes a function as its first argument
- that function runs once right away (similar to ``componentDidMount``) and again any time component state or props change (similar to ``componentDidUpdate``) unless an optional second argument is provided
- the optional second argument is an array of values (called dependencies) that specify which prop or state changes useEffect should care about or respond to
- if an empty array is provided as the second argument, useEffect will run just once when the component mounts (useful for fetching or reading data)
- useEffect can also mimic ``componentDidUnmount`` by returning a function that will run if the component instance is removed from the DOM

Other take aways:
- TIL you can dynamically change the title of a page by setting ``document.title`` equal to a variable 🤯
- just a friendly reminder that you gotta use JSON.stringify and JSON.parse when working with localStorage..

Some more reading on useEffect:
- [A Complete Guide to useEffect — Overreacted](https://overreacted.io/a-complete-guide-to-useeffect/)
- [Hooks API Reference – React](https://reactjs.org/docs/hooks-reference.html#useeffect)


### Day 3
**Sunday, October 20, 2019**

Completed Section 18 and started on Section 19 of the Complete React Developer Course. In Section 18 I got practice gutting a ‘finished’ project to create a boilerplate repo, wiping the git history ( ``rm -rf .git`` ), initializing a new git repository ( ``git init`` ), and making an initial commit ( ``git add .`` followed by ``git commit`` ). Section 19 starts off by talking about create-react-app in depth and then moves into looking at hooks. Completed 3 lessons on the useState hook which served as a good review. Excited for tomorrow to start digging into some hooks I have a little less experience with (useEffect, useReducer, useContext). Ciao!


### Day 2
**Saturday, October 19, 2019**

Completed the rest of Section 17  of the Complete React Developer Course! Got some good practice working with media queries/breakpoints, creating & importing partial files/using selectors to target specific elements or decendants of specific elements, and setting up variables for reusing colors, font sizes/styles, and margin/padding amounts.

Also had a good refresher on the difference between inline and inline-block..

**display: inline**
- **cannot** set width and height of an element
- top & bottom margins are **not** respected

**display: inline-block**
- can set width and height of an element
- top & bottom margins are respected

Deployed version of the Expensify app with added styles is up at https://e-x-pensify.herokuapp.com/


### Day 1
**Friday, October 18, 2019**

Picked back up with Section 17 of Andrew Mead's Complete React Developer Course. Completed the first two videos of the section which detailed the process of refactoring and adding styles to LoginPage and Header. It was fun seeing Sass again (maybe it's been close to a year?) and trying to remember some of the concepts (partials, nested selectors, variables, BEM).

Also had to solve an error being thrown by Firebase due to missing .env files (sadly lost in the Great Hard Drive Crash of September 2019 😪). Had everything back up and running in just a handful of minutes, which I was really proud of.
