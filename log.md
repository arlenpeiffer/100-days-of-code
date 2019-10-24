# 100 Days Of Code - Log

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
