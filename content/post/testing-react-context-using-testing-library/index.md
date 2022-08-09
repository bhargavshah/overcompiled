---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Declarative testing patterns for React Context"
subtitle: Using react testing library to test React context
summary: Use function currying and composition for clean and fast test setup for React Context using react testing library
tags: ["jest", "react", "TDD"]
categories: ['React', 'TDD']
date: "2022-03-03T18:04:09.685Z"
lastmod: "2022-08-09T18:04:09.685Z"
featured: false
draft: false
image:
  filename: featured.jpeg
  focal_point: Smart
  preview_only: false
---
When Context API was announced the React ecosystem was taken by storm. There was now a native solution to solve the problems of prop drilling, state management, etc. Everything was great!

Except, testing it.

Testing components (using [react testing library](https://testing-library.com/docs/react-testing-library/intro/) or otherwise) that were context consumers was a real pain, because of the sheer effort required to setup context in an isolated unit test suite. Not to mention the amount a boilerplate code required to set it up, which added no value to the reader of that test, including the author itself.

Consider the below React app with the 2 context providers `UserProvider` and `SessionProvider`,
<iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2FApp.js&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

Somewhere deep in the component hierarchy these contexts are consumed by the `Level2` component,
<iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2FLevel2.js&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

The hooks `useUser` and `useSession` internally `useContext` hook. This way of consuming context is popularized by Kent C Dodds in this [blog post](https://kentcdodds.com/blog/how-to-use-react-context-effectively).

If you wanted to test `Level2` component following the approach in [official testing library docs](https://testing-library.com/docs/example-react-context/) you can do so like this,
<iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2FLevel2.test.js&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

So what is wrong with the above code?

Nothing!

But imagine if components in your app consume more than 5 context providers and some of them needed custom context values in your tests. How would this serve you? Not very well. It would require you to create this wrapper tree for each of your test suite, which is not very expressive.

Another extreme is creating a render helper that always has all the providers in the app and using that for testing every component. This is also not ideal, because your component renders differently than it does in your real app so it reduces the confidence that your tests give you. This also makes passing custom context values to your context providers messy and your render-all-context-providers helper becomes a victim of too much configuration.

It would be nice to have a middle way,

<iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2FLevel2Alt.test.js&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

This way, you still have to create a helper render method, but it is so lean! Passing custom context values is done via the `withProps` helper. Perfect balance of expressiveness and configuration! 💯

# How to do this?
There is a one time setup involved,

1. Copy the code that enables this render composition,
   <iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2Ftest-utils%2Fcustom-render.jsx&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

2. Create test wrappers for your context providers
   <iframe src="https://codesandbox.io/embed/context-testing-0e9nye?fontsize=14&hidenavigation=1&module=%2Fsrc%2Ftest-utils%2Fwrappers.jsx&theme=dark&view=editor"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Context Testing"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

   The ones that need custom context values can allow so `withProps` pattern. This is needed only once for every context provider.

3. In your test suite, use `composedRender` from step #1 and test wrappers from step #2 and create a render function that is decorated with a tree of the context wrappers.
   ```typescript
      const render = composedRender(
         UserProviderWrapper.withProps({ value: { name: "Bernie" } }),
         SessionProviderWrapper
      );
   ```

4. In your test suite, use `render` like you would use it if it was imported from `@testing-library/react`
   ```typescript
      describe("<Level2 />", () => {
         test("should display logged in user", () => {
            render(<Level2 />);
            screen.getByText(/Logged in as Bernie/);
         });
      }
   ```

You can see all of the above working together on this [codesandbox](https://codesandbox.io/s/context-testing-0e9nye?file=/src/Level2Alt.test.js).


And that's it, quick tests at the cost of a very small setup! If you have other patterns that work well, do share them with me.
