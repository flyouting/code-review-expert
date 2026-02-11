# Frontend (Vue/TS) Code Review Checklist

Use this checklist when reviewing `.vue`, `.ts`, `.js`, or `.tsx` files. Focus on reactivity, component lifecycle, and browser performance.

## 1. Vue Reactivity & State
- **Prop Mutation**: Are `props` being mutated directly? This violates one-way data flow. Suggest emitting events.
- **Reactivity Loss**: Are reactive objects (props, state) destructured without `toRefs()`? This kills reactivity.
- **Ref vs Reactive**: Is `ref` used for primitives and `reactive` for objects? (Or consistent usage based on team style).
- **Computed Side Effects**: Do `computed` properties have side effects (e.g., API calls, DOM manipulation)? They must be pure.

## 2. Component Lifecycle & Cleanup
- **Event Listeners**: Are global event listeners (`window.addEventListener`) or timers (`setInterval`) removed in `onUnmounted`?
- **Memory Leaks**: Check for detached DOM nodes or large datasets retained in closed components.

## 3. Template & Rendering Performance
- **v-for Keys**: Does `v-for` have a unique `:key`? **Flag** if `index` is used as key for mutable lists.
- **v-if vs v-show**: Is `v-if` used for elements that toggle frequently (expensive)? Suggest `v-show`.
- **v-if with v-for**: Are `v-if` and `v-for` used on the same element? This is an antipattern. Suggest using a computed property to filter the list first.

## 4. TypeScript & Best Practices
- **Any Type**: Flag usage of `any`. Suggest specific interfaces or `unknown`.
- **Async/Await**: Is error handling (`try/catch`) present for async API calls?
- **Direct DOM Access**: Is `document.getElementById` or `querySelector` used inside Vue? Suggest using `ref` template refs.

## 5. Security (Frontend)
- **XSS**: Flag usage of `v-html`. Is the content sanitized (e.g., DOMPurify)?
- **Dependencies**: Are sensitive keys (AWS secrets, private API keys) visible in the client-side code?