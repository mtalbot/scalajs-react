scalajs-react [neo]
=============

[![Build Status](https://travis-ci.org/japgolly/scalajs-react.svg?branch=master)](https://travis-ci.org/japgolly/scalajs-react)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/japgolly/scalajs-react?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This branch is where a [redesign](https://github.com/japgolly/scalajs-react/issues/259) is currently taking place.
At the moment, it's all in a new module called [`neo`](neo/src).

The v0.x.y series started as an experiment and grew organically from there.
As such, it has accrued a number of annoyances and obstacles to desired improvements,
that can now only be solved by a redesign.
This begins a v1.x.y series and will begin with **v1.0.0**.

Contributions welcome.

# Done

- Component interfaces that allow any kind (JS, Scala, Clojure) of React component to be used generically.

  This is really awesome because it allows a component to declare access to a subset of any component's state as long as it is a certain type, then satisfy it using Monocle to zoom in and/or transform along the way, even if its a JS component.

  - Allow (any kind of) constructor transforms.
  - Allow (any kind of) props transforms.
  - Allow (any kind of) state transforms.

- Better Constructors
  - Agnostic to underlying implementation of component (JS, Scala, etc.)
  - Don't ask for non-existent or singleton props.
  - Depending on component, either don't ask for children, or ensure children are specified.
  - Allows possibility of even more children type-safety such as requiring exactly one child.
  - Input can be transformed.
  - Output can be transformed.

- More transparency. No more magic.
  - A separate `.raw` package that contains the React JS facade (without any Scala niceness).
  - All components expose their raw JS types.
  - All Scala components expose their underlying JS components.
  - It should be trivial to reuse `scalajs-react` components in other React libraries, and vice-versa.

- `JsComponent` - Import React components written in pure JS.
  ([test JS](neo/src/test/resources/component-es3.js) & [test Scala](neo/src/test/scala/japgolly/scalajs/react/JsComponentTest.scala))

  Importing a JS component is now a one-liner.
  ```scala
  val Component = JsComponent.byName[JsProps, AcceptsChildren, JsState]("ReactXYZ")
  ```

- Type-safety for JS components that expose ad-hoc methods once mounted.

- `JsFnComponent` - Import React functional components written in pure JS.
  ([test JS](neo/src/test/resources/component-fn.js) & [test Scala](neo/src/test/scala/japgolly/scalajs/react/JsFnComponentTest.scala))

- `ScalaFnComponent` - Create React functional components in Scala.

- Safe `PropsChildren` type and usage.

- Consistency wrt wrapping typed effects. Eg. `BackendScope.getDOMNode` should be `Callback`/direct just like everything else.


# In Progress

- Create React components in Scala.

  Currently `ScalaComponent` and a basic `ScalaComponentB` similar to `ReactComponentB` exist and work (with tests).
  Not sure if 'ole `ReactComponentB` was the best idea - it does work very well - but it would be nice to explore alternatives.

  (Note: if for backwards-compatibility and nothing else, `ReactComponentB` will be kept around for a long while. If a nice alternative appears then both methods will coexist.)

- Virtual DOM.

  This will be a major revision that will improve safety, ease of use, and readability.

  See [NEO-VDOM.md](NEO-VDOM.md) for detail.

# Pending

- Integration with all the `.extra` awesomeness. (Because most of it has been tremendously useful in real-world code.)
- Maybe a new means of declaring Scala mixins.
- Static and dynamic props (for Scala components).
- Simplify `Scalaz` and `Monocle` modules.
- Functional components don't allow refs. Current `CtorType` doesn't support this.
- Refs.
  - Stringly typed.
  - DOM/component type of target may differ between declaration and assignment.

# Maybe

- Component DOM/`getDOMNode` types. Currently none.
  - Having `N <: TopNode` all over the place was annoying.
  - Could add manually like before.
  - Would be nice to auto-detected based on `.render` result. Introduces circular dependency wrt `BackendScope` tho.

- Anything ES6-related should be easy to add now. Please contribute if interested.
  - Facades over ES6-based JS classes. (I tried briefly but didn't get the JS working.)
  - Scala-based ES6-based classes. Because it's important to some people. (Apparently its faster but I'm yet to see any benchmarks or other evidence supporting this.)
  - Once the above works, it would be good to be able to choose a backend type for `ReactComponentB`.

- Add a `Cats` module too? Contribution welcome.
