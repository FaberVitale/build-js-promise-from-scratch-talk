---
theme: default
background: /assets/barn-images-t5YUoHW6zRo.jpg
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: Let's build Ecmascript Promise from scratch
mdc: true
---

<h1 class="drop-shadow-2xl">Let's build JS Promise from scratch</h1>

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer space-next-button" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<h2 class="drop-shadow-2xl absolute bottom-4 right-4">Speaker: Fabrizio A. Vitale</h2>

---
layout: image-right
image: /assets/maxim-shklyaev-RThOx_CsT-Q-unsplash.jpg
---

<style>
  li {
    font-size: 1.5rem;
  }
</style>

# Goals

<div class="slide-big-list mt-8">
<v-clicks>

- Build a [Promise A+](https://promisesaplus.com/) spec compliant Promise in Typescript.
- Learn Javascript Promises.
- Have fun!

</v-clicks>
</div>

---
transition: fade
---

<style>
  code {
    font-size: 1rem;
  }
</style>

# From

<div class="code-block-1 w-full h-full flex justify-center align-center">

```ts
export class PinkyPromise<T> { 
  // TODO add implementation
}
```

</div>

---

# To

```ts
/**
 * A [Promise A+](https://promisesaplus.com/) spec compliant implementation written in typescript.
 */
export class PinkyPromise<T> implements PromiseLike<T> {
  #state: PromiseState<T> = { status: "pending" };
  #fulfilledListeners: ((value: unknown) => unknown)[] = [];
  #rejectedListeners: ((reason: unknown) => unknown)[] = [];

  constructor(executor: PinkyPromiseExecutor<T>) {
    if (typeof executor !== "function") {
      throw new TypeError(`${executor} is not a function`);
    }

    try {
      executor(this.#resolve, this.#reject);
    } catch (executorError) {
      this.#reject(executorError);
    }
  }

  /**
   * Notifies the subscribed listeners if the promise is settled.
   */
  #notify() {
  // Continue...
}
```

---
layout: center
---

# Ready?

---
layout: image-right
image: /assets/kelly-sikkema-hqjxQ3sqTW8-unsplash.jpg
transition: none
---

# So... what's a Promise?

<v-click>
<div class="my-12"> 
<p class="text-4xl !leading-10">A promise is an object that represents the eventual result of an asynchronous operation.</p>
</div>
</v-click>

---
layout: image-right
image: /assets/kelly-sikkema-hqjxQ3sqTW8-unsplash.jpg
transition: slide-left
---

# So... what's a Promise?

<div class="my-12"> 
<p class="text-4xl !leading-10">Allows the subscription of listeners to the result of an asynchronous action or the reason why it has failed.</p>
</div>

---
layout: image-right
image: /assets/julie-molliver-Z3vFp7szCAY-unsplash.jpg
transition: none
---

# A bit of history / 1

<div class="slide-big-list">
<v-clicks>

- Based on paper written in the [1976](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.295.9692) by Daniel P. Friedman, David, S. Wise.
- Popularized in Javascript by [JQuery](https://api.jquery.com/category/deferred-object/) in [2011](https://blog.jquery.com/2011/01/31/jquery-15-released/).
- A Promise implementation, called [Promises](https://github.com/nodejs/node/commit/7cd09874), is committed in 2009 in Node by Ryan Dahl.

</v-clicks>
</div>

---
layout: image-right
image: /assets/julie-molliver-Z3vFp7szCAY-unsplash.jpg
transition: slide-left
---

# A bit of history / 2

<div class="slide-big-list">
<v-clicks>

- A specification, [Promise A+](https://promisesaplus.com/) written by [Domenic Denicola](https://github.com/domenic) was created in 2012.
- Standardized in ECMAScript20215 (ES6) in 2015.

</v-clicks>
</div>

---
layout: center
---

# Promise state


---
layout: image-right
image: /assets/kelly-sikkema-hqjxQ3sqTW8-unsplash.jpg
---

# Promise state

<div class="my-12 text-2xl"> 
A Promise is in one of these states:


<v-clicks>

- <div class="p-0 mx-0 mt-4 mb-2"><strong class="text-3xl">😴 pending</strong>: initial state</div>
- <div class="p-0 mx-0 mb-2"><strong class="text-3xl">✅ fulfilled</strong>: the operation was completed successfully.</div>
- <div class="p-0 mx-0 mb-2"><strong class="text-3xl">❌ rejected </strong>: the operation failed.</div>

</v-clicks>

</div>

<div class="absolute bottom-8 left-12">

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#description)

</div>

---

# Promise state

<div class="flex justify-center">
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 770.9943594734391 390" width="770.9943594734391" height="390" filter="invert(93%) hue-rotate(180deg)">
  <!-- svg-source:excalidraw -->
  
  <defs>
    <style class="style-fonts">
      @font-face {
        font-family: "Virgil";
        src: url("https://excalidraw.com/Virgil.woff2");
      }
      @font-face {
        font-family: "Cascadia";
        src: url("https://excalidraw.com/Cascadia.woff2");
      }
    </style>
    
  </defs>
  <rect x="0" y="0" width="770.9943594734391" height="390" fill="#ffffff"/><g stroke-linecap="round" transform="translate(15.058324904552535 170) rotate(0 88 62.5)"><path d="M31.25 0 C54.79 -0.71, 78.71 1.11, 144.75 0 M31.25 0 C70.39 1.01, 107.21 0.49, 144.75 0 M144.75 0 C164.46 0.5, 177.16 9.57, 176 31.25 M144.75 0 C165.17 0.03, 175.08 10.11, 176 31.25 M176 31.25 C174.03 54.01, 175.15 74.67, 176 93.75 M176 31.25 C174.7 52.41, 176.64 74.58, 176 93.75 M176 93.75 C176.51 115.99, 166.27 124.61, 144.75 125 M176 93.75 C175.94 116, 165.58 124.55, 144.75 125 M144.75 125 C109.63 123.7, 73.05 124.57, 31.25 125 M144.75 125 C122.78 125.92, 99.35 124.65, 31.25 125 M31.25 125 C9.71 124.09, -0.59 114.15, 0 93.75 M31.25 125 C12.28 124.69, 1.19 113.5, 0 93.75 M0 93.75 C-2.23 72.53, -0.82 54.13, 0 31.25 M0 93.75 C0.57 75.16, 0.04 57.22, 0 31.25 M0 31.25 C-1.28 10.3, 10.79 -1.1, 31.25 0 M0 31.25 C-2.21 10.49, 11.62 -1.88, 31.25 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(47.058324904552535 210) rotate(0 61.766666412353516 22.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Pending</text></g><g mask="url(#mask-f3lqn7OfYFljtZ7P1XqBi)" stroke-linecap="round"><g transform="translate(193.05832490455253 240) rotate(0 178.86352691351902 -40.87955196833326)"><path d="M-0.11 -0.14 C28.33 -9.55, 111.18 -43.86, 171.09 -57.13 C231 -70.4, 328.05 -76, 359.36 -79.73 M-1.63 -1.26 C26.7 -10.42, 110.64 -42.27, 170.71 -55.66 C230.78 -69.05, 327.38 -77.31, 358.78 -81.62" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(193.05832490455253 240) rotate(0 178.86352691351902 -40.87955196833326)"><path d="M331.95 -68.19 C340.87 -70.9, 349.98 -76.66, 358.78 -81.62 M331.95 -68.19 C342.73 -72.52, 351.04 -78.27, 358.78 -81.62" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(193.05832490455253 240) rotate(0 178.86352691351902 -40.87955196833326)"><path d="M329.6 -88.58 C339.2 -84.38, 349.1 -83.24, 358.78 -81.62 M329.6 -88.58 C341.24 -85.32, 350.42 -83.45, 358.78 -81.62" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-f3lqn7OfYFljtZ7P1XqBi"><rect x="0" y="0" fill="#fff" width="652.0583249045526" height="420.61489387867215"/><rect x="301.74165925513853" y="160.5" fill="#000" width="122.63333129882812" height="45" opacity="1"/></mask><g transform="translate(301.74165925513853 160.5) rotate(0 70.18019256293309 38.62044803166674)"><text x="61.31666564941406" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">resolve</text></g><g mask="url(#mask-v6Klh140ZchNSEboBSH4O)" stroke-linecap="round"><g transform="translate(189.05832490455253 237) rotate(0 180.5652201659372 39.23132434973522)"><path d="M1.11 0.92 C29.24 9.79, 108.43 40.71, 168.4 53.58 C228.36 66.44, 328.94 74.2, 360.9 78.11 M0.23 0.35 C28.27 8.85, 107.68 39.1, 167.66 51.76 C227.63 64.42, 328.03 71.89, 360.08 76.29" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(189.05832490455253 237) rotate(0 180.5652201659372 39.23132434973522)"><path d="M330.94 83.43 C340.28 81.9, 347.55 79.15, 360.08 76.29 M330.94 83.43 C342.61 81.69, 354 78.08, 360.08 76.29" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(189.05832490455253 237) rotate(0 180.5652201659372 39.23132434973522)"><path d="M333.17 63.03 C341.96 67.01, 348.63 69.74, 360.08 76.29 M333.17 63.03 C343.84 69.1, 354.38 73.3, 360.08 76.29" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-v6Klh140ZchNSEboBSH4O"><rect x="0" y="0" fill="#fff" width="650.0583249045526" height="414.11738878957055"/><rect x="304.52499207984556" y="267.5" fill="#000" width="105.06666564941406" height="45" opacity="1"/></mask><g transform="translate(304.52499207984556 267.5) rotate(0 65.09855299064424 8.731324349735218)"><text x="52.53333282470703" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">reject</text></g><g stroke-linecap="round" transform="translate(559.0583249045526 107.5) rotate(0 100 54.5)"><path d="M27.25 0 C62.36 -1.08, 93.89 -1.58, 172.75 0 M27.25 0 C79.62 -1.67, 131.67 -1.16, 172.75 0 M172.75 0 C189.81 -0.07, 198.16 7.81, 200 27.25 M172.75 0 C192.47 -2.17, 199.8 10.68, 200 27.25 M200 27.25 C199.32 46.46, 201.21 67.84, 200 81.75 M200 27.25 C200.59 47.35, 200.69 68.44, 200 81.75 M200 81.75 C199.83 100.62, 192.45 109.79, 172.75 109 M200 81.75 C199.67 99.96, 192.55 110.9, 172.75 109 M172.75 109 C128.42 110.58, 83.88 110.13, 27.25 109 M172.75 109 C117.27 108.22, 60.18 107.55, 27.25 109 M27.25 109 C10.72 107.03, 0.23 101.33, 0 81.75 M27.25 109 C8.59 109.03, -1.12 97.95, 0 81.75 M0 81.75 C-0.02 68.36, 1.32 57.04, 0 27.25 M0 81.75 C-0.44 68.66, -1.31 53.47, 0 27.25 M0 27.25 C-0.55 10.26, 9.85 1.55, 27.25 0 M0 27.25 C1.98 11.13, 7.5 1.04, 27.25 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(582.0583249045526 139) rotate(0 67.55000305175781 22.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Fulfilled</text></g><g stroke-linecap="round" transform="translate(556.0583249045526 264) rotate(0 100 58)"><path d="M29 0 C64.48 1.69, 104.3 0.74, 171 0 M29 0 C75.49 0.12, 120.08 -0.79, 171 0 M171 0 C191.69 1.97, 200.6 8.17, 200 29 M171 0 C189.65 -0.03, 198.75 10.14, 200 29 M200 29 C198.57 46.11, 201.14 64.41, 200 87 M200 29 C200.33 50.03, 200.82 73.09, 200 87 M200 87 C201.79 106.13, 190.73 116.02, 171 116 M200 87 C199.67 107.11, 192.12 114.47, 171 116 M171 116 C118.81 116.64, 69.46 116.16, 29 116 M171 116 C132.28 116.5, 94.55 116.51, 29 116 M29 116 C8.54 117.51, 0.81 104.75, 0 87 M29 116 C8.89 117.97, 1.8 106.96, 0 87 M0 87 C-1.2 65.43, -1.33 41.65, 0 29 M0 87 C0.75 69.58, 0.81 53.26, 0 29 M0 29 C1.56 9.72, 8.87 1.3, 29 0 M0 29 C-1.25 10.2, 11.69 0.37, 29 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(581.0583249045526 297) rotate(0 77.0999984741211 22.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Rejected</text></g><g transform="translate(581.0583249045526 10) rotate(0 65.96666717529297 22.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Settled</text></g><g transform="translate(44.058324904552535 16) rotate(0 61.766666412353516 22.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="36px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Pending</text></g><g stroke-linecap="round"><g transform="translate(10.058324904552535 79) rotate(0 375.438854832167 0.4259681125214314)"><path d="M0.08 -0.02 C125.23 0.13, 625.78 0.71, 750.94 0.88 M-0.06 -0.15 C125.07 0.01, 625.62 0.79, 750.79 1" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask/>
</svg>
</div>

---
transition: fade
---

# Promise state implementation / 1

```ts
/**
 * A Promise is in one of these states:
 *
 * - pending: initial state, neither fulfilled nor rejected.
 * - fulfilled: meaning that the operation was completed successfully.
 * - rejected: meaning that the operation failed.
 */
type PromiseState<T> =
  | { readonly status: "pending" }
  | { readonly status: "fulfilled"; readonly value: T }
  | { readonly status: "rejected"; readonly reason: any };

export class PinkyPromise<T> {
  #state: PromiseState<T> = { status: "pending" };
}
```

---

# Promise state implementation / 2

```ts
export class PinkyPromise<T> {
  #state: PromiseState<T> = { status: "pending" };

  /**
   * Settles promise, transitioning it from state pending to either `fulfilled` or `rejected`.
   */
  static #transition(
    pinkyPromise: PinkyPromise<any>,
    state: PromiseState<any>,
  ) {
    if (pinkyPromise.#state.status === "pending") {
      pinkyPromise.#state = state;
    }
  }
}
```

---
layout: center
transition: fade
---

# Create a Promise

---
layout: image-right
image: /assets/julie-molliver-Z3vFp7szCAY-unsplash.jpg
transition: fade
---

# JS Promise constructor spec

<v-clicks>

- Throws error if it's invoked without `new`.
- Throws error if input executor is not a function.
- Synchronously calls the input executor with `resolve` and `reject`.
- Rejects the promise if input executor raises an error.

</v-clicks>

<div class="absolute bottom-8 left-12">

[Spec](https://tc39.es/ecma262/multipage/control-abstraction-objects.html#sec-promise-objects) - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)

</div>

---
transition: slide-left
---

# JS Promise constructor implementation

```ts {all|1|5-7|9-23}
export class PinkyPromise<T> {
  #state: PromiseState<T> = { status: "pending" };

  constructor(executor: PinkyPromiseExecutor<T>) {
    if (typeof executor !== "function") {
      throw new TypeError(`${executor} is not a function`);
    }

    try {
      executor(this.#resolve, this.#reject);
    } catch (executorError) {
      this.#reject(executorError);
    }
  }

  #transition(pinkyPromise: PinkyPromise<any>, state: PromiseState<any>) { /* implementation */ }

  #resolve = (value: T | PromiseLike<T>): void => {
    PinkyPromise.#transition(this, { status: "fulfilled", value });
  };
  #reject = (reason: unknown): void => {
    PinkyPromise.#transition(this, { status: "rejected", reason });
  };
}
```

---
layout: center
transition: slide-left
---

# Notify Promise resolution
