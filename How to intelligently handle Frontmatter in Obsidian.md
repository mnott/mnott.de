---
title: # Sample Post
menu_order: 1
post_status: publish
post_excerpt: This shows how to adapt Obsidian to show, partly show or hide the frontmatter
taxonomy:
    category:
        - Computer
        - Obsidian
    post_tag:
        - Memex
        - Obsidian
comment_status: open
---

# How to intelligently handle Frontmatter in Obsidian

## Memex

If you have not yet checked out [Memex](https://memex.garden/), you definitely should. It is an amazing plugin that allows you to mark up on web pages, summarise them using AI, and even summarise the content of Youtube videos. What's more, you can have an Obsidian helper application, which will immediately drop notes that you make in your browser, into your Obsidian vault. This is a game changer for me, as I'm often seeing myself wanting to take a note about what I am reading, but then again am probably to lazy to make a new note in Obsidian, copy paste the URL, etc., just in case I might need it later.

With Memex, I can "clip to Obsidian" in a very elegant way, and not only the URLs, but all the notes I annotate directly on a web page. Yes, I know that [Readwise](https://readwise.io/) can do highlighting and will capture in a very similar way to Obsidian, but the AI feature in Memex makes it just so much better.

Full transparency: I'm not affiliated with Memex, but I am a lifetime supporter for Memex, and I have had many very productive discussions with Oli, Memex' CEO, who has been amazing at responding to requests and ideas, fixing bugs, hopping on the phone even - amazing.

## Obsidian

So why do I refer to Memex when I'm blogging about Obsidian Frontmatter. That's because since I started using Memex, one particular thing started to be even more annoying than it had been for me, before: Obsidian Properties. You see, with the recent update of Obsidian, what was before a `YAML` header in Obsidian now became a property that would actually get in my way when previewing notes:

![Annoying Frontmatter](00_annoying_fronmatter.png)

As you can see, my Home page in Obsidian is a dashboard, and the frontmatter now messes everything up. So what I've done next ist to create a little bit of CSS that I can add as a `cssclass` yaml header called `frontmatter-hidden`:

```css
.frontmatter-container {
	height: 0;
	visibility: hidden;
}

/* This sets the default for all notes to show the frontmatter */
.metadata-container {
    max-height: 1000px;
    opacity: 1;
}

/* This will hide the frontmatter */
.frontmatter-hidden .metadata-container {
    display: none !important;
}
```

With this, everything is good, and the frontmatter is hidden. But sometimes I want to sort of "optionally" view the frontmatter, i.e., have a kind of a button that will show it when I click on it. So by default it would show up collapsed:

```css
.frontmatter-collapsed .metadata-container {
    max-height: 2.7rem;
    opacity: 0.6;
    overflow: hidden;
    transition: max-height 250ms ease-in-out, opacity 250ms;
    margin-bottom: 0;
}

@media screen and (min-width: 1024px) {
    /* Hover and focus behavior for the collapsible frontmatter */
    .frontmatter-collapsed  .metadata-container:hover,
    .frontmatter-collapsed  .metadata-container:focus-within {
        max-height: 1000px;
        opacity: 1;
        transition: max-height 300ms ease-in-out, opacity 300ms;
        transition-delay: 0.5s;
    }
}
```



Here is the effect:

![](01_frontmatter_collapsed.png)

And if I click on `Properties`, it would show:

![](01_frontmatter_collapsed_uncollapsed.png)

So far I was using this as a default, so for all notes, except for when the frontmatter was actually hidden.


But now the problem with the arbitrary Memex note would be this:

![](02_memex_frontmatter_collapsed.png)

As you can see, what I'm missing there really is the URL that Memex puts into the frontmatter. So in other words, particularly for Memex notes, I really want to always see the frontmatter.

So the solution to that is of course to not do this all of the time, but to have the `frontmatter-collapsed` there optionally, and use it where needed, typically via a Templater plugin. For the rare cases where I don't want to see any frontmatter (Dashboards), I use the `frontmatter-hidden` class, and particularly for Memex, where I have no control over the yaml that Memex writes, I will go with the default Obsidian behavior:

![](03_frontmatter_default.png)

This allows me to see the URL and other annotations (like, Spaces) that Memex puts per default, while at the same time either collapsing, or entirely hiding the frontmatter in other places.


