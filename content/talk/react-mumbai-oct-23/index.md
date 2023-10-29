---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "React Mumbai"
subtitle: My experience speaking at an external community conference
summary: My experience speaking at an external community conference
authors: []
tags: ['frontend', 'react', 'conferences', 'tech talks']
categories: ['Talks']
date: 2023-10-28T10:00:00+05:30
lastmod: 2023-10-28T10:00:00+05:30
featured: false
draft: false
image:
  filename: featured.jpg
  focal_point: SMART
  preview_only: false
---

React Mumbai was a fun and happening event.
The fact that it was organized by a bunch of young and fresh devs truly surprised me, because I was amazed by how well it was organised! It felt like the work of professional conference organizers. Shout out to this energetic bunch
- [Ehtesham Siddiqui](https://x.com/ehteshamdev)
- [Amtulla Baranwala](https://twitter.com/amtulladev)
- [Imran Sayed](https://x.com/sayedimrandev)

<br>

# Talks

## Micro interactions in React native
By [_Abhishek Joshi_](https://www.linkedin.com/in/abhishek95joshi/)

Abhishek showed some fantastic animations and how they can be done by writing very little code with React native reanimated library. I always imagined coding choreographed animations would be very time consuming, but this talk proved how easy it is in reality. Later I connected with him discussing some of the interactivity problems I was facing in my current project and he was very generous to share tips to solve them.

<br>

## INP: Make your webpage more interactive
By [_Divyansh Gupta_](https://twitter.com/iamdivyansh)

Divyansh explained the core web vitals and how Google uses it to score your website for search rankings. Tradiotionally First Input Delay (FID) was used as a metric, but it proved to be insufficient because a delay in later interactions for the user were not accounted for. Hence Google came up with Interaction to Next Paint (INP), which accounts for every interaction and considers the full time it takes from user interaction to browser painting it on screen. His deck was loaded with performance improvement tips.

<br>

## Frontend performance optimization using code splitting
By [_Yuvraj Pandey_](https://www.linkedin.com/in/yuvrajpy/) and [_Bhargav Shah_](https://www.linkedin.com/in/overcompiled/) pairing

Yuvraj and I explained the history of modules in JavaScript and how bundling came into being. We explained the concepts of tree shakable 3rd party libraries. We went on to explain the dynamic imports and how they can be done declaratively using React.lazy and Suspense. Finally we touched upon the browser resource hints *preload* and *prefetch* and how to decide which one to use when.

<br>

## Introducing Bun: A game changer in JavaScript
By [_Sarthak Upadhyay_](https://www.linkedin.com/in/sarthakupadhyay/)

Sarthak had a witty approach to explain the JS landscape and how there were so many tools to solve each one of those problems. Bun was introduced as a drop in replacement of Node and he shared his experiences using it. Final verdict was that it is pretty good and works in most cases but it is not production battle tested yet. If your project depends on any Node APIs that Bun doesn't support, you can't _drop in_ Bun as it claims.