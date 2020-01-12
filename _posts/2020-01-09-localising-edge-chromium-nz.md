---
layout: post
title:  Localising Edge for NZ
date:   2020-01-09T15:00:00+13:00
author: Scott Pettit
categories: intune
tags: edge
---

Microsoft are about to release the new [Edge](https://www.microsoftedgeinsider.com/en-gb/) browser built on Chromium on January 15th. We've been using the Dev channel internally at [End 2 End](https://www.end2end.co.nz) and [Vorco](https://www.vorco.net) for a while now and I'm a big fan and definitely excited about being able to roll out the stable version to our clients very shortly. In preparation for this I wanted to share a few useful settings that are relevant in our part of the world:

## Default Search - Google NZ

In New Zealand Google pretty much have all of the web search market. Bing/Yahoo!/DuckDuckGo are virtually unheard of. Google works better with localised search results though. It took me a while to track down the right URL to make search suggestions work so here we go.

### Step 1 - Default search provider name

I've named it "Google NZ" so we don't clash with just "Google" (we don't want the generic .com results).

<a href="/assets/posts/edgesearch1.png" data-lightbox="edgesearch1-large" data-title="Edge Search 1">
<img src="/assets/posts/edgesearch1.png" alt="Edge Search 1" title="Edge Search 1"/>
</a>

### Step 2 - Default search provider search URL

This is the URL that provides the search results (not suggestions) back to Edge - set this to https://www.google.co.nz/search?q={searchTerms}

<a href="/assets/posts/edgesearch2.png" data-lightbox="edgesearch2-large" data-title="Edge Search 2">
<img src="/assets/posts/edgesearch2.png" alt="Edge Search 2" title="Edge Search 2"/>
</a>

### Step 3 - Default search provider URL for suggestions

This gets the suggestions popping up as you type in the address bar - set this to https://www.google.co.nz/complete/search?output=chrome&amp;q={searchTerms}

<a href="/assets/posts/edgesearch3.png" data-lightbox="edgesearch3-large" data-title="Edge Search 3">
<img src="/assets/posts/edgesearch3.png" alt="Edge Search 3" title="Edge Search 3"/>
</a>

### Step 4 - Enable search suggestions

You have to turn on search suggestions otherwise just setting the URL as above does nothing.

<a href="/assets/posts/edgesearch4.png" data-lightbox="edgesearch4-large" data-title="Edge Search 4">
<img src="/assets/posts/edgesearch4.png" alt="Edge Search 4" title="Edge Search 4"/>
</a>

### Step 5 - Enable the default search provider

Like enabling search suggestions you also need to activate overriding the default search provider.

<a href="/assets/posts/edgesearch5.png" data-lightbox="edgesearch5-large" data-title="Edge Search 5">
<img src="/assets/posts/edgesearch5.png" alt="Edge Search 5" title="Edge Search 5"/>
</a>

Enjoy your now localised search results!

Happy to hear from anyone who knows the search and suggestion URLs for other search engines that support localisation too.

## Make spellcheck speak English

By default the spellcheck uses American English (simplified English?), here's how to change it to Australian English since that's the nearest equivalent to New Zealand English:

<a href="/assets/posts/edgespellcheck.png" data-lightbox="edgespellcheck-large" data-title="Edge Spellcheck NZ">
<img src="/assets/posts/edgespellcheck.png" alt="Edge Spellcheck NZ" title="Edge Spellcheck NZ"/>
</a>