---
theme: default
background: /assets/barn-images-t5YUoHW6zRo.jpg
class: text-center
highlighter: shiki
lineNumbers: true
colorSchema: dark
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

<div class="absolute top-8 right-14">

[source <uim-github/> ](https://github.com/FaberVitale/pinky-promise)

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

<div class="absolute top-8 right-14">

[source <uim-github/> ](https://github.com/FaberVitale/pinky-promise)

</div>

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

# Promise states


---
layout: image-right
image: /assets/kelly-sikkema-hqjxQ3sqTW8-unsplash.jpg
---

# Promise states

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

# Promise states

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

# Promise states implementation / 1

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

# Promise states implementation / 2

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

# JS Promise constructor

<v-clicks>

- Throws error if it's invoked without `new`.
- Throws error if input executor is not a function.
- Synchronously calls the input with `resolve` and `reject`.
- Rejects the promise if executor raises an error.

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

---
layout: image-right
image: /assets/maxim-shklyaev-RThOx_CsT-Q-unsplash.jpg
transition: fade
---

# .then() method


<v-clicks>

- Notifies when a Promise settles either `onFulfill(value)` or `onRejection(reason)` once.
- Returns the unwrapped output of the invoked promise in a new Promise.
- The callbacks are always invoked asynchronously in a microtask.

</v-clicks>


---
transition: fade
---

# .then() method

<div class="flex justify-center">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 0 794.2747856604276 660.5" width="1588.5495713208552" height="751" filter="invert(93%) hue-rotate(180deg)">
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
  <rect x="0" y="0" width="794.2747856604276" height="375.5" fill="#ffffff"/><g stroke-linecap="round" transform="translate(10.11881576843956 181.5) rotate(0 73.5 36)"><path d="M18 0 C49.63 1.24, 81.12 0.07, 129 0 M18 0 C59.87 -1.84, 103.39 -0.39, 129 0 M129 0 C141.34 -0.32, 148.52 5.91, 147 18 M129 0 C142.62 -1.94, 145.19 5.35, 147 18 M147 18 C145.54 33.68, 147.35 47.07, 147 54 M147 18 C147.58 30.01, 147.4 40.79, 147 54 M147 54 C145.07 67.92, 142.38 73.62, 129 72 M147 54 C146.66 65.17, 141.93 71.45, 129 72 M129 72 C101.6 70.6, 72.07 73.25, 18 72 M129 72 C105.5 72.12, 82.58 70.6, 18 72 M18 72 C4.9 72.36, -0.39 64.22, 0 54 M18 72 C7.81 73.93, -0.77 66.07, 0 54 M0 54 C-0.3 48.27, 1.38 37.8, 0 18 M0 54 C0.07 47.24, -0.29 39.15, 0 18 M0 18 C1.2 5.71, 5.72 -0.8, 18 0 M0 18 C-1.36 4.46, 7.34 0.84, 18 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(36.11881576843956 202.5) rotate(0 50.20000076293945 17.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="28px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Promise</text></g><g mask="url(#mask-Vx9rt5D3Q9rYhKxmffdow)" stroke-linecap="round"><g transform="translate(160.11881576843956 214.89905611180131) rotate(0 77.02220231874844 -27.995987935350627)"><path d="M0.11 -0.52 C9.31 -6.27, 30.44 -24.34, 55.92 -33.89 C81.4 -43.44, 136.63 -54.01, 152.98 -57.82 M-1.3 1.83 C7.77 -3.82, 29.34 -22.84, 55.45 -32.61 C81.56 -42.38, 138.87 -52.37, 155.34 -56.77" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(160.11881576843956 214.89905611180131) rotate(0 77.02220231874844 -27.995987935350627)"><path d="M130.05 -40.64 C138.16 -44.92, 147.27 -53.91, 155.34 -56.77 M130.05 -40.64 C136.99 -44.84, 143.17 -49.35, 155.34 -56.77" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(160.11881576843956 214.89905611180131) rotate(0 77.02220231874844 -27.995987935350627)"><path d="M125.6 -60.67 C135.41 -57.78, 146.1 -59.6, 155.34 -56.77 M125.6 -60.67 C133.68 -59.74, 141.01 -59.1, 155.34 -56.77" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-Vx9rt5D3Q9rYhKxmffdow"><rect x="0" y="0" fill="#fff" width="414.11881576843956" height="372.20064252799807"/><rect x="187.86156537308602" y="171.5" fill="#000" width="54.53333282470703" height="20" opacity="1"/></mask><g transform="translate(187.86156537308614 171.5) rotate(0 49.27945271410192 15.403068176450688)"><text x="27.266666412353516" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">resolve</text></g><g mask="url(#mask-ctUEOEm9Hj43unAbQevEL)" stroke-linecap="round"><g transform="translate(158.11881576843956 216.88413960680361) rotate(0 74.84159647175576 29.145436807999545)"><path d="M0.85 1.1 C10.59 5.63, 33.45 18.25, 58.29 27.52 C83.12 36.78, 134.69 51.79, 149.85 56.69 M-0.16 0.64 C9.98 5.25, 35.76 18.98, 60.63 28.48 C85.49 37.98, 134.26 52.76, 149 57.65" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(158.11881576843956 216.88413960680361) rotate(0 74.84159647175576 29.145436807999545)"><path d="M119.02 58.86 C129.51 57.11, 134.5 57.7, 149 57.65 M119.02 58.86 C125.58 59.2, 133.57 58.67, 149 57.65" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(158.11881576843956 216.88413960680361) rotate(0 74.84159647175576 29.145436807999545)"><path d="M125.26 39.31 C134 43.25, 137.19 49.48, 149 57.65 M125.26 39.31 C130.17 44.4, 136.64 48.64, 149 57.65" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-ctUEOEm9Hj43unAbQevEL"><rect x="0" y="0" fill="#fff" width="408.11881576843956" height="374.2435268722626"/><rect x="193.7688153869699" y="235.5" fill="#000" width="46.70000076293945" height="20" opacity="1"/></mask><g transform="translate(193.7688153869699 235.5) rotate(0 39.19159685322549 10.52957641480316)"><text x="23.350000381469727" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">reject</text></g><g stroke-linecap="round" transform="translate(315.11881576843956 123.5) rotate(0 82 34)"><path d="M17 0 C49.9 -0.14, 80.6 -1.64, 147 0 M17 0 C53.67 -1.34, 90.84 -0.33, 147 0 M147 0 C158.81 1.82, 162.9 6.01, 164 17 M147 0 C160.29 -1.41, 162.78 6.37, 164 17 M164 17 C165.45 27.16, 164.87 34.85, 164 51 M164 17 C164.82 28, 164.87 36.96, 164 51 M164 51 C165.52 60.81, 157.81 67.63, 147 68 M164 51 C163.02 64.21, 156.71 67.32, 147 68 M147 68 C118.18 68.45, 89.05 65.36, 17 68 M147 68 C104.3 69.64, 59.73 68.65, 17 68 M17 68 C5.19 69.92, 0.12 60.94, 0 51 M17 68 C4.06 65.76, -1.85 61.47, 0 51 M0 51 C-1.69 37.15, -1.31 23.78, 0 17 M0 51 C0.01 38.54, -0.98 28, 0 17 M0 17 C-0.7 4.76, 6.09 1.37, 17 0 M0 17 C1.78 5.87, 6.65 -0.93, 17 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(343.068816531379 147.5) rotate(0 54.04999923706055 10)"><text x="54.04999923706055" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">.then(onFulfill)</text></g><g stroke-linecap="round" transform="translate(309.11881576843956 236.5) rotate(0 88 43.5)"><path d="M21.75 0 C52.01 0.06, 85.42 1.93, 154.25 0 M21.75 0 C50.37 -0.29, 79.51 0.34, 154.25 0 M154.25 0 C167.56 -1.09, 176.5 6.4, 176 21.75 M154.25 0 C168.61 2.24, 176.33 5.63, 176 21.75 M176 21.75 C174.72 31.2, 174.16 39.67, 176 65.25 M176 21.75 C175.07 31.82, 176.53 40.3, 176 65.25 M176 65.25 C176.69 78.76, 170.15 85.47, 154.25 87 M176 65.25 C177.6 78.43, 168.33 88.44, 154.25 87 M154.25 87 C108.86 84.09, 68.63 84.35, 21.75 87 M154.25 87 C116.39 85.74, 76.64 86.26, 21.75 87 M21.75 87 C5.63 85.67, -0.54 80.31, 0 65.25 M21.75 87 C7.41 87.96, 0.42 78.63, 0 65.25 M0 65.25 C-1.88 48.86, -1.61 31.98, 0 21.75 M0 65.25 C0.4 55.61, -0.26 47.39, 0 21.75 M0 21.75 C-1.41 6.33, 6.07 -0.85, 21.75 0 M0 21.75 C0.89 8.44, 5.85 -0.74, 21.75 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(320.2521470672677 250) rotate(0 76.86666870117188 30)"><text x="76.86666870117188" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">.then(...,onRejection)</text><text x="76.86666870117188" y="20" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge"/><text x="76.86666870117188" y="40" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">.catch(onRejection)</text></g><g stroke-linecap="round"><g transform="translate(10.11881576843956 52.5) rotate(0 387.01857706177424 2.464118137342865)"><path d="M-0.12 -0.16 C128.89 0.7, 645.12 4.21, 774.16 5.08 M0.11 0.11 C129.1 0.93, 645.13 4.07, 774.11 4.85" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask/><g transform="translate(27.11881576843956 10) rotate(0 48.016666412353516 17.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="28px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Pending</text></g><g transform="translate(337.11881576843956 11.5) rotate(0 51.29999923706055 17.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="28px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Settled</text></g><g mask="url(#mask-Kedv754zntr6KoJc4_adV)" stroke-linecap="round"><g transform="translate(485.11881576843956 156.5) rotate(0 69.0067109951508 21.72678026133292)"><path d="M0.06 -0.28 C10.87 1.87, 42.01 5.38, 64.94 12.64 C87.87 19.9, 125.4 38.15, 137.63 43.28 M-0.39 0.44 C10.36 2.65, 41.46 5.75, 64.6 12.97 C87.73 20.18, 126.08 38.78, 138.4 43.73" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(485.11881576843956 156.5) rotate(0 69.0067109951508 21.72678026133292)"><path d="M108.47 41.8 C117.52 42.29, 126.96 42.34, 138.4 43.73 M108.47 41.8 C117.43 42.46, 127.1 43.03, 138.4 43.73" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(485.11881576843956 156.5) rotate(0 69.0067109951508 21.72678026133292)"><path d="M116.72 23.01 C123.09 29.52, 129.89 35.58, 138.4 43.73 M116.72 23.01 C123.2 29.46, 130.33 35.81, 138.4 43.73" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-Kedv754zntr6KoJc4_adV"><rect x="0" y="0" fill="#fff" width="723.1188157684398" height="299.7518812895303"/><rect x="526.0688165313791" y="159.5" fill="#000" width="48.099998474121094" height="20" opacity="1"/></mask><g transform="translate(526.0688165313791 159.5) rotate(0 28.056710232211344 18.72678026133292)"><text x="24.049999237060547" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">return</text></g><g mask="url(#mask-3B6oDABX7KIOi1yC2dwyv)" stroke-linecap="round"><g transform="translate(486.11881576843956 282.0326902465166) rotate(0 68.4630162274791 -39.16269454306658)"><path d="M0.51 0.54 C11.75 -4.65, 44.46 -17.2, 67.31 -30.11 C90.17 -43.03, 125.93 -68.81, 137.61 -76.93 M-0.69 -0.23 C10.96 -4.74, 46.78 -15.78, 69.67 -28.88 C92.55 -41.99, 125.37 -70.62, 136.65 -78.86" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(486.11881576843956 282.0326902465166) rotate(0 68.4630162274791 -39.16269454306658)"><path d="M120.9 -53.33 C126.13 -61.92, 128.82 -68.8, 136.65 -78.86 M120.9 -53.33 C127.73 -63.16, 132.57 -71.48, 136.65 -78.86" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(486.11881576843956 282.0326902465166) rotate(0 68.4630162274791 -39.16269454306658)"><path d="M108.17 -69.42 C117.22 -73.42, 123.58 -75.64, 136.65 -78.86 M108.17 -69.42 C119.56 -73.26, 129.05 -75.7, 136.65 -78.86" stroke="#1e1e1e" stroke-width="2" fill="none"/></g></g><mask id="mask-3B6oDABX7KIOi1yC2dwyv"><rect x="0" y="0" fill="#fff" width="724.1188157684396" height="459.7413417385016"/><rect x="530.0688165313791" y="242.5" fill="#000" width="48.099998474121094" height="20" opacity="1"/></mask><g transform="translate(530.0688165313791 242.5) rotate(0 24.51301546453965 0.3699957034500301)"><text x="24.049999237060547" y="0" font-family="Virgil, Segoe UI Emoji" font-size="16px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">return</text></g><g stroke-linecap="round" transform="translate(626.6188157684396 168.5) rotate(0 73.5 36)"><path d="M18 0 C47.85 -2.64, 80.12 -2.58, 129 0 M18 0 C40.05 -0.99, 63.17 -0.53, 129 0 M129 0 C140.39 1.75, 148.09 5.66, 147 18 M129 0 C141.07 -0.51, 147.54 6.34, 147 18 M147 18 C147.71 30.79, 146.62 39.14, 147 54 M147 18 C146.44 26.02, 147.31 36.03, 147 54 M147 54 C145.92 67.44, 142.95 72.48, 129 72 M147 54 C147.86 64.02, 142.06 71.79, 129 72 M129 72 C96.06 71.3, 66.17 71.65, 18 72 M129 72 C88.89 72.92, 47.72 71.57, 18 72 M18 72 C4.88 71.19, -0.67 67.12, 0 54 M18 72 C7.18 72.91, -0.42 64.46, 0 54 M0 54 C0.81 41.73, 1.16 29.23, 0 18 M0 54 C0.15 41.37, -1.11 31.26, 0 18 M0 18 C-0.72 4.54, 6.4 1.02, 18 0 M0 18 C-2.2 7.77, 4.53 1.66, 18 0" stroke="#1e1e1e" stroke-width="2" fill="none"/></g><g transform="translate(649.9188150055002 187) rotate(0 50.20000076293945 17.5)"><text x="50.20000076293945" y="0" font-family="Virgil, Segoe UI Emoji" font-size="28px" fill="#1e1e1e" text-anchor="middle" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Promise</text></g><g transform="translate(643.1021493560861 11) rotate(0 48.016666412353516 17.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="28px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">Pending</text></g><g transform="translate(330.11881576843956 88.5) rotate(0 69.05000305175781 13.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="21.600000000000005px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">handle result</text></g><g transform="translate(334.11881576843956 340.5) rotate(0 58.95000076293945 12.5)"><text x="0" y="0" font-family="Virgil, Segoe UI Emoji" font-size="20px" fill="#1e1e1e" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="text-before-edge">handle error</text></g></svg>
  </div>

---
transition: fade
---

# Chaining Promises

```ts {all|1-6|8-13}
new Promise(resolve => resolve(5))
  .then(a => a + 5)
  .then(a => a + 6)
  .then(console.log)

// Logs 16

new Promise((_, reject) => reject(5))
  .then(a => a + 5)
  .then(a => a + 6)
  .catch(console.log)

// Logs 5
```

[`then(onFulfill, onRejection)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 

returns a new Promise that contains the output of computation of either `onFulfill` or `onRejection`.

---
transition: fade
---


# .then() method implementation / 1
 
```ts {all|1|2|14-22|4-12}
export class PinkyPromise<T> implements PromiseLike<T> {
  #notify() { /* TO DO add implemetation */ }

  static #transition(
    pinkyPromise: PinkyPromise<any>,
    state: PromiseState<any>,
  ) {
    if (pinkyPromise.#state.status === "pending") {
      pinkyPromise.#state = state;
      pinkyPromise.#notify();
    }
  }

  then<TResult1 = T, TResult2 = never>(
    onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | null | undefined,
    onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | null | undefined,
  ): PinkyPromise<TResult1 | TResult2> {
    return  new PinkyPromise<TResult1 | TResult2>((resolve, reject) => {
      // do something then resolve
      this.#notify();
    });
  }
}
```

---
transition: fade
---

# .then() method implementation / 2

```ts {all|7|10-17|19-23}
export class PinkyPromise<T> implements PromiseLike<T> {
  #state: PromiseState<T> = { status: "pending" };
  #fulfilledListeners: ((value: unknown) => unknown)[] = [];
  #rejectedListeners: ((reason: unknown) => unknown)[] = [];
  // Notify listeners if the Promise is settled
  #notify() {
    switch (this.#state.status) {
      case "pending":
        break;
      case "fulfilled": {
        this.#fulfilledListeners.forEach((cb) => queueMicrotask(() => cb(this.#state.value)));
        break;
      }
      case "rejected": {
        this.#rejectedListeners.forEach((cb) => queueMicrotask(() => cb(this.#state.reason)));
        break;
      }
    }
    // remove listeners at the end if the Promise is not settled
    if (this.#state.status !== "pending") {
      this.#fulfilledListeners.splice(0, this.#fulfilledListeners.length);
      this.#rejectedListeners.splice(0, this.#rejectedListeners.length);
    }
  }
}
```
