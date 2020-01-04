# 100 Days Of Code - Log

### Day 78
**Friday, January 3, 2020**

Made a little progress on the journal app today. Was able to refactor/add some more thoughtful names, update the log types, actions, and reducer to account for all of the new logs being added, and set up `AddItem` to submit its value when pressing `Enter` (hopefully that functionality will be able to be used in places like the `Login` and `SignUp` pages as well).

Also, a couple cool tidbits I learned..
- you can use block-scoped variables in a switch case if you wrap the case in brackets
- there's an html element called `optgroup` that can be used to group options in a select


### Day 77
**Thursday, January 2, 2020**

Finally got to spend a good chunk of time coding today.. first time in a couple few weeks. 😅

Started the day by re-reading Dan Abramov's article from yesterday (https://overreacted.io/my-decade-in-review/) and exploring some of the links included throughout. Here are a couple things I saved to watch/read later..

Talks:
- [Bret Victor - Inventing on Principle on Vimeo](https://vimeo.com/36579366)
- [(12) Dan Abramov - Live React: Hot Reloading with Time Travel at react-europe 2015 - YouTube](https://www.youtube.com/watch?v=xsSnOQynTHs)

Links:
- [React 16: A look inside a rewrite of our frontend UI library - Facebook Engineering](https://engineering.fb.com/web/react-16-a-look-inside-an-api-compatible-rewrite-of-our-frontend-ui-library/)

Terms:
- **dogfooding** — short for “Eating your own dog food,” which represents the practice of using your own products

After that, was able to work on the journal for a most of the late morning/afternoon. Cleaned up a lot of the ideas I'd been playing around with over the last handful of days regarding the async actions/thunks/firebase calls and started getting things set up to add new logs for diets, ingredients, meals (renamed from food), medications, and moods. Next step is setting up types/action creators/actions for the new logs and to update the logs reducer to handle all of the new action types.

Excited to keep at it.. hopefully some more progress on everything tomorrow.

Til then! 👋


### Day 76
**Wednesday, January 1, 2020**

Coding really made me want to cry today 😢 Struggled through a few hours of debugging that taught me to stop forgetting the `return` keyword at the end of functions and that I don't need to use thunks in every action.

Rough start to the new year coding-wise.. gonna get some rest and choose to believe I'll have more luck tomorrow. 

Til then..


### Day 75
**Tuesday, December 31, 2019**

Didn't have a ton of time today but was able to find a couple half hour windows (before work and during my lunch break) to continue exploring the rethink/refactor of the redux actions/firebase calls for the journal logs. It's seeming like I'll be able to reuse the `valueExists` and `setupAddToLogFunction` utility functions to replace all of the `handleLog...` functions in `AddEntry` and `EditEntry` but am going to need to write another function that takes the entry or editedEntry as well as a a log to map over. More on that tomorrow.

Ciao!


### Day 74
**Monday, December 30, 2019**

Worked on starting to rethink some of my redux actions / firebase calls and where the logic for checking whether or not an item already exists (whether or not to dispatch an action) happens. Wrote these helper functions that I'd like to refactor a bit tomorrow..

```
export const valueExists = (snapshot, inputValue) => {
  const values = Object.values(snapshot.val());
  const valueExists = values.some(
    value => value.toLowerCase() === inputValue.toLowerCase().trim()
  );
  return valueExists;
};

export const setupAddToLogFunction = (input, logName, callbacks) => {
  const { requestCallback, successCallback, failureCallback } = callbacks;

  return (dispatch, getState) => {
    const userId = getState().user.profile.id;
    const pathToLog = firebase
      .database()
      .ref(`users/${userId}/logs/${logName}`);

    dispatch(requestCallback());

    pathToLog.once('value').then(snapshot => {
      if (valueExists(snapshot, input)) {
        return console.log('failure');
        // dispatch(failureCallback('error message'));
      } else {
        return pathToLog.push(input).then(dispatch(successCallback(input)));
      }
    });
  };
};

export const setupCallbacksObject = (request, success, failure) => ({
  requestCallback: request,
  successCallback: success,
  failureCallback: failure
});
```

Also learned a sweet VScode trick.. how to make simulatneous edits using multiple cursors 😱 Shortcut is: `Opt + Cmd + Down` or `Opt + Cmd + Up`.


### Day 73
**Sunday, December 29, 2019**

Read Chapters 12 and 13 in *Clean Code* and started work on the `AddItem` component for the journal. Made some pretty good progress on the latter. It's mostly styled I think, and is able add items to a log (logs.supplements) when passed a callback function (addSupplement) as a prop. More to come on that tomorrow..

- [java - Design Patterns: Factory vs Factory method vs Abstract Factory - Stack Overflow](https://stackoverflow.com/questions/13029261/design-patterns-factory-vs-factory-method-vs-abstract-factory)


### Day 72
**Saturday, December 28, 2019**

Made some more big progress on the journal tonight. Figured out the styles for the arrow thing in `Popup` (pretty much ended up just copy/pasting from the Material-UI docs `ScrollPlayground` file), got rid of 3 packages (`antd`, `customize-cra`, and `react-app-rewired`), did a little refactoring, and updated the styles in `Header`. All in all was about 9 commits over the course of the afternoon and evening. Excited to keep at it and hopefully be able to get these changes up to production soon!

Ciao!


### Day 71
**Friday, December 27, 2019**

Made some awesome progress on the journal project today!! Was able to get the `Popup` component I've been working on to a good place (only thing I'd still like to add is a little styled arrow pointing to the anchor element.. links below ⬇️) and make use of it in `Entry` and `FieldArray`. I think `Entry` is the last component still using anything from Ant Design so that means I can finally remove that package from the project!

A couple links regarding the Popper arrowRef thing..

- [material-ui/ScrollPlayground.js at c0a1ad8da125cb73f2c0c9ec950f8fed13dc0321 · mui-org/material-ui](https://github.com/mui-org/material-ui/blob/c0a1ad8da125cb73f2c0c9ec950f8fed13dc0321/docs/src/pages/components/popper/ScrollPlayground.js)
- [reactjs - Example of using arrow in Popper component of material-ui - Stack Overflow](https://stackoverflow.com/questions/57500324/example-of-using-arrow-in-popper-component-of-material-ui)
- [poppers-with-arrows - StackBlitz](https://stackblitz.com/edit/poppers-with-arrows)


### Day 70
**Thursday, December 26, 2019**

Solved the leetcode problem from yesterday! Maybe not the most elegant of solutions, but am proud of myself for working through it and finding where my logic wasn't working. Here's my answer..

leetcode / **Roman to Integer**

```
var romanToInt = function(s) {
  const numerals = {
    'I': 1,
    'V': 5,
    'X': 10,
    'L': 50,
    'C': 100,
    'D': 500,
    'M': 1000
  };

  const arr = s.split('');

  const reducer = (accumulator, currentValue, currentIndex) => {
    const nextValue = arr[currentIndex + 1];

    const currentValueIsI = /I/.test(currentValue);
    const nextValueIsVorX = /V|X/.test(nextValue);
    const currentValueIsX = /X/.test(currentValue);
    const nextValueIsLorC = /L|C/.test(nextValue);
    const currentValueIsC = /C/.test(currentValue);
    const nextValueIsDorM = /D|M/.test(nextValue);

    const shouldReturn4or9 = currentValueIsI && nextValueIsVorX;
    const shouldReturn40or90 = currentValueIsX && nextValueIsLorC;
    const shouldReturn400or900 = currentValueIsC && nextValueIsDorM;

    if (shouldReturn4or9 || shouldReturn40or90 || shouldReturn400or900) {
      return accumulator - numerals[currentValue];       
    }
    return accumulator + numerals[currentValue];
  };

  return arr.reduce(reducer, 0);
};
```

Also spent a little while working on the journal project and was finally able to figure out why the Popper component wasn't positioning itself correctly! Had to create a new component (Popup) that has the Popper component and a button inside of it, making it possible to use the button (via event.currentTarget) as Popper's anchorEl prop.

Ok that's all for tonight. Ciao!


### Day 69
**Wednesday, December 25, 2019**

Spent at least an hour struggling with the Roman to Integer problem on leetcode. Here's where I was when I had to stop..

```
const numerals = {
  'I': 1,
  'V': 5,
  'X': 10,
  'L': 50,
  'C': 100,
  'D': 500,
  'M': 1000
};

const arr = s.split('');

const reducer = (accumulator, currentValue, currentIndex) => {
  const currentValueIsIXorC = currentValue === ('I' || 'X' || 'C');
  const nextValueExists = currentIndex + 1 < arr.length;
  const valueIsNotI = arr[currentIndex + 1] !== 'I';
  const nextValueExistsAndIsNotI = nextValueExists && valueIsNotI;

  if (currentValueIsIXorC && valueIsNotI) {
    console.log(currentIndex, accumulator, numerals[currentValue])
    return accumulator - numerals[currentValue];
  }
  console.log(currentIndex, accumulator, numerals[currentValue])
  return accumulator + numerals[currentValue];
};

return arr.reduce(reducer, 0);
```


### Day 68
**Tuesday, December 24, 2019**

Worked on a few coding problems on leetcode today.. wanna try to get into the habit of doing that a little bit each day. Here are my answers for the problems I completed and a few improvements I learned about after submitting.

leetcode / **Two Sum**

```
var twoSum = function(nums, target) {
  let currentSum;
  
  for (i = 0; i < nums.length - 1; i++) {
    for (j = i + 1; j < nums.length; j++) {
      currentSum = nums[i] + nums[j];
        if (currentSum === target) {
          return [i, j];
      }
    }
  }
}

// improved //
var twoSum = function(nums, target) {
  for (i = 0; i < nums.length; i++) {
    for (j = i + 1; j < nums.length; j++) {
      if (nums[j] === target - nums[i]) {
        return [i, j];
      }
    }
  }
}
```

leetcode / **Reverse Integer**

```
var reverse = function(x) {
  const reverseX = Math.abs(x).toString().split('').reverse().join('');
  const output = x < 0 ? -Math.abs(reverseX) : reverseX;
  if (output > 2147483647 || output < -2147483647) {
    return 0;
  }
  return output;
};

// improved //
const reverse = x => {
  const limit = 2147483648;
  const k = x < 0 ? -1 : 1;
  const n = Number(String(Math.abs(x)).split('').reverse().join(''));
  return n > limit ? 0 : n * k;
};
```

leetcode / **Palindrome Number**

```
var isPalindrome = function(x) {
  const rev = x.toString().split('').reverse().join('');
  const answer = x.toString() === rev ? true : false
  return answer;
};

// improved //
var isPalindrome = function(x) {
  return x === Number(x.toString().split("").reverse().join(""));
};
```

Also a couple links..
- [Algorithms in JavaScript: Hash Tables - JavaScript in Plain English - Medium](https://medium.com/javascript-in-plain-english/algorithm-in-javascript-hash-table-7b0464d2b81b)
- [What is 32 bit signed integer? - Quora](https://www.quora.com/What-is-32-bit-signed-integer)


### Day 67
**Monday, December 23, 2019**

Feeling a little better/less defeated today. Was able to eventually realize that my issue from last night with CSS grid was just user error.. the thing I was trying to make work is literally not possible with grid. That was important to realize.

Was able to take a step back and see that what I really needed was flexbox, which I had written off earlier thinking that grid was the only answer. I think it's going to be important for me going forward to be able to step back and catch myself when I'm falling into these narrow-minded, hyper-focused traps.

My work time today was spent cleaning up the CSS for the resume project and continuing to build out the layout and data object. It's coming along.

Also found this cool blog post with some nice color schemes..

- [50 Best Website Color Schemes of 2020 | Design Shack](https://designshack.net/articles/trends/best-website-color-schemes/)

Ciao!


### Day 66
**Sunday, December 22, 2019**

Still struggling with CSS Grid and becoming increasingly defeated feeling tonight. Can't help but feel like I'm wasting my time fiddling with responsive CSS when I should honestly just be done with my little resume project by now.


### Day 65
**Saturday, December 21, 2019**

Made it about halfway, or a little more, in the Wes Bos CSS Grid course. Starting to pick up some of the core concepts for sure.

Here are a few notes I took while working through the videos..

**implicit vs explicit**

- `grid-template-columns: 200px 200px;` ➡️ creates two explicitly defined columns. if you have more than 2 grid items, CSS Grid will start implicitly creating new rows to place the items in.

- to define the size of implicitly created rows we can use `grid-auto-rows: 200px;`

**auto-flow**

- `grid-auto-flow: row;` ➡️ adds implicit row(s) for extra grid items when explicit grid is full. it's the default setting.

- `grid-auto-flow: column;` ➡️ adds implicit column(s) for extra grid items when explicit grid is full.

**sizing tracks**

- `fr` = fractional unit ➡️ similar to flex grow.. 2fr will take up twice the amount of free space as 1fr.

**repeat**

- `grid-template-columns: repeat(4, 1fr);` ➡️ creates 4 columns with equal widths.

- `grid-template-columns: repeat(2, 1fr 2fr);` ➡️ creates 4 columns with alternating widths of 1fr and 2fr.

**sizing grid items**

- `grid-column: span 2;` ➡️ grid item will span 2 columns.

- `grid-row: span 2;` ➡️ grid item will span 2 rows.

**placing grid items** 
- `grid-column-start: 2;` ➡️ starts item at track value 2.

- `grid-column-end: 5;` ➡️ grid item goes up until track value 5.

- `grid-column: 2 / 5;` ➡️ shorthand syntax.. item starts at track value 2 and spans 3 columns to track value 5.

- `grid-column: span 2 / 5;` ➡️ grid item will span 2 columns and end at track value 5 (so ends up starting at track value 3).

- `grid-column: 1 / span 2;` ➡️ grid item starts at track value 1 and spans 2 columns.

- `grid-column: 1 / -1` ➡️ grid item spans entire row.


### Day 64
**Friday, December 20, 2019**

Worked tonight for a few hours on the new resume project and made some progress on figuring out how to customize the `skelet.css` file. Was able to set up more fitting `h1` - `h6` sizes/weights/spacing and as well a responsive size (and static breakpoint size) for `p` tags. Would still maybe like to move the parts of `skelet.css` that I'm using to a seperate  file just to make things a little cleaner and easier to read but that's all yet to come.

Also, started watching the Wes Bos CSS Grid videos in hopes of gaining a little better understanding of how grid works. Pretty sure Skelet is using it behind the scenes but I'd kind of like to set up my own grid styles to gain a better understanding of how CSS Grid works without Skelet's abstraction.

More to come on all of that tomorrow. Til then!


### Day 63
**Thursday, December 19, 2019**

Continued with the resume project today. Figured out the right way to import css files (`import './css/skelet.css';` in `index.js`) after trying for a while to add them via a link tag in the head of `index.html`. Not 100% sure why that way wasn't working but the React way seems cleaner.

Spent most of the night struggling to get CSS to do what I wanted.. realized I've got a lot still to learn on that front.

By the end of the night was able to get:
- font sizes to be responsive based on window size
- font sizes to become static at a 600px breakpoint
- the "references" section to render in Skelet's little grid system

Still got a ways to go but happy to be starting on the resume. Ciao!


### Day 62
**Wednesday, December 18, 2019**

Took a break from the journal project today and spent some time thinking on how to approach my resume. Decided it might be a cool idea to build a simple react app that will render a styled & responsive version of the resume data. Did some exploring for a while, hoping to find a simpler boilerplate than create-react-app, and came upon this thing [Skeleton: Responsive CSS Boilerplate](http://getskeleton.com/) which seemed kinda cool. Dug into that for a while and figured out how to set their simple boilerplate up with React/Babel/Webpack ([this article was helpful](https://dev.to/theenadayalan/how-to-set-up-react-js-from-scratch-without-using-create-react-app-37mk)) and eventually came upon a CSS framework attached to Skeleton called [Skelet. — a contemporary CSS framework & HTML boilerplate](https://selekkt.dk/skelet/).

Still trying to figure out exactly what it is I'm looking for for this little project, but feel like I started to get somewhere after playing around for a while. Definitely know I want this project to be quick and simple, but would like the CSS and HTML markup to be smart. Also, started to wonder this evening if CSS Grid is something that might be helpful / could replace the need for Skeleton / Skelet. We'll see, hopefully there will be more time tomorrow to explore and make some decisions.

Also listened to the new Syntax episode today (Episode #206) and took some notes on things I want to do more research on / jotted down some links to start/keep exploring.

Terms/Concepts:
- css atomic styles
	- * [Let’s Define Exactly What Atomic CSS is | CSS-Tricks](https://css-tricks.com/lets-define-exactly-atomic-css/)
- observables
- data attributes
	- [Using data attributes - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)
- pure functions
- css specificity
	- [Specifics on CSS Specificity | CSS-Tricks](https://css-tricks.com/specifics-on-css-specificity/)

Links:
- [Lynn Fisher - lynnandtonic.com](https://lynnandtonic.com/)
- [A Single Div](https://a.singlediv.com/)
- [Airport Codes](https://airportcod.es/)
- [WhyAZ](https://why.az/)
- [Single Div Drawings with CSS - Mozilla Hacks - the Web developer blog](https://hacks.mozilla.org/2014/09/single-div-drawings-with-css/)
- [CSSDay Talk: Illustration with CSS - lynnandtonic.com](https://lynnandtonic.com/thoughts/entries/talk-illustration-with-css/)


### Day 61
**Tuesday, December 17, 2019**

Worked all day on the journal stuff.. looks like I did 14 commits today which feels affirming.

Made some pretty big progress on the `feature/removeAntDesign` branch. I think I've just got one component left before I can remove `antd` as a dependency!

I feel like I spent a good amount of time today working on more UX sorts of stuff (like whether the `Login` form should submit when pressing Enter), which was important but felt kind of like the wrong thing to be focusing on. Had to tell myself a handful of times to stop focusing on the UX stuff so much and just get the current changes to production, even if the UX isn't perfect rn.

Working on figuring out the `Popper` component now that asks for confirmation when removing an entry or item. Seeming complicated so far but found this library ([material-ui-popup-state](https://github.com/jcoreio/material-ui-popup-state)) that may help.. hoping I can figure it out without having to use another dependency tho.


### Day 60
**Monday, December 16, 2019**

Alright. Had a pretty productive day over here. It's late rn and I'm really tired so we'll see how much sense this entry will make. Figured out the issue I was having with the `Filters` component and got that all put together and working. After that started working on rewriting the old `Error` component (now named `Alert`) to get rid of Ant Design. Got that part almost all finished, and feeling like it's a pretty nice and reusable component.

A couple reading links from the day..
- [Getting to the bottom of line height in Figma](https://www.figma.com/blog/line-height-changes/)
- [Tim Berners-Lee - Wikipedia](https://en.wikipedia.org/wiki/Tim_Berners-Lee)
- [Typography is impossible - Medium Engineering](https://medium.engineering/typography-is-impossible-5872b0c7f891)
- [wsmd/react-use-form-state: 📄 React hook for managing forms and inputs state](https://github.com/wsmd/react-use-form-state)


### Day 59
**Sunday, December 15, 2019**

Worked on the `Filters` compoment for an hour or so after work. Was able to figure out the disabled date handling (code below!) and started hooking the filter values up to Redux. Only issue I'm having rn is with the danged `Input` component which is updating behind the scenes but the display value remains set to its own internal value. Trying to figure out if the hook I wrote yesterday will be able to work with this use case or if I need to rewrite it to be more flexible. More on that tomorrow...

Here's the code for the date disabling:
```
// startDate and endDate are being pulled from formik values.date //

const handleDisableEndDates = (date, startDate) => {
  if (!startDate) {
    return false;
  }
  return date < startDate;
};

const handleDisableStartDates = (date, endDate) => {
  if (!endDate) {
    return false;
  }
  return date > endDate;
};

// shouldDisableDate={date => handleDisableEndDates(date, startDate)}
// shouldDisableDate={date => handleDisableStartDates(date, endDate)}
```


### Day 58
**Saturday, December 14, 2019**

Fixed a bug created by the useEffect hook I added to `Input` yesterday. Realized it was wiping any starting values when editing an entry due to the effect running on mount. Extended the hook to use a ref to check for the mounted state. Here's the code I wrote:

```
const componentIsMounted = useRef(false);

useEffect(() => {
  if (componentIsMounted.current && allowReset) {
    setValue(resetValue);
    setFieldValue(field.name, resetValue);
    setFieldTouched(field.name, false);
  }
  componentIsMounted.current = true;
}, resetDependencies);
```

Thought about making it a custom hook that takes the side effect function and the dependencies array as arguments, but decided I'll wait until I have a few other components that need to run useEffect on update, but not on render, first.

After that started playing around with swapping out the Ant Design components in `Filters` and ran into a wall trying to get the handling for disabled dates to work with the Material DatePicker. Keep chipping away at that tomorrow, but feeling mighty confused by it tonight.

Ciao!


### Day 57
**Friday, December 13, 2019**

I completed the custom Material-UI components finally!!! `EntryForm` is now completely overhauled with nicer looking UI! Still got plenty to take on but that feels like a big accomplishment. Pretty proud of how a lot of it turned out.

Let's see, TIL that..
- Radio inputs work best (only work?) with string values.. spent a while trying to pass a copule boolean values earlier womp womp
- I can code! Had a long VSCode Live Share hang with my buddy Julian, trying to sort out an issue with one of the use cases for the `Input` component and was able to find a solution!! Was really proud of myself and my ability to explain and work through the problem with him, and am so pumped to have found a decent/working solution. Here's the part that made it all work..

```
useEffect(() => {
  if (allowReset) {
    setValue(resetValue);
    setFieldValue(field.name, resetValue);
    setFieldTouched(field.name, false);
  }
}, resetDependencies);
```

A couple links from the day to top it off..
- [seek-oss/treat: Themeable, statically extracted CSS‑in‑JS with near‑zero runtime.](https://github.com/seek-oss/treat) - Julian mentioned this new alternative to styled-components
- [React Training: When do I use functions in a Hooks Dependency Array?](https://reacttraining.com/blog/when-to-use-functions-in-hooks-dependency-array/)

Ciao!


### Day 56
**Thursday, December 12, 2019**

Another big day today. Spent pretty much the entire day working on things and kinda seems like I did a lot.

Spent the first couple hours of the day throwing together a `Printout` component to collect Callie's journal data for Dr. Gupta. Think it turned out alright.

After that worked on the `Slider` component and tried to make some decisions about it. By the middle of the afternoon had that looking and feeling pretty good, and started trying to throw together a `Switch` component for the "travel" `EntrySection`. That's the last part of the `EntryForm` left, and unfortuantely it's turning into another hurdle. I'm not yet able to figure out how to go about updating the `travel.location` value (I'd like it to switch from 'Home' to an empty string) based on the value of `travel.isTraveling`. I was able to get the Formik values updated behind the scenes but for some reason the `Input` is not reflecting those changes. Aw here it goes.

Stepped away from that issue after a while and started refactoring and breaking the Formik validation schemas out into their own files. Realizing what an insanely large project this is, and starting to feel like maybe I should take a step back and just focus on the personal site and resume for the remainder of the year, as I'm skeptical that I'll honestly be ready to commit all of these changes to `master` any time soon.

Gonna sleep on it. See ya tomorrow.


### Day 55
**Wednesday, December 11, 2019**

Made some huge (feeling) progress on the journal today! Finalized all of the changes from the last few days and committed/merged everything back into the `feature/refactorFormik` branch. Also made some big progress on refactoring and fleshing out the Slider component, and am pretty proud of some of the code I wrote for it.. ⬇️

**My first recursive function in real life ??**
```
const generateMarks = (min, max) => {
  if (max === min) {
    return [{ value: min }];
  }
  const marks = generateMarks(min, max - 1);
  marks.push({ value: max });
  return marks;
};
```

**A time formatting function for Slider.js**
```
const displayFormat= value => {
  let numberOfHours = Math.floor(value);
  let numberOfMinutes = (value - numberOfHours) * 60;

  const plural = number => (number > 1 ? 's' : '');
  const hours = numberOfHours
    ? `${numberOfHours} hr${plural(numberOfHours)}`
    : '';
  const minutes = numberOfMinutes
    ? `${numberOfMinutes} min${plural(numberOfMinutes)}`
    : '';

  return `${hours} ${minutes}`;
};
```

Lastly, a couple links from the day..
- [css - What is the difference between display: inline and display: inline-block? - Stack Overflow](https://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block/14033814)
- [How to Center in CSS](http://howtocenterincss.com/)

Ciao!


### Day 54
**Tuesday, December 10, 2019**

Worked really hard on trying to finish the rest of the journal components today. Sadly only made it most of the way through the CheckboxGroup component by the end of the day.

Tried for a few hours in the morning to add some features to the Toggle component (changing the border color to `theme.primary.main` when focused or hovered and improving the onBlur functionality/setting the touched state to true if the user selects and deselects the same value) but had to give up and move on after a while.

The rest of the afternoon was spent figuring out things for the CheckboxGroup component.. how to sort the array of checked values before updating the field value and not have Formik re-render an unnecessary amount of times, and then lots of styling things like how to remove the silly halo effect on hover (and then how to remove it on hover when the checkbox state is checked 🙄), how to get rid of the silly amount of padding around the checkboxes, and how to make the font size smaller in Material's FormControlLabel component (still not totally happy about the workaround I figured out). Also spent a while learning how the MUI Grid system works.

Got pretty into using the keyboard shortcut `⌘ + Shift + C` in the Chrome DevTools to inspect elements.

Hopefully with a little luck I can get the rest of the CSS hurdles figured out tomorrow and be on my way to finishing these components.. 😅


### Day 53
**Monday, December 9, 2019**

Spent the morning working on the Toggle component for journal and was able to figure out styling and the `flex-direction` switch based on screen size! Learned about overriding styles and props in the global theme object, using the classes prop to access style class names on components, and ways to pass props for dynamic styling just like in styled-components. Here are some links:

- [Globals - Material-UI](https://material-ui.com/customization/globals/)
- [Default Theme - Material-UI](https://material-ui.com/customization/default-theme/#default-theme)
- [Overriding styles - classes prop - Material-UI](https://material-ui.com/styles/advanced/#overriding-styles-classes-prop)
- [@material-ui/styles - Material-UI](https://material-ui.com/styles/basics/#material-ui-styles)

Ciao!


### Day 52
**Sunday, December 8, 2019**

Only had a little time to work on coding tonight after working all day at the cafe, but tried to make the most of it by digging into the Material UI docs on theming and dynamic rendering. Was looking for information on a way to change the layout of a component based on the size of the screen. Learned briefly about a few things..

- `Grid` - a component that deals with dynamic rendering/layout. Don't think it's the tool I need for this situation though as I need the component itself to change its layout, not the page.
- `Hidden` - a component that I didn't know existed. Can dynamically show or hide whatever is passed to it, cool to know about.
- `theme.breakpoints` - part of the theme configuration object. It contains a list of breakpoint values and functions for returning media queries.. definitely seems like this is what I'm after. Feeling a little confused though by its functions (`up`, `down`, `between`, etc.) as they weren't working the way I expected. Definitely gonna do some more reading tomorrow and try to figure out if it's something I'm doing wrong or an issue with the Material API.

Alrighty. Ciao!


### Day 51
**Saturday, December 7, 2019**

Spent a good chunk of the day working on the components for journal. Finished up with `AutoComplete`, which can now fully support the `multiple` prop for storing an array of items (using it to save a list of searchable ingredients under `food.meals.items`).

Got a pretty good start on the `Select` component but still need to figure out how I want to set it up to take an array of items (`logs.diets` for example) as a prop and render a `MenuItem` for each item in the array. Also, would like to be able to pass `Select` a prop that, it true, will render an `AddItem` component below the menu, allowing the user to add new `MenuItem`s, etc.

The major hang up (learning experience) today came in the form of a basic HTML attribute. I started noticing while working on the `AutoComplete` component that when I would hit enter inside of the input, the entire form would submit and all fields were suddenly touched and showing validation errors. Oh god! Turns out that's a standard feature of the attribute `type="submit"`. Got rid of that and added an `onClick={handleSubmit}` and everything worked as expected. Great to know for the future!

Alright, that's all for now. See ya tomorrow!


### Day 50 🎉
**Friday, December 6, 2019**

Woohoo! I made it halfway 🙃

Worked on refactoring and building out the input components for journal some more. Lots of playing around with different edge cases tonight, like "since the AutoComplete components only update the field value when you select something from the dropdown or hit enter, what happens if you have something typed in the input but tab or click to another field?" (Answer: gotta setFieldValue onBlur!)

Also figured out how passing props to a field in Formik works when using useField..

➡️ `const [field, meta] = useField({ name, validate, ...props });`

Feel like it's all coming together and the intricacies of Formik are finally starting to make some more sense. Hopefully be able to wrap up most of the work on the input components tomorrow. Fingers crossed!

Ciao!


### Day 49
**Thursday, December 5, 2019**

Spent pretty much the entire day working on the journal project. I feel like it's harder to quantify what I learned on days when I'm plugging away at a project, reading docs, and refactoring.

I did learn some about controlled vs uncontrolled inputs today though. Controlled inputs keep the source of truth in component state while uncontrolled inputs keep the source of truth in the DOM. The value of an uncontrolled input can be retreived from the DOM at any point using a ref. **Sidenote:** refs have really confused me up to this point, and I still have more to learn about them, but today felt like good exposure/a nice introduction to them.

Other than that, decided to scratch moving forward with React Hook Form (rewriting the entire project was seeming daunting, and also the uncontrolled input thing was freaking me out/making me feel like I was going to run into unforseen problems down the line). Went back to Formik and came up with a hacky way to get around the constant re-rendering Input fields. Not the prettiest of things but I'm happy to be moving again.

See ya for Day 50 tomorrow! 🎉


### Day 48
**Wednesday, December 4, 2019**

Stayed up til almost 2am playing around with React Hook Form. Seeming like it might be a better solution than Formik, but still got a few more things to figure out. More on everything tomorrow..

Ciao!


### Day 47
**Tuesday, December 3, 2019**

Wow okay so... just spent the entire day grappling with Formik. Super over it. Not finding a way to get it to work without everything constantly rerendering (becomes a major problem when inputs cause everything to rerender on every keystroke 😬). Sidenote: not sure why I wasn't having this same issue with Ant Design, but it's become an increasingly troublesome issue since switching to the new Material componenets. Either way, feeling bummed but hanging on to hope after finding another library that looks promising (React Hook Form). Excited to try bringing that in tomorrow and see how it works.

Links from the day:
- [why-did-you-render](https://github.com/welldone-software/why-did-you-render)
- [Why Did You Render Mr. Big Pure React Component??? - Welldone Software - Medium](https://medium.com/welldone-software/why-did-you-render-mr-big-pure-react-component-2a36dd86996f)
- [Why Did You Render Mr. Big Pure React Component Part 2- Common Fixing Scenarios](https://medium.com/welldone-software/why-did-you-render-mr-big-pure-react-component-part-2-common-fixing-scenarios-667bfdec2e0f)
- [Using recompose HOC’s to get better performance in forms using Formik and Yup](https://travix.io/using-recompose-hocs-to-get-better-performance-in-forms-using-formik-and-yup-e51024d645ba)
- [create-formik-fields-comparator](https://github.com/armand1m/create-formik-fields-comparator)
- [react-final-form](https://github.com/final-form/react-final-form)
- [React Top-Level API — React.memo](https://reactjs.org/docs/react-api.html#reactmemo)
- [react-hook-form](https://github.com/react-hook-form/react-hook-form)
- [Build the Next Generation of Forms With React Hooks Forms](https://medium.com/better-programming/build-the-next-generation-of-forms-with-react-hooks-forms-b4f2039e51c1)
- [React Hook Form - Array Fields - CodeSandbox](https://codesandbox.io/s/6j1760jkjk)




### Day 46
**Monday, December 2, 2019**

Still struggling today. I feel like all I'm doing is finding/creating more and more issues with these custom Material components for the journal. Having trouble with the Formik validation and figuring out how not to have components rerender into an error'd state when another field changes (gotta keep fucking with the onBlur and touched values), and also for some reason the Input component is suuuuper laggy. Tried using Formik's FastField instead of Field but didn't see any immediate difference so thinking it must be something else (or I'm just doing something wrong).

Trying really hard to remember that even though I've been stuck on some version of this same issue for about four days now, I don't suck at coding..


### Day 45
**Sunday, December 1, 2019**

Spent a few more hours tonight on the TimePicker component, just trying to get it cleaned up. Not sure why this one required so much effort but feeling pretty frustrated/defeated/embarrassed about it. I think I'm just feeling wiped/a little burnt out and maybe also struggling a bit with the intricacies of the Formik library (definitely don't think I'll be using it again in the future).

Either way here's the refined code for the TimePicker component..

Ciao!

```
// TimePicker.js

import React, { useEffect } from 'react';
import { Field, useField } from 'formik';
import moment from 'moment';
import { TimePicker as MuiTimePicker } from '@material-ui/pickers';

const TimePicker = ({ name, setFieldValue, ...props }) => {
  const [date] = useField('date');
  const [field] = useField(name);

  const startOfDay = moment(field.value).startOf('day');
  const timeSinceStartOfDay = moment(field.value).diff(startOfDay);
  const selectedTimeOnCurrentDate = moment(date.value)
    .add(timeSinceStartOfDay)
    .valueOf();

  useEffect(() => {
    setFieldValue(name, selectedTimeOnCurrentDate);
  }, [date.value]);

  return (
    <Field name={name} {...props}>
      {({ field, form, meta }) => {
        const { error } = meta;
        const hasError = error ? true : false;

        const handleChange = value => {
          form.setFieldValue(field.name, moment(value).valueOf());
        };

        return (
          <MuiTimePicker
            error={hasError}
            format="h:mm A"
            helperText={error}
            onChange={handleChange}
            value={selectedTimeOnCurrentDate}
            {...props}
          />
        );
      }}
    </Field>
  );
};

export default TimePicker;
```


### Day 44
**Saturday, November 30, 2019**

Feeling a little burnout/imbalance coming on again today. Noticed a frustration, both with the pace of this week and with not being able to find as much time to work on everything, that I feel is being taken out on myself (feeling incapable/not good enough because I'm moving at a slower pace than I had hoped). Feels good to recognize it though, and to remind myself that this is all a process and that I'm still learning and developing my workflow.

Spent a handful of hours this morning chipping away at the TimePicker issue I mentioned yesterday.. Was able to get it working after a couple of hours but am still grappling with getting the code to be less of a mess. Here's what I had by the end of the morning..

```
// TimePicker.js

import React, { useEffect } from 'react';
import { Field, useField } from 'formik';
import moment from 'moment';
import { TimePicker as MuiTimePicker } from '@material-ui/pickers';

const TimePicker = ({ name, setFieldValue, ...props }) => {
  const [date] = useField('date');
  const entryDate = date.value;
  const [time] = useField(name);
  console.log(time);
  const startOfDay = moment(time.value).startOf('day');
  const timeSinceStartOfDay = moment(time.value).diff(startOfDay);
  const selectedTimeOnEntryDate = moment(entryDate).add(timeSinceStartOfDay);
  console.log(selectedTimeOnEntryDate);

  useEffect(() => {
    console.log('useEffect', name);
    setFieldValue(name, selectedTimeOnEntryDate.valueOf());
  }, [entryDate]);

  return (
    <Field name={name} {...props}>
      {({ field, form, meta }) => {
        const { error } = meta;
        const hasError = error ? true : false;

        {
          /* const startOfDay = moment(field.value).startOf('day');
        const timeSinceStartOfDay = moment(field.value).diff(startOfDay);
        const selectedTimeOnEntryDate = moment(entryDate).add(
          timeSinceStartOfDay
        ); */
        }

        const handleChange = value => {
          form.setFieldValue(field.name, moment(value).valueOf());
        };

        return (
          <div>
            <MuiTimePicker
              error={hasError}
              format="h:mm A"
              helperText={error}
              onChange={handleChange}
              value={selectedTimeOnEntryDate}
              {...props}
            />
            <p>Date Unix: {entryDate}</p>
            <p>Date String: {moment(entryDate).toString()}</p>
            <p>
              Date String (Formatted):{' '}
              {moment(entryDate)
                .format('dddd, MMMM Do YYYY, h:mm a')
                .toString()}
            </p>
            <p>Time Unix: {time.value}</p>
            <p>Time String (Default): {moment(time.value).toString()}</p>
            <p>
              Time String (Formatted):{' '}
              {moment(time.value)
                .format('dddd, MMMM Do YYYY, h:mm a')
                .toString()}
            </p>
            <p>
              selectedTimeOnEntryDate:{' '}
              {moment(selectedTimeOnEntryDate).format(
                'dddd, MMMM Do YYYY, h:mm a'
              )}
            </p>
          </div>
        );
      }}
    </Field>
  );
};

export default TimePicker;
```

Hoping I can refactor this a little tomorrow and also step up my git workflow going forward. I know I can do better with branching as well as staying on task when on a particular branch.


### Day 43
**Friday, November 29, 2019**

Worked tonight on refactoring the FieldArray component and getting it to work inside of another FieldArray (`items` FieldArray inside of `meals` FieldArray). Also worked on setting up a TimePicker component and learned the difference between `@material-ui/pickers`'s regular DatePicker/TimePicker and their KeyboardDatePicker/KeyboardTimePicker. Seems like switching to the basic version sans keyboard control makes my life simpler in a lot of ways (doesn't require me to make the input read only, opens the picker modal when clicking anywhere in the compoment instead of just when clicking the icon on the right), so got those swapped out. Still gotta figure out how to get the correct unix value for the TimePicker/how to make it sync up with the current date.. That'll be my first challenge for tomorrow 🙃

Also worth noting, took the State of JS survey today which felt really cool after reading through all of the data from last year. I feel excited to be at a place where I can contribute my thoughts and experience thus far, even if it is only a year or so's worth of thoughts and experiences.

Excited to keep at it tomorrow. See ya then!


### Day 42
**Thursday, November 28, 2019**

Back at it today. Worked today on creating my own custom Formik FieldArray component that abstracts some repeated code out of the EntryForm component. Started learning about React.Children + React.cloneElement and when they can be useful in place of props.children.

[reactjs - When should I be using React.cloneElement vs this.props.children? - Stack Overflow](https://stackoverflow.com/questions/37521798/when-should-i-be-using-react-cloneelement-vs-this-props-children/50441271#50441271)


### Day 41
**Wednesday, November 27, 2019**

Happy Birthday Arlen!


### Day 40
**Tuesday, November 26, 2019**

Worked on more of the same today. Got a start on a CheckboxGroup component and also made a FieldLabel component using Material's InputLabel with the shrink prop. Took a while to get that one to look right.. had to do some css research but learned how to select the adjacent sibling of an element with the **adjacent sibling combinator** (+).

Feeling pretty tedious getting these components set up but hopefully it will pay off in the end. Ciao!

### Day 39
**Monday, November 25, 2019**

Super tired rn, but gonna make a quick log.

Worked on the journal / got a few more components started (Slider, Toggle) and Rating seems pretty much done. Grappling with the spacing quite a bit, feel like I should maybe dig into the docs on theme.spacing() tomorrow.. figure out how to do all that.

Also read the *Clean Code* chapter on classes and did some extra reading on private vs. public variables, static vs. non-static variables, coupling, and cohesion.

Ok goodnight


### Day 38
**Sunday, November 24, 2019**

Only had a little time to work on stuff today, but got a decent start on creating a Rating compoment for the journal, and learned that passing `multiline={true}` to the MUI TextField compoment will convert it from a standard input to a textarea. Pretty cool!

Also started reading Chapter 10 of *Clean Code* and have a growing list of OOP concepts to look up/understand better. So far it's just:
- what are private and public variables?
- what are static variables?

Was hoping to have more time to read up on everything tonight but I think it'll have to wait til tomorrow..

Til then!


### Day 37
**Saturday, November 23, 2019**

Spent pretty much the entire day reading docs and playing around with Material-UI and Formik. Starting to get a better system going for Journal. Definitely gonna be a bit of work these next few days as I figure out how to re-implement what I had going on with some of the Ant Design components, but shouldn't be too crazy.

TIL about:
- **jsconfig.json files** https://code.visualstudio.com/docs/languages/jsconfig
	- *started quickly looking at ways to set up absolute paths in a create react app and I guess doing it via a jsconfig.json file is the recommended way in the CRA docs* https://create-react-app.dev/docs/importing-a-component/#absolute-imports
- **index.lock files in git**
	- *experienced a weird new error i've never seen before. was happening every time i was trying to stage changes in SourceTree. seems maybe kind of common? read a few things briefly, but seems like the way to solve it is by running* `rm -f .git/index.lock`

Okay I'm fried. See ya tomorrow.


### Day 36
**Friday, November 22, 2019**

Had a really fun day today with coding! Spent the evening doing some research / getting inspiration for my personal website. Looked at a bunch of other front end dev's portfolios and took down a bunch of notes.

During the couple hours of research, I started scratching the surface of a cool new programmer/art/intellectual world that I want to get more involved with. Here are a bunch of links related to that scratching..

- http://www.onemilescroll.com/
- https://www.are.na/jon-kyle-mohr
- http://obs.astro.ucla.edu/towercam.htm#imagetop (Mt. Wilson Towercam)
- https://thecreativeindependent.com/
- https://a-b-z.co/
- http://kenlumart.com/
- http://ascii.designforthe.net/ (ASCII Town)
- http://designforthe.net/
- https://www.are.na/mindy-seu/
- http://www.mindyseu.com/

After all of that fun digging, I was feeling super motivated and excited to start rethinking a few parts of the Journal project. Been heavily considering ditching the Ant Design stuff and trying out a different UI library. Was looking at Material Design for a while tonight but after scratching my head just trying to get it imported, I realized that it's massively complicated and that Material-UI is actually the way to go. Spent some time getting that set up and then playing around with colors and the ThemeProvider. Feel really excited to keep digging on all of that tomorrow.

Ciao!


### Day 35
**Thursday, November 21, 2019**

Man this thing is a crazy ride. Learning every day..

Went to bed early last night and woke up today feeling a little lighter. Tried not to set too many expectations or programming goals for the day, and just started out by reading some of *Clean Code* and a couple articles.. *(links below)*

After that I tried taking another crack at the refactor stuff for Restock that I've been struggling with. Wound up getting tangled in it again and decided I'm gonna have to leave that one for later and move on. Feels really discouraging to have been working at it now for a couple days and to not be seeing any results.

After forfeiting on the Restock stuff, I started digging into the freeCodeCamp section on Object Oriented Programming. Feels fun so far. Made it through 11 out of the 26 challenges for the section.

Links from the day:
- [Inversion of Control](https://kentcdodds.com/blog/inversion-of-control)
- [MICHÆL JACKSON on Twitter: …](https://twitter.com/mjackson/status/1195495535483817984)
- [No, disabling a button is not app logic. - DEV Community 👩‍💻👨‍💻](https://dev.to/davidkpiano/no-disabling-a-button-is-not-app-logic-598i)


### Day 34
**Wednesday, November 20, 2019**

Had a mostly pretty frustrating day today. I think I'm burning out a bit.

Spent pretty much the whole day coding again, but tried to step away and take a few breaks.

Was able to finish the Basic Algorithm Scripting section on freeCodecamp. The last challenge really threw me for a loop.. feel like I spent way too long on that one, and in the end had to look up hints for how to solve it. Either way, solutions for the last 3 challenges are below.

Also read a copule chapters in *Clean Code* and spent some time trying to refactor things in Restock. Still trying to figure out the split reducer stuff, and if I can move the handleUpdateOrder logic out of App.js and into the components that call the ADD/UPDATE/REMOVE actions. As I work on it more I'm struggling to see if it's even possible to separate some of the layers given the way it's built. Might spend a little time on it tomorrow, but kind of feeling like I should maybe ditch the refactor stuff and move on. Find out tomorrow..

Here're today's solutions from Basic Algorithm Scripting..

**Where do I Belong**

```
function getIndexToIns(arr, num) {
  let indexToIns = 0;
  const sorted = arr.sort();

  for (let i = 0; i < sorted.length; i++) {
    if (num > sorted[i]) {
      indexToIns++;
    }
  }
  return indexToIns;
}

getIndexToIns([40, 60], 50);
```

**Mutations**

```
function mutation(arr) {
  const string = arr[0].toLowerCase();
  const characters = arr[1].toLowerCase().split('');
  return characters.every(character => string.includes(character));
}

mutation(["hello", "hey"]);
```

**Chunky Monkey**

```
function chunkArrayInGroups(arr, size) {
  const chunks = [];
  while (arr.length > 0)
    chunks.push(arr.splice(0, size));
  return chunks;
  // const chunks = [];
  // const numOfChunks = Math.ceil(arr.length / size);
  // for (let i = 0; i < arr.length; i += size) {
  //   const remainingItems = arr.length - i;
  //   if (remainingItems <= size) {
  //     const chunk = arr.slice(i);
  //     chunks.push(chunk);
  //   }
  //   const chunk = arr.slice(i, size);
  //   chunks.push(chunk);
  // }
  // console.log(chunks);
  // return chunks;
}

chunkArrayInGroups(["a", "b", "c", "d", "e"], 2);
```

### Day 33
**Tuesday, November 19, 2019**

Got to spend pretty much the entire day working on coding today. Not sure if you'd be able to tell that or not from what I accomplished but it was nice to have the entire day to dig into a handful of different things.

Spent the morning kind of assessing my goals for the week and took another stab at setting up some tests for Restock. With a little help from my buddy Dylan I realized I don't actually need a testing library for what I'm doing right now and was able to get some basic Jest tests going for my reducer file.

After that I spent a couple hours working on splitting up the Restock reducer logic into section and item reducers, and tried to break up the handleUpdateOrder function into some smaller and more specific functions. Got kinda lost in it all and started feeling frustrated so decided to step away for the day and try something else.

The last little bit of the day I spent working through more challenges on freeCodeCamp. Made it through 13 of the 16 challenges in the Basic Algorithm Scripting section, and saved all of my solutions as I went through to be able to look back at. Think I'll post them here for safe keeping..

**Convert Celsius to Fahrenheit**

```
function convertToF(celsius) {
  let fahrenheit = celsius * 9/5 + 32;
  return fahrenheit;
}

convertToF(30);
```

**Reverse a String**

```
function reverseString(str) {
  const arr = str.split('');
  const reversedStr = arr.reduceRight((acc, char) => {
    return acc + char;
  })
  return reversedStr;
}

reverseString("hello");
```

- **note:** could use .join(‘’) instead of .reduceRight(...)

**Factorialize a Number**

```
function factorialize(num) {
  if (num <= 1) return 1;
  return num * factorialize(num - 1);
}
console.log(factorialize(5));
factorialize(5);
```

**Find the Longest Word in a String**

```
function findLongestWordLength(str) {
  const arr = str.split(" ");
  let longestLength = 0;
  arr.map(word => {
    if (word.length > longestLength) {
      return longestLength = word.length;
    }
  });
  return longestLength;
}

findLongestWordLength("The quick brown fox jumped over the lazy dog");
```

**Return Largest Numbers in Arrays**

```
function largestOfFour(arr) {
  let largestNumbers = [];
  arr.map(subArray => {
    let largestNumber = subArray[0];
    subArray.map(number => {
      if (number > largestNumber) {
        largestNumber = number;
      }
    })
    largestNumbers.push(largestNumber);
  });
  return largestNumbers;
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

**Confirm the Ending**

```
function confirmEnding(str, target) {
  const charsFromEnd = str.length - target.length;
  const ending = str.substring(charsFromEnd);
  return ending === target;
}

confirmEnding("Bastian", "n");
```

**Repeat a String Repeat a String**

```
function repeatStringNumTimes(str, num) {
  if (num < 1) return "";
  return str + repeatStringNumTimes(str, num - 1);
}

repeatStringNumTimes("abc", 3);
```

**Truncate a String**

```
function truncateString(str, num) {
  if (str.length > num) {
    return str.substring(0, num) + "..."
  }
  return str;
}

truncateString("A-tisket a-tasket A green and yellow basket", 8);
```

- **note:** could also use a ternary, might look nicer

**Finders Keepers**

```
function findElement(arr, func) {
  return arr.find(element => func(element))
}

findElement([1, 2, 3, 4], num => num % 2 === 0);
```
**Boo who**

```
function booWho(bool) {
  // What is the new fad diet for ghost developers? The Boolean.
  return typeof bool === "boolean";
}

booWho(null);
```

**Title Case a Sentence**

```
function titleCase(str) {
  const words = str.split(" ");
  const capitalized = words.map(word => {
    const firstLetter = word.slice(0, 1).toUpperCase();
    const remainingLetters = word.slice(1).toLowerCase();
    return firstLetter + remainingLetters;
  })
  return capitalized.join(" ");
}

titleCase("I'm a little tea pot");
```

**Slice and Splice**

```
function frankenSplice(arr1, arr2, n) {
  const beginning = arr2.slice(0, n);
  const end = arr2.slice(n);
  const combined = [...beginning, ...arr1, ...end];
  return combined;
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

**Falsy Bouncer**

```
function bouncer(arr) {
  return arr.filter(item => !!item !== false);
}

bouncer([7, "ate", "", false, 9]);
```


### Day 32
**Monday, November 18, 2019**

Got to work on things for an hour or so before work this morning and an hour or so after dinner. Was able to complete the remaining lessons in the Basic Data Structures section, but not without a bit of trouble 😅

Had a really tough time for some reason with the *Check if an Object has a Property* and *Iterate Through the Keys of an Object with a for...in Statement* challenges. The first one I think I was making overly complicated by trying to use the wrong array method for the job (.map instead of .every) and not thinking about where I was placing my return values. For the for...in one, I was just being dumb and not realizng that user is a variable (see below) and therefore I should be using bracket notation. Here was my answer for the second challenge..

```
function countOnline(usersObj) {
  // change code below this line
  let onlineUsers = 0;
  for (let user in usersObj) {
    usersObj[user].online && onlineUsers++;
  }
  return onlineUsers;
  // change code above this line
}

```

I also think there were a few times I had issues just because I was forgetting the variable declaration keyword.. so um, just a reminder that you got use let or const 🙃


### Day 31
**Sunday, November 17, 2019**

Completed the freeCodeCamp section on Debugging and made it halfway through the following section on Basic Data Structures. Feels like review, but honestly never hurts to do a refresher on the array methods..

- `.push()`
	- takes one or more elements as parameters and adds those elements to the end of an array
	- returns the new length of the array
- `.unshift()`
	- takes one or more elements as parameters and adds those elements to the beginning of an array
	- returns the new length of the array
- `.pop()`
	- takes no parameters
	- removes a single element from the end of an array
	- returns the removed item 
- `.shift()`
	- takes no parameters
	- removes a single element from the beginning of an array
	- returns the removed item
- `.splice()`
	- removes any number of consecutive elements from anywhere in an array
	- takes up to 3 parameters
       	1. index at which to begin removing elements
        2. number of elements to remove
        3. element, or elements separated by commas, to add to the array in place of the removed elements
	- modifies the array it’s called on and also returns a new array containing the values of the removed elements
- `.slice()`
	- copies or extracts a given number of elements to a new array, leaving the array it was called upon untouched
	- takes 2 parameters
        1. index at which to begin extraction
        2. index at which to stop extraction *(extraction will occur up to, but not including the element at this index)*
- `.indexOf()`
	- checks for the presence of an element in an array
	- takes an element as a parameter
	- returns the position (index) of the given element, or -1 if the element does not exist in the array


### Day 30
**Saturday, November 16, 2019**

Completed the remaining 20 challenges in the Regular Expressions section 🏆

Some takeaways..

- `^` = matches pattern at the beginning of a string
- `$` = matches pattern at the end of a string
- `\w` = [A-Za-z0-9_] (matches all letters, numbers, and underscore)
- `\W` = [^A-Za-z0-9_] (matches everything but letters, numbers, and underscore)
- `\d` = [0-9] (matches all numbers)
- `\D` = [^0-9] (matches everything but numbers)
- `\s` = [ \r\t\f\n\v] (matches all whitespace characters)
- `\S` = [^ \r\t\f\n\v] (matches everything but whitespace characters)
- `?` = checks for zero or one of the preceding element
- `?=` = positive lookahead
- `?!` = negative lookahead

Also spent a little time trying out some tests with both react-native-testing-library and native-testing-library.. still not sure what to do about all of that.

Pretty wiped out after 30 days straight, but feeling really motivated and excited to keep at it. See ya tomorrow.


### Day 29
**Friday, November 15, 2019**

Still scratching my head a bit about the React Native testing stuff. Followed the Expo docs on setting up Jest, but am still a bit confused about whether to use react-native-testing-library or native-testing-library. Did some reading and enjoyed these couple articles:

- [Why Native Testing Library Exists - Brandon Carroll - Medium](https://medium.com/@brandoncarroll/why-native-testing-library-exists-629ffb85cae2)
- [Hello, Native Testing Library 👋 - Brandon Carroll - Medium](https://medium.com/@brandoncarroll/hello-native-testing-library-91ea326ea0f5)

Also worked through a handful more lessons in the freeCodeCamp Regex section. Learned about:

- `.` — wildcard character
- `[0-9a-z]` - character class/set
- `[^0-9a-z]` — negated character class/set
- `+` — character or set occurs one or more times
- `*` — character or set occurs zero or more times


### Day 28
**Thursday, November 14, 2019**

Completed the rest of the section on ES6 for the Algorithms and Data Structures Certification on freeCodeCamp 🏆 Started working through a few lessons in the next section on Regular Expressions.

Also spent some time looking into a few different improvements to the Restock project. Played around with adding a clear button ('x') that appears after an item is checked off on the Checklist screen. Mostly got it working but something is up with the way that the item count is rendered in the Counter componenets on the Form screen. Gotta look into that more.

Briefly took a look at Dylan's suggestion of splitting up my reducer logic into section and item reducers. Not quite sure yet how to do this with hooks as compared to combineReducers in Redux, but did find this article to maybe dig into further..

- [Build Your Own React-Redux Using useReducer and useContext Hooks](https://blog.bitsrc.io/build-our-own-react-redux-using-usereducer-and-usecontext-hooks-a5574b526475)

Spent some time as well looking into setting up testing for Restock, but ran out of brain-energy after a bit. Here are the tabs I had open on getting things set up with React Native Testing Library/Jest/Expo..

- [Install · React Native Testing Library](https://www.native-testing-library.com/docs/install)
- [react/packages/react-test-renderer at master · facebook/react](https://github.com/facebook/react/tree/master/packages/react-test-renderer)
- [Configuring Jest · Jest](https://jestjs.io/docs/en/configuration)
- [Testing with Jest - Expo Documentation](https://docs.expo.io/versions/latest/guides/testing-with-jest/)


### Day 27
**Wednesday, November 13, 2019**

Finished the last two challenges on recursion in the Basic JavaScript section and feel like I'm getting a better handle on writing and understanding recursive functions.

Broke down this function from the second to last challenge to help illustrate what each recursive call does..

```
function rangeOfNumbers(startNum, endNum) {
  if (endNum === startNum) {
    return [endNum];
  } else {
    var numbers = rangeOfNumbers(startNum, endNum - 1);
    numbers.push(endNum);
    return numbers;
  }
};
```

**Q:** What happens when we call `rangeOfNumbers(1, 5)`?

- rangeOfNumbers(1, 5)
	- OR ➡️ `rangeOfNumbers(1, 5 -1).push(5);`
	- OR ➡️ `rangeOfNumbers(1, 4).push(5);`
	- OR ➡️ `[1, 2, 3, 4].push(5);`
	- OR ➡️ `[1, 2, 3, 4, 5];`
- rangeOfNumbers(1, 4)
	- OR ➡️ `rangeOfNumbers(1, 4 -1).push(4);`
	- OR ➡️ `rangeOfNumbers(1, 3).push(4);`
	- OR ➡️ `[1, 2, 3].push(4);`
	- OR ➡️ `[1, 2, 3, 4];`
- rangeOfNumbers(1, 3)
	- OR ➡️ `rangeOfNumbers(1, 3 -1).push(3);`
	- OR ➡️ `rangeOfNumbers(1, 2).push(3);`
	- OR ➡️ `[1, 2].push(3);`
	- OR ➡️ `[1, 2, 3];`
- rangeOfNumbers(1, 2)
	- OR ➡️ `rangeOfNumbers(1, 2 - 1).push(2);`
	- OR ➡️ `rangeOfNumbers(1, 1).push(2);`
	- OR ➡️ `[1].push(2);`
	- OR ➡️ `[1, 2];`
- rangeOfNumbers(1, 1)
	- OR ➡️ `[1];`

Also started working through the next seciton on ES6. It was kinda wild to see how many of the new features introduced in ES6 are so core to how I write JavaScript. Lessons so far have been mostly on var/let/const, arrow functions, spread, rest, and destructuring. A few quick take aways:

- When you declare a variable with `var`, it is declared globally, or locally if declared inside a function.
- When you declare a variable with `let` inside a block, statement, or expression, its scope is limited to that block, statement, or expression.
- Any attempt at changing the object passed to Object.freeze() will be rejected without an error.
- Destructuring allows you to assign a new variable name when extracting values. You can do this by putting the new name **after** a colon when assigning the value. 🆒🆕

```
const user = { name: 'John Doe', age: 34 };
const { name: userName, age: userAge } = user;
// userName = 'John Doe', userAge = 34
```



### Day 26
**Tuesday, November 12, 2019**

Finished reading and taking notes on the "Learn and Understand Recursion" article from yesterday. Still wrapping my head around writing recursive functions but I think the general ideas are there.

Also worked through a handful more chanllenges on freeCodeCamp.. today they were mostly focused on recursion, parseInt(), and Math.random(). Got pretty hung up on [this challenge](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-javascript/profile-lookup) and after toiling for a long while, I ended up reading the challenge hints and realized I had everything correct except for the placement of one of the return statements. I think the way I had it initially was causing the for loop to return early, so not every item in the array was actually being looped over.

Ultimately this caused me to realize that I think I've had some misconceptions about for loops + .map().. thinking that they were the exact same thing (they're not) and that neither could return early (they can, or at least the for loop can). Either way, I think I gotta do some more solidifying/resetting my understanding of for loops!

Keepin' at it tomorrow. Will definitely be able to wrap up the Basic JavaScript section, only two challenges left! 🏆

Ciao!


### Day 25
**Monday, November 11, 2019**

Worked through a few freeCodeCamp challenges on looping and recursion this morning before work. Also spent a little time with this article on recursion.. [Learn and Understand Recursion in JavaScript - codeburst](https://codeburst.io/learn-and-understand-recursion-in-javascript-b588218e87ea)

A few takeaways from the first half of the article:
- **recursion** ➡️ when a function calls itself
- **factorial** ➡️ the product of an integer and all the integers below it

- The three key features of recursion:
	- 1️⃣ A Termination Condition — *if (something bad happened) { STOP };*
	- 2️⃣ A Base Case — *if (this happens) { Yay! We're done };*
	- 3️⃣ The Recursion — *our function calling itself*
	
More on all of this tomorrow... Ciao!


### Day 24
**Sunday, November 10, 2019**

Spent a couple hours working on the freeCodeCamp stuff tonight after work. Made it through 25 more challenges, so that puts us at 92/110 completed. 18 more to go in this section! 🏆

The last challenge I worked on tonight (Record Collection) really threw me for a loop for some reason, maybe I'm just tired.. Either way, I saved my solution to be able to look back at. Took a while to get there (was trying to approach it all out of order for a while 😞) but I'm proud of myself for pushing through and happy with the intent around the function names (trying to practice what I've been reading in *Clean Code*). Still plenty of room to improve, but moving in the right direction. Here's my solution..

```
// Setup
var collection = {
  2548: {
    album: "Slippery When Wet",
    artist: "Bon Jovi",
    tracks: [
      "Let It Rock",
      "You Give Love a Bad Name"
    ]
  },
  2468: {
    album: "1999",
    artist: "Prince",
    tracks: [
      "1999",
      "Little Red Corvette"
    ]
  },
  1245: {
    artist: "Robert Palmer",
    tracks: [ ]
  },
  5439: {
    album: "ABBA Gold"
  }
};

// Only change code below this line
function propIsTracks(prop) {
  return prop === "tracks";
}

function entryHasTracksProp(id) {
  return collection[id].hasOwnProperty("tracks");
}

function valueIsEmptyString(value) {
  return value === "";
}

function updateRecords(id, prop, value) {
  if (valueIsEmptyString(value)) {
    delete collection[id][prop];
  } else {
    if (propIsTracks(prop)) {
      if (!entryHasTracksProp(id)) {
        collection[id].tracks = [];
      }
      collection[id].tracks.push(value);
    } else {
      collection[id][prop] = value;
    }
  }
  return collection;
}

// Alter values below to test your code
updateRecords(5439, "artist", "ABBA");
```


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
