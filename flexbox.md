## Video 6: Flexbox Aignment - Centering with `justify-content`

### `justify-content`: how content is aligned along the main axis

```CSS
.container {
  display: flex;
  justify-content: flex-start | flex-end | center | space-between | space-around
}
```
  * `flex-start`: default, start of flex container
  * `flex-end`
  * `center`: no margin tricks
  * `space-between`: divvy up the space between elements
  * `space-around`: takes into account space on left and right

vertical center in flexbox

```CSS
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
```

## Video 7: Flexbox Alignment - Centering with `align-items`

### `align-items`: align content along cross axis

```CSS
.container {
  display: flex;
  align-items: stretch | center | flex-start | flex-end | baseline;
  height: 100vh;
}
```

  * `baseline`: along the bottom of the text
  * `stretch`: default, all of the containing element
  * `center`: vertical center
  * `flex-start`: at the top
  * `flex-end`: at the bottom

## Video 8: Flexbox Alignment - Centering with `align-content`

### `align-content`: similar to `justify-content`, but along takes care of the extra space along the cross axis

```CSS
.container {
  display: flex;
  flex-wrap: wrap;
}

.box {
  width: 33%;
}
