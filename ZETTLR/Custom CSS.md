### Custom CSS
---
----


```css
/* Horizontal rule style */
body .main-editor-wrapper .cm-hr {
    display: inline-block;
    width: 100%;
    line-height: 0.25;
    color: transparent;
    background: linear-gradient(90deg, var(--c-primary),  #7beac3, transparent);
}

/* Full-width, thicker and grey horizontal lines
    Two "---" can also be concatenated  (with line return in-between
    for even thicker lines.*/
body .main-editor-wrapper .cm-hr {
  display: block;
  width: 100%;
  border: none;
  border-top: 1px solid #a1d1a1;
  height: 0;
  background: none;
  box-sizing: border-box;
}
```
