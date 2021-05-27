---
title:  "Updating My Site With Chirpy"
date:   2021-05-27 18:45:00 -0500
categories: [Computers, Web]
tags: [website]
---

## Intro

Howdy!

Seeing as this site was a little bland, I've decided to update it with a new theme and some new tabs. I've been putting off this update for a decent bit, but once I got around to seeing it through I found it actually quite refreshing.

## Theme

For my site, I decided to use the Chirpy theme. It's minimal, responsive, and reminds me of the theme that GitHub uses, which I quite like. It boasts a slew of features, including syntax highlighting, Google Analytics, a table of contents, a search bar, pinned posts, and support for mathematical expressions and Mermaid diagrams. It also looks brilliant out of the box, which is a very big plus for me. I'm not too good at front-end development as it is, so the less mucking about I have to do in a theme, the better. Overall, this theme seems perfect for a feature-rich website.

Here are some links for Chirpy, if you're interested:  
[Main repo](https://github.com/cotes2020/jekyll-theme-chirpy/)  
[Demo site](https://chirpy.cotes.info)  

Unfortunately, installing Chirpy took me a bit of time, as I ran into a version conflict: the Chirpy gem, `jekyll-themes-chirpy`, needed `Jekyll 4.1`, but the gem required for GitHub Pages publishing system to work, `github-pages`, needed `Jekyll 3.9`. As it turns out, GitHub Pages was still using a pretty outdated version of Jekyll. I had two options for resolving this: replace Chirpy with another theme, or replace GitHub Pages with another publishing system.

### New theme?

I found a few sources for themes that are compatible with `github-pages` out of the box. The first one was GitHub Pages' own [Supported themes](https://pages.github.com/themes/) page, which boasted a decent collection of themes that natively worked with GitHub Pages. However, none of the themes looked as nice as Chirpy out of the box (in my opinion), and were lacking in overall features. There were also some themes available from [Jekyll Themes](https://jekyllthemes.io/github-pages-themes), but the free themes left a lot to be desired, and the paid themes looked nice enough, but were out of my price range. As much as I value this website, I'm not looking to spend $50 on a theme for it!

### GitHub Actions

Since I couldn't find a compatible theme that I liked more than Chirpy, I decided to rip out the `github-pages` gem and find an alternative way to publish my site. Fortunately, I found a very lovely [article](https://jekyllrb.com/docs/continuous-integration/github-actions/) on Jekyll's main page that detailed how to publish a static website for GitHub Pages with the use of GitHub Actions. By using this method, I was able to use whatever version of Jekyll I wanted, thereby solving the version conflict. It took a bit of time to get working right, but in the end I got GitHub Actions to completely automate my entire publishing process, from pushing a commit to seeing the updated website. Once this was in place, installing Chirpy was a cakewalk. Very nice.

## New Tabs

Seeing as this site was intended to be used as a personal portfolio, I might as well make it into one. For this, I've added some new tabs.

### About

This tab just has some info on who I am, what I do, and what I'm interested in. It's a little sparse at the moment, but I can always add to it down the road.

### Categories, Tags, and Archives

These tabs came with Chirpy to better organize my posts, and I decided to keep them. They didn't need much tweaking, so I left them as-is and they seem to work just fine.

## Conclusion

Well, this was quite the adventure into site building! I've gotten Chirpy to play nice with Jekyll, and set up a Github Actions system to publish my website. I also added a few new pages to my site, turning a sparse blogging website into a sparse portfolio/blogging website. I'd call that an improvement!

Ciao for now,  
\- CF
