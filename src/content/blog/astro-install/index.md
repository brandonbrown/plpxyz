---
title: 'Building With Astro'
description: 'New Tech, new site'
pubDate: 'Jun 01 2024'
# heroImage: '/blog-placeholder-3.jpg'
---

Install to writing in less than 5 minutes. I'm sitting with my 4 year-old while she watches Across The Spiderverse on a Saturday morning, so I'm sure it could have been faster.

I decided to use the blog template option Astro offers out of the box to have more to see and play with. The README.md looks nicely setup for jumping into the sandbox but should be replaced. I want to keep the content as is to be able to reference back to some of the claims and expectations it has set. [The content can be found here](./original-readme).

This was the first "task" I tackled since install, and I have to say it was quite easy to understand what to do and how to do it. The task at hand was having a blog post with its own set of sub-paths for supporting content. Astro's directory structure made this an easy process.

From the blog template, the configuration allowed for me to understand I can:

1. Create a directory as the blog post holder
2. Add `index.md` for the content of the main post
3. Create supporting file `original-readme.md` to create the path `/astro-install/original-readme`
    > Note: This showed me the content still needs to be preceeded by front-matter, even if the content of the page is "utility" in nature. Makes sense to me.
4. Create a link to this content with markdown pointed to `(./original-readme)`
5. It works!

Let's deploy as is. I already have a [Netlify](https://www.netlify.com/) account, so that's where I'm going to send this thing. For the sake of building in public, this first deploy will only have this article and a few updates of personal information attached to it. Feedback on the deploy process and any gotchas from the out-of-the-box state to follow.

*After First Deploy*

Well, that was pretty seamless. Point Netlify to the correct branch of the project on GitHub, and you're essentially done. 

Things I noticed afterwards on Astro:

- /blog defaults to earliest post
- /nested/directories/ in blogs show on the main blog listing page

These are not too surprising, but good to be aware of.

For the first, in `pages/blog/index.astro`, it's a simple swap of the sort function they've already provided:

```
const posts = (await getCollection('blog')).sort(
	(a, b) =>  b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
);
```

Originally, this was `a - b`, a swap to `b - a` changes the rendering order.

For the second part, this is most likely related to Astro's `collections`, so I'll start [looking for this in the docs](https://docs.astro.build/en/guides/content-collections/#organizing-with-subdirectories).

After reading this, I thought my original excitement for organizing posts with subdirectories may have been misplaced. However, digging into this a bit more led me down a path of updating the blog frontmatter with an optional prop I've called `onIndex` set as a boolean. Back to the sort function, I've added a filter for this prop:

```
const posts = (await getCollection('blog', ({ data }) => {
  return data.onIndex !== false;
})).sort(
	(a, b) =>  b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
);
```

As long as `onIndex` is not explicitly set to `false`, then the correct level of blog posts are rendered. I'd like a solution that does not involve a manual task such as this, but problem solved for the time being.
