# CSS is Hard

The introduction to CSS focuses on the box model. We learned about sizing elements and the difference between block-level and inline elements. It will be a while before it will feel natural to write styles for your application.

The concepts we want to learn today are:

- Position
- Selectors/Specificity
- Layouts

Topics that aren't covered on day one:

- [Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)
- [Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)
- [CSS Animation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations)
- [CSS Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transforms)
- [Media Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries)
- [Common Use Cases](https://developer.mozilla.org/en-US/docs/Learn/CSS/Howto)

## Position

The default behaviour is to have elements positioned automatically for us based on the flow of the document. There are ways to change the position of an element directly. Elements have a `position` property that can be set to `static`, `relative`, `absolute` or `fixed`.

HTML is by default responsive. It is the addition of styles that can remove the responsiveness of a document. There are definitely good use cases for each of the position values so it's important understand how the behaviour is changed.

### static

This is the default behaviour for all HTML elements.

### relative

When setting an element to relative, the behaviour will seem to be the same as static. There are two hidden differences.

1. You can now use top, left, right and bottom properties.
2. Your element is now considered to be 'positioned', this becomes important when also using 'absolute'.

### absolute

> I need to put a disclaimer here. Absolute position seems to be the easiest way to make CSS do what you want. The truth is that in order to make absolute positiong work, you need to be really good at CSS. It is very easy to get stuck in a hole by making things absolute.

If you set an element to have it's position be absolute, it will position the element relative to the window.

```css
div {
  position: absolute;
  left: 0;
  top: 0;
}
```

In this example we set the location absolutely to 0,0 of the window. We can also position absolute elements relative to their container. We just need to ensure the container is a 'positioned' element. That is what `relative` is for. This is not intuitive.

### fixed

This is very similar to absolute. It is most common used for headers or footers that need to stay 'affixed' to the top or bottom of the viewport. I would use it sparingly.

## Selectors

We use a `selector` to define which element to apply rules to. [Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) are patterns which can match elements based on their type, class or id.

```css
div p, #id:first-line {
  /* a series of declarations */
}
```

This is a [group selector](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Combinators_and_multiple_selectors). Two selectors separated by a comma. In this example any `paragraph` that has a `div` as an ancestor and the first line of element with the id `#id` would match the pattern.

We can be quite specific about which element we are trying to target. We can combine a tag and a class like `div.primary`. We can also use combinators and extend our ability to target elements more specifically. If we want to target a paragraph that is a decendant of a div we could use `div p`.

With so many options it is easy to get confused. There is a difference between `div.primary` and `div .primary`. A simple space character turns it into a combinator.

## Specificity

When the browser applies styles properties to an element it uses the [specificity](http://cssspecificity.com/) to resolve multiple matches for the same property.

```css
div p {
  color: cyan;
}

p {
  color: magenta;
}
```

```html
<div>
  <p>Content</p>
</div>
```

Apply the specificity rules to the example CSS.  We can determine that the `div p` has a specificity of 2, while the `div` has a specificity of 1. The way that it is determined is by identifying the number of ID selectors, class selectors and type selectors in the overall selector. There are other selectors used, but for the purposes of learning how to determine the specificity these three are enough.

ID selectors > class selectors > type selectors.

Think of a 3 digit number:

1. The number of type selectors is used as the 'ones' digit.
2. The number of class selectors is used as the 'tens' digit.
3. Finally the number of ID selectors (one I hope) is used as the 'hundreds' digit.

With the example of `div p` you would have a specificity of 002. If you added a class `div.warning p` then the specificity is 012.

```
p                       001
article p               002
img.bio.rounded         021
#container              100
div#container.centered  111
```

It is possible to have more than 10 tags as a selector. With 10 tags and a 10 classes you would get something like `0, 10, 10`. It would be more specific than 11 tags and 9 classes `0, 9, 11`.

If any of these examples are not clear then read more about the rules on [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) or [css-tricks](https://css-tricks.com/specifics-on-css-specificity/).

### style attributes

The most specific way to apply a style is by using the `style` attribute. I can give an HTML element styles directly in the markup language.

```html
<div style="color: yellow">Content</div>
```

Placing your styles inline is an anti-pattern. It doesn't allow for composibilty and re-usability of styles. Reserve this for edge cases.

### !important

> Disclaimer: The use of !important should be reserved for the most extreme cases. If you use !important too much then how do we know what is important anymore?

If you tag a property with the `!important` suffix it will increase it's importance. More specific selectors will not be allowed to assign a value to the property, unless they too are marked as !important.

```css
p {
  color: yellow !important;
}

div p {
  color: magenta;
}
```

Even though `div p` is more specific than `p`, the color yellow will be applied. Good luck.

## Layouts

Early web technologies were designed to share documents. Now we build elaborate web applications. Web applications have different layout requirements than documents. Over time CSS evolved to allow for more flexibility with regard to layouts.

The primary concept used to design complex layouts is the 'Grid'. It allows designers to keep the content aligned and ordered.

### Grid

The earliest grid implementations relied on the `float` property for block-level elements. The most common grid impelementations were 12 columns. If we take 100% and divide it by 12 we get 8.333333333333333%, the width of each column.

```html
<div class="row">
  <div class="col-12">Top Navigation</div>
</div>
<div class="row">
  <div class="col-4">Sidebar></div>
  <div class="col-8">Main Content</div>
</div>
<div class="row">
  <div class="col-4">First Column></div>
  <div class="col-4">Second Column</div>
  <div class="col-4">Third Column</div>
</div>
```

The 12 column grid became popular because of the number of configuration options it provides. In the above example we start with a full width navigation bar. We split our main window into 2 columns. The sidebar and the main content. Finally we have a footer that provides 3 equally sized columns of content.

### Learn Flexbox

CSS is not easy. It's also improving at a pretty steady rate. We are limited by what browsers will implement. It is important to get in the habit of consistetly reviewing the new features of CSS and spending the time to learn them.

[Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout) is well supported and it solves a problem that developers have dealt with for many years. It would be irresponsible to try and teach Flexbox before going through the foundation of CSS.

There is also an official grid implementation that does not have the same level of browser support as Flexbox. I don't think the question is 'Should I learn flexbox or should I learn grid?'. Learn both and use the one that makes sense for your layout.

Once you have read some of the documentation for flexbox, you can get some practice with a game like [Flexbox Froggy](https://flexboxfroggy.com/).

## Bonus

CSS Frameworks. They provide a foundation to build upon. The default styles contained in a framework are typically well designed. [Bootstrap](https://getbootstrap.com/), [Foundation](https://foundation.zurb.com/), [Bulma](https://bulma.io/) and [Skeleton](http://getskeleton.com/) are all frameworks. If you take a look they all tend to provide styles for the most common types of elements. They even compose elements together to make components.

When using a framework, pay attention to how they name their classes. How they organize their files. I get better at CSS by observing how others use it. I follow the lead of people who know the language well.

Use a framework to get something that looks good working quickly. Don't use a framework to avoid learning CSS.

Once you have made the decision to learn CSS, take a look at a methodology like [BEM](https://css-tricks.com/bem-101/). Scaling styles to larger applications will require a system in place to manage complexity.
