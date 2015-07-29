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
