# Panorama

> An unbroken view of the whole region surrounding an observer

## Ask

Use the CSS [translate()](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translate) function to pan through an image (or a series of images).

## Requirements

Requires 3 [HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started) elements:

```html
<div class="view">
  <div class="translate">
    <div class="scene">...</div>
  </div>
</div>
```

- `<div class="scene">` contains the moving parts
- `<div class="translate">` moves the *scene-element*
- `<div class="view">` is the *scene-element*’s viewport

## Considerations

- The *scene-element* should be larger than the *view-element* so that the *scene-element* can be partially hidden inside of the *view-element*.
- The *translate-element* should play the *scene-element* for 5 seconds or less. Otherwise the translation will require a [pause, stop or hide](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html) control for accessibility ([a11y](https://www.a11yproject.com/)).
- Speaking of accessibility, consider building an alternative scene for folks that [prefer reduced motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion).

## Solutions

The CSS rules for panning through a scene depend on the direction of the translation.

### [Translate X](https://cdpn.io/pen/debug/YzRGXWX)

Panning from the **left** side of a scene to the **right** side of a scene requires a fixed *scene-width*, e.g. `1125px`. The *scene-width* is subtracted from `100%` to calculate the end of the scene, e.g. `calc(100% - 1125px)`.

```css
.scene {
  width: 1125px;
  height: 450px;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url(https://dw9to29mmj727.cloudfront.net/promo/2016/5583-Tier07_Headers_BookHeroes_2000x800.jpg);
  background-position: center;
}

.translate {
  transition-timing-function: ease-in-out;
  transition-property: transform;
  transition-duration: 5s;
}

.translate.is-active {
  transform: translateX(100%) translateX(-1125px);
}

.view {
  overflow: hidden;
}
```

### [Translate Y](https://cdpn.io/pen/debug/ExONOEj)

Panning from the **top** of a scene to **bottom** of a scene requires a fixed *view-height*, e.g. `450px`. The *view-height* is added to `-100%` to calculate the end of the scene, e.g. `calc(-100% + 450px)`.

```css
.scene {
  width: 2000px;
  height: 800px;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url(https://dw9to29mmj727.cloudfront.net/promo/2016/5579-Tier07_Headers_GoGoMonster_2000x800.jpg);
  background-position: center;
}

.translate {
  transition-timing-function: ease-in-out;
  transition-property: transform;
  transition-duration: 5s;
}

.translate.is-active {
  transform: translateY(-100%) translateY(450px);
}

.view {
  height: 450px;
  overflow: hidden;
}
```

### [Translate XY](https://cdpn.io/pen/debug/RwqoLPX)

Panning from the **top-right** corner of a scene to the **bottom-left** corner of a scene requires a fixed *scene-width*, e.g. `2000px`, and a fixed *view-height*, e.g. `450px`.

```css
.scene {
  width: 2000px;
  height: 800px;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url(https://dw9to29mmj727.cloudfront.net/promo/2016/5945-SeriesHeaders_DemonSlayer_2000x800_wm.jpg);
  background-position: center;
}

.translate {
  transition-timing-function: ease-in-out;
  transition-property: transform;
  transition-duration: 5s;
  transform: translateX(100%) translateX(-2000px);
}

.translate.is-active {
  transform: translateY(-100%) translateY(450px);
}

.view {
  height: 450px;
  overflow: hidden;
}
```

### [Translate YX](https://cdpn.io/pen/debug/mdQOQMw)

Panning from the **top-left** corner of a scene to the **bottom-right** corner of a scene requires a fixed *scene-width*, e.g. `2000px`, and a fixed *view-height*, e.g. `450px`.

```css
.scene {
  width: 2000px;
  height: 800px;
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url(https://dw9to29mmj727.cloudfront.net/promo/2016/5541-Ghibli_SeriesHeaders_Kiki_2000x800.jpg);
  background-position: center;
}

.translate {
  transition-timing-function: ease-in-out;
  transition-property: transform;
  transition-duration: 5s;
}

.translate.is-active {
  transform: translateX(100%) translateX(-2000px) translateY(-100%) translateY(450px);
}

.view {
  height: 450px;
  overflow: hidden;
}
```

## Help from my friends

Thanks to [Zahra Ilyas](https://www.linkedin.com/in/zahra-ilyas-39847113b/) and [Joe Banks](https://www.linkedin.com/in/joebanks10/) for their timely feedback. Zahra walked me through the expected behavior, while Joe suggested I use the CSS [calc](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) function to calculate any variable widths and heights. I used their feedback to tinker around with possible solutions.

## Digging a little deeper

I am still not exactly sure how these techniques work, but here is how I think they work:

- The Translate X technique seems to work by pushing the scene rightwards completely offscreen (`100%`). Once the scene is offscreen, subtracting the *scene-width* pulls the scene leftwards. So if the *scene-width* is 900 pixels, then the scene will be pulled leftwards 900 pixels.

- The Translate Y technique seems to work by pushing the scene upwards completely offscreen (`-100%`). Once the scene is offscreen, adding the *view-height* pulls the scene downwards. So if the *view-height* is 360 pixels, then the scene will be pulled downwards 360 pixels.

I also tried optimizing the techniques by using just 2 HTML elements instead of 3:

```html
<div class="view">
  <div class="scene">...</div>
</div>
```

The good news, the Translate Y technique seems to work as expected. The bad news, the Translate X technique doesn’t move the scene at all ([🤔](https://cdpn.io/pen/debug/GRwNzNR)). So if you only need to move a scene up and down, then by all means optimize! Otherwise, using 3 HTML elements will give you the flexibilty of moving up, down, and side to side.

## Improvements

I had fun working on this project. I don’t know how useful these techniques are, but they can definitely be improved. Adding some curves to the linear movements would be a good start. This would allow for more visual coverage of a scene. That being said, you’d probably have to switch from [CSS transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_transitions/Using_CSS_transitions) to [CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations/Using_CSS_animations) to support changes in direction.

There are some advantages to using CSS animations, including the ability to **pause** and **reverse** movements. Here is an example of how the *translate-element* could be refactored:

```css
.translate {
  animation-direction: normal;
  animation-duration: 5s;
  animation-fill-mode: forwards;
  animation-iteration-count: 1;
  animation-timing-function: ease-in-out;
}

.translate.is-active {
  animation-name: pan;
}

.translate.is-paused {
  animation-play-state: paused;
}

.translate.mod-reverse {
  animation-direction: reverse;
}

@keyframes pan {
  to {
    transform: translateX(100%) translateX(-1125px);
  }
}
```

## Resources

- [Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)
- [Not possible to use CSS calc() with transform:translateX in IE](https://stackoverflow.com/questions/21469350/not-possible-to-use-css-calc-with-transformtranslatex-in-ie)
- [Making CSS Animations Feel More Natural](https://css-tricks.com/making-css-animations-feel-natural/)
