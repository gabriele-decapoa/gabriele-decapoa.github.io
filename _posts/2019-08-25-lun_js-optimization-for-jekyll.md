---
layout: post
title: Lunr.js optimization for Jekyll
categories:
  - Node_js
---
I started to use [Jekyll](https://jekyllrb.com/) to have a simple framework to learn for my blog, and it is very simple.
This framework is the one used here, inside Github Pages, so you do not have to pay any hosting service for that!  
But what is a blog without a search bar?

Basically, a blog is a set of documents, and the best tool to define indices and allow searching on documents is [Apache Solr](https://lucene.apache.org/solr/).
Anyway, Solr is a Java tool and required to be installed into the hosted server.
In Github Pages it is not easy to do that, so you need some more lightweight tool, written in another language easy to install, like [Lunr.js](https://lunrjs.com/).

There are lots of blog posts in the Net that explain how to use Lunr.js in a Jekyll blog, but all builds the search index in browser memory.
If you have lots of documents, it will be not so efficient, and could cause the crash of your browser.  
This is why I developed an optimization version of those posts, as you could notice in {% gist 9e6561d958da2ee8bd671d6732323aa6 %}.
`build-index.js` and `package.json` files define a Node.js scripts that reads the `_posts` directory and define a couple of static Javascript file, one with the documents' list and one with the index.
`search.js` is a client-side Javascript file you would import into your blog that uses jQuery to render search results.s