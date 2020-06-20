---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Convert pixel values to rem, Sass style"
subtitle: ""
summary: ""
authors: ['Bhargav Shah']
tags: []
categories: []
date: 2020-06-20T18:00:25+05:30
lastmod: 2020-06-20T18:00:25+05:30
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
![img](https://cdn-images-1.medium.com/max/1600/1*vzA3LuouczBEUKy8YggS_A.png)

SCSS is a very widely used CSS pre-processer. Selector nesting, mixins, variables are some of the most used features of SCSS. One of the most unused and underrated SCSS feature is [functions](https://sass-lang.com/documentation/at-rules/function).

## Why use a function

Functions are very much like mixins, with the key difference that they just return a value and do not cause side effects. In other words, they cannot set any CSS properties. All they do is take a value, perform some computation and return the value. They are [pure](https://en.wikipedia.org/wiki/Pure_function).

They are a good candidate to convert pixel values to rem as many websites use `rem` not just to set font sizes, but other box properties too. Here, is a [good CSS Tricks article](https://css-tricks.com/theres-more-to-the-css-rem-unit-than-font-sizing/) about that. Having a handy function can make your stylesheets much more elegant, not having to visually process decimal values.

**Note***: For the rest of the article, all the snippets are in SCSS syntax, which is a brother of the SASS syntax. They are different syntaxes of the same tool.* [*More details here*](https://sass-lang.com/documentation/syntax)*.*

![img](https://cdn-images-1.medium.com/max/2400/1*zRdH93AghHhAVuiIaQYfCg.png)

Pretty neat huh? I think the plus side is that we think sizes in px, so it becomes easy to visualise the size of something when writing pixel values.

## The code

Define the function,

```
$html-font-size: 16px;
@function stripUnit($value) {
    @return $value / ($value * 0 + 1);
}
@function rem($pxValue) {
    @return #{stripUnit($pxValue) / stripUnit($html-font-size)}rem;
}
```

Then you can use it like this,

```
.component {
    font-size: rem(14px); // or rem(14)
}
```

This will set `font-size: 0.875rem` to the element which has `component` CSS class. Do note, the value you pass to the function can be **with or without a unit**.

![img](https://cdn-images-1.medium.com/max/1600/0*2KRjDTWWS9t6h8Fa)

Yeah, it’s that easy 💯

## How does it work, you ask?

The first function `stripUnit()` takes any value and strips the unit off. This is necessary if you want to perform any arithmetic operation on the value, which we do later in the `rem()` function. Notice, we add the `rem` unit to the result of `rem()`

## Further reading

If I couldn’t convince you to use functions 😭 and you still want to use mixins, this [CSS Tricks article](https://css-tricks.com/snippets/css/less-mixin-for-rem-font-sizing/) has a simple and an advanced implementation. Some of the function code in this article was inspired from there.
