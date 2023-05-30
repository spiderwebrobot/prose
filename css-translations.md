# CSS Translations

The [translate()](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translate) CSS function repositions an element in the horizontal and/or vertical directions. It can be used to pan through an oversized-element inside of a smaller parent-element. The technique requires 3 HTML elements:

1. A view-element
2. A translate-element
3. A scene-element

Where the view-element is the root-element, e.g.

```html
<div class="view">
  <div class="translate">
    <div class="scene"></div>
  </div>
</div>
```

CSS variables

```css
:root {
  --animation-direction: normal;
  --animation-duration: 5s;
  --animation-timing-function: linear;
  --scene-height: 360px;
  --scene-width: 900px;
  --view-height: auto;
  --view-max-width: none;
}
```

View styles

```css
.view {
  max-width: var(--view-max-width);
  height: var(--view-height);
  overflow: hidden;
}
```

Translate styles

```css
.translate {
  animation-direction: var(--animation-direction);
  animation-duration: var(--animation-duration);
  animation-fill-mode: forwards;
  animation-iteration-count: 1;
  animation-timing-function: var(--animation-timing-function);
}

.translate.is-active.mod-x {
  animation-name: slide-x;
}

.translate.is-paused {
  animation-play-state: paused;
}
```

Scene styles

```css
.scene {
  width: var(--scene-width);
  height: var(--scene-height);
}

.scene.mod-x {
  background-repeat: no-repeat;
  background-image: url(https://dw9to29mmj727.cloudfront.net/promo/2016/5583-Tier07_Headers_BookHeroes_2000x800.jpg);
  background-position: center;
  background-size: cover;
}
```

CSS animation

The [transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) CSS property lets you rotate, scale, skew, or translate an element. The formula used `calc(100% - 900px)` or `translateX(100%) translateX(-900px)`

```css
@keyframes slide-x {
  to {
    transform: translateX(100%) translateX(calc(var(--scene-width) * -1));
  }
}
```
