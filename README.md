# Oscillation

```bash
npm install oscillation
```

Oscillation is a physics-based animation library for creating smooth, natural-looking animations.

```js
import { motion, spring } from "oscillation";

// Animate a single value
motion(spring(0, 100), (value) => {
  console.log(value);
});

// Animate multiple properties
motion({ x: spring(0, 100), y: spring(0, 200) }, (state) => {
  console.log(state.x, state.y);
});

// Animate an array of values
motion([spring([0, 0, 0], [255, 128, 0]), spring(0, 360)], ([color, rotation]) => {
  console.log(color, rotation);
});
```

## API

### `motion(defs, callback, options?)`

Starts an animation and calls the callback with updated values on each frame. The `motion` function
is overloaded to support different input types:

1. Single motion object:

```js
motion(spring(0, 100), (value) => {
  // value gets animated from 0 to 100
});
```

2. Object with motion properties:

```js
motion({ x: spring(0, 100), y: spring(-50, 50) }, ({ x, y }) => {
  // x and y animated simultaneously
});
```

3. Array of motion objects:

```js
motion([spring([0, 0, 0], [255, 128, 0]), spring(0, 360)], ([color, rotation]) => {
  // color and rotation animated simultaneously
});
```

Parameters:

- `defs`: A single `Motion` object, an object with `Motion` values, or an array of `Motion` objects.
- `callback`: A function called on each frame with the current state.
- `options`: Optional object with an `AbortSignal` to stop the animation.

### `spring(source, destination, config?)`

Creates a spring-based motion.

Parameters:

- `source`: Initial value(s).
- `destination`: Target value(s).
- `config`: Optional spring configuration (`damping`, `frequency`, `precision`).

Supports various numeric types: `number`, `Float64Array`, `Float32Array`, `Uint32Array`,
`Int32Array`, `Uint16Array`, `Int16Array`, `Uint8Array`, `Int8Array`, `Array<number>`.

## Cancelling Animations

You can cancel ongoing animations using an `AbortSignal`:

```js
let ctl = new AbortController();

motion(
  spring(0, 100),
  (value) => {
    console.log(value);
  },
  { signal: ctl.signal },
);

// To cancel the animation:
ctl.abort();
```

## Support `prefers-reduced-motion`

If the user's system has
[reduced motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion)
setting enabled, the animation path will be skipped and destination state going to be the only state
rendered. To opt-out of this behavior and always animate the whole path, use `ignoreReducedMotion`
flag:

```js
motion(
  spring(0, 100),
  (value) => {
    console.log(value);
  },
  { ignoreReducedMotion: true },
);
```

## Examples

### Animating UI Elements

```js
import { motion, spring } from "oscillation";

let button = document.querySelector("#myButton");

motion({ x: spring(0, 100), scale: spring(1, 1.2) }, (state) => {
  button.style.transform = `translateX(${state.x}px) scale(${state.scale})`;
});
```

### Complex Color and Rotation Animation

```js
import { motion, spring } from "oscillation";

let element = document.querySelector("#animatedElement");

motion([spring([0, 0, 0], [255, 128, 0]), spring(0, 360)], ([color, rotation]) => {
  let [r, g, b] = color;
  element.style.backgroundColor = `rgb(${r}, ${g}, ${b})`;
  element.style.transform = `rotate(${rotation}deg)`;
});
```

## Prior Art

Oscillation project brings nothing significantly new to the scene and is based on the work of many
engineers and researchers.

- [React Motion](https://github.com/chenglou/react-motion) by
  [@chenglou](https://github.com/chenglou)
- [Mobile First Animation](https://github.com/aholachek/mobile-first-animation) talk by
  [@aholachek](https://github.com/aholachek)
- [Designing Fluid Interfaces](https://developer.apple.com/videos/play/wwdc2018/803/) talk from WWDC
  2018

## License

[ISC License](./LICENSE) &copy; Oleksii Raspopov
