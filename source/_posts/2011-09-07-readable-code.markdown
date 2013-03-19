---
layout: post
title: "Readable Code"
date: 2011-09-07 22:29
comments: true
categories: [coding] 
---

# Clean Code

I have graduated from college this June. The culture in my college is you do all group projects by youself. So collaboration is some what new to me.

I started working in a web company after my graduation. After two months of non-stop coding day and night, I learnt a few things about coding.

This time I will talk about coding practice.

## Background

The project I invovled was a CMS-like Web Site Builder. The whole team had 4 coders. We used SVN as a version control tools for collaboration. A PHP MVC framework is used in developing the project(God bless :) ).

## Coding Practice

Assignment or functions call can tightly packed:

    a = 1;
    foo();
    bar();

Loop should wrap with newline

    foo();

    if(bar){
      a = 1;
    }

    foo();

    for($i = 0; $i < 10; $i++){

      if(bar){
        exit();
      }

    }

    exit();

## Conclusion

I always think coding practice is not a very important thing in development. But after I worked with a project that contains 30K LOC, I understand this kind of agreement can help with code maintenance and easier for debugging.

