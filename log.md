# 100 Days Of Code - Log

### Day 23
**Saturday, November 9, 2019**

Starting to feel the burn of doing this every day with no breaks. Also starting to feel a craving for a little more structure/guidance. Decided to try picking back up with some lessons on freeCodeCamp in response to that craving, but rather than jump back into the HTML/CSS stuff I had started at the beginning of the year thought it might be more beneficial to start with the JavaScript Algorithms and Data Structures Certification. Blew through 67 of the 110 lessons in the Basic JavaScript section today, and took some notes while I did it to help cement some of the early concepts and to remind me of a few things I had forgotten.


### Day 22
**Friday, November 8, 2019**

Holy cow, what a day. Made a snap decision late last night to buy a domain name ( [arlenpeiffer.io](https://arlenpeiffer.io) ) and set up a portfolio site for myself.

Spent the morning today reading up on how to get everything set up for the new domain.. figured out hosting (went with Netlify) and learned about SSL certificates (stands for Secure Sockets Layer and it's what https uses to make a secure connection), DNS (Domain Name Server), CDNs (Content Delivery Networks), and nameservers (the portion of your DNS records that enable people to use your domain name rather than a complex IP address to access your site).

After I decided to go with Netlify for hosting and got all of the nameservers switched over for that, I started to read up on Gatsby (I think that's what I'd like to try using to make my portfolio). Set up a new repo and played around for a while but ended up deciding to make the little landing page for the site with CRA. Think i need a little more time to play around with and understand the Gatsby world..

Here are a few notes I took this morning about Gatsby and the Gatsby CLI:

- **install Gatsby CLI:** ``npm install -g gatsby-cli``

- **create new site:** ``gatsby new arlenpeiffer.io``

- **start dev server:** ``gatsby develop``
	- *Gatsby will start a hot-reloading development environment accessible by default at localhost:8000*

- **create production build:** ``gatsby build``
	- *Gatsby will perform an optimized production build for your site, generating static HTML and per-route JavaScript code bundle*

- **serve production build locally:** ``gatsby serve``
	- *Gatsby starts a local HTML server for testing your built site. Remember to build your site using gatsby build before using this command.*

- **access documentation for CLI commands:** ``gatsby —help``

Also, read this in *Clean Code* today and thought these were fun and probably worth remembering..

- Number of function arguments:
	- 0 = niladic
	- 1 = monadic
	- 2 = dyadic
	- 3 = triadic
	- 4 of more = polyadic


### Day 21
**Thursday, November 7, 2019**

Published v1 of Restock! It's up at https://expo.io/@arlenpeiffer/restock

Currently I think it's only possible to view the app in Expo Client on Android but doing/did a bunch of reading on how to create an iOS standalone version. Links to a lot of that reading below.

Other than the publishing thing, did lots of refactoring and renaming this morning.. feel pretty good about how all of that turned out. ➡️ [Merge branch 'feature/refactorStyles' · arlenpeiffer/restock@4dc5829](https://github.com/arlenpeiffer/restock/commit/4dc5829e9127eba4cedd040118240441d3c6303d)

Also read some of *Clean Code* and looked into getting a laptop stand/external keyboard/mouse to help save my neck. Pretty alright day.

Reading:
- [Building Standalone Apps - Expo Documentation](https://docs.expo.io/versions/latest/distribution/building-standalone-apps/)
- [Deploying to App Stores - Expo Documentation](https://docs.expo.io/versions/latest/distribution/app-stores/)
- [Configuration with app.json - Expo Documentation](https://docs.expo.io/versions/latest/workflow/configuration/)
- [Publishing - Expo Documentation](https://docs.expo.io/versions/latest/workflow/publishing/)
- [React Native: How To Publish An Expo App To TestFlight + Debug Common Errors](https://levelup.gitconnected.com/react-native-how-to-publish-an-expo-app-to-testflight-debug-common-errors-90e427b4b5ea)
- [Enrollment - Support - Apple Developer](https://developer.apple.com/support/enrollment/)

### Day 20
**Wednesday, November 6, 2019**

Hung with Julian today and learned/took notes about the following:
- point-free style
- pipe()
- compose()
- [Eric Elliott – Medium](https://medium.com/@_ericelliott)
- breadth-first search (BFS)
- depth-first search (DFS)

Job boards:
- https://remoteok.io
- https://epicjobs.co

Books:
- [Code: The Hidden Language of Computer Hardware and Software](https://www.amazon.com/Code-Language-Computer-Hardware-Software/dp/0735611319) by Charles Petzold

Also spent a little bit of time toying with adding a footer / 'Close' tab to the bottom of each Section component in Restock. Seems like a nice idea, probably finish that up tomorrow.

Ciao!


### Day 19
**Tuesday, November 5, 2019**

Spent most of the day today working on the Restock app and made quite a bit of progress. I feel like most of the styling is now pretty much done so unless anyone I show it to has suggestions for new features, I can start wrapping things up and maybe try to write some tests.

Here are a few things I learned about today:
- **SafeAreaView** — renders nested content and automatically applies padding to reflect the portion of the view that is not covered by navigation bars, tab bars, toolbars, and other ancestor views. Moreover, and most importantly, Safe Area's paddings reflect the physical limitation of the screen, such as rounded corners or camera notches (i.e. the sensor housing area on iPhone X). Currently only applicable to iOS devices with iOS version 11 or later.
- **marginHorizontal** — has the same effect as setting both marginLeft and marginRight.
- **marginVertical** — has the same effect as setting both marginTop and marginBottom.
- **Dimensions.get('window').height** and **Dimensions.get(‘window’).width** — ways to access the device height and width (I think in pixels) using the React Native Dimensions class.
- [React Native Get Device Height Width on Button Click - DEV Community 👩‍💻👨‍💻](https://dev.to/skptricks/react-native-get-device-height-width-on-button-click-19ld)
- [react-native-viewport-units](https://github.com/jmstout/react-native-viewport-units)
- [react-native-normalize](https://github.com/NewBieBR/react-native-normalize)


### Day 18
**Monday, November 4, 2019**

Put in a copule hours after dinner on the Restock application. Got the navigation.navigate calls moved into the Header component and added some dynamic styles for indicating which nav item is currently selected. Did this by accessing the current index property on ``props.navigation.state`` and setting that equal to the respective index of each screen. The boolean from that then gets passed as prop isSelected to the styled NavigationItem component. Looks like this..

- ``isSelected={props.navigation.state.index === 0}  // Form``
- ``isSelected={props.navigation.state.index === 1}  // Checklist``

Took a bit of grappling with the React Navigation docs to realize that i didn't actually need to use their setParams and getParam functions in order to achieve the dynamic navigation styling I was aiming for (even though using those two functions seemed to be the most encouraged practice).

Anyways, that's about it for tonight. Ciao!

### Day 17
**Sunday, November 3, 2019**

Merged all of my commits from the last many days into the master branch! 🥳

Felt good to wrap up all the changes and tidy things up a bit. Next step is gonna be adding a header to the application. Starting to look at the best ways to do this with React Navigaiton. Here are a few things I've learned so far and a few links to things to look into further:

- To remove the back button in the upper left corner when moving from one screen to another, we can set ``headerLeft: null`` on the ``navigationOptions`` object.
``` 
// inside of createStackNavigator RouteConfigs object..
Checklist: {
  screen: Checklist,
  navigationOptions: () => ({
    headerLeft: null
  })
}
```
- To add a specified component as the header we can again set a prop on the ``navigationOptions`` object, this time setting ``header: Header`` where Header is our specified component.
- Turns out I won't be needing this to access the navigation prop inside of Header (I guess it's considered part of our screen components?) but to gain access to props.navigation in any ol' component you can use the HOC withNavigation: [withNavigation · React Navigation](https://reactnavigation.org/docs/en/with-navigation.html#docsNav)

Alright, that's all for tonight. See ya tomorrow!


### Day 16
**Saturday, November 2, 2019**

Got to mess around with things for a couple hours before and after work today. Spent most of that time working on a function that searches the global state (order) for a given section + item combination, and if found returns either item.amount or item.isChecked and uses that value for the component initial state. Went through a few iterations (which I'll paste below) and in the end decided to try setting it up as a custom hook (useInitialState).

Outside of that (and also inside of that) spent a lot of time thinking about being precise with naming. Always a struggle but feel pretty good about where I ended up with everything.

Alright here are a few versions of the function I worked on today..

```
v1 (SectionItem version)
// not reusable at all, had to have different versions of the function for both components (SectionItem / ChecklistItem)

const setInitialValue = (order, section, item) => {
  let initialValue = 0;
  order.some(s => {
    if (s.name === section) {
      s.items.some(i => {
        if (i.name === item) {
          initialValue = i.amount;
        }
      });
    }
  });
  return initialValue;
};


v2
// more reusable but requires a long list of arguments and has no default value

const findInitialValue = (order, section, item, field) => {
  let initialValue;
  order.some(s => {
    if (s.name === section) {
      s.items.some(i => {
        if (i.name === item) {
          initialValue = i[field];
        }
      });
    }
  });
  return initialValue;
};


v3 (Custom hook version)
// still requires more arguments than I'd like but happier with the naming/clarity

const useInitialValue = (
  initialValueKey,
  defaultValue,
  sectionToMatch,
  itemToMatch
) => {
  const { order } = useContext(OrderContext);

  let initialValue = defaultValue;

  order.some(section => {
    if (section.name === sectionToMatch) {
      section.items.some(item => {
        if (item.name === itemToMatch) {
          initialValue = item[initialValueKey];
        }
      });
    }
  });

  return initialValue;
};
```

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
