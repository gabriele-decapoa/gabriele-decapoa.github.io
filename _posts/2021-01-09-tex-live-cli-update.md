---
layout: post
title: TeX Live update via CLI
categories:
  - LaTeX
---

Writing my BSc thesis I discovered [LaTeX](https://en.wikipedia.org/wiki/LaTeX), a software used to prepare documents looking like a book.
LaTeX, in fact, is a very good typography software, that allows users to write plain texts and then, with the help of some markup tags, split the text in chapter and section, add styles (bold, italics, etc.), cross-references, citations, etc.  

At the beginning I used a TeX distribution called [MikTeX](https://en.wikipedia.org/wiki/MiKTeX), because I used a Windows OS.
Step by step, as soon as I understood how much powerful this software is, I started to use [TeX Live](https://en.wikipedia.org/wiki/TeX_Live) on Unix OS.  
Those two distributions have a main difference: MikTex, as mainly used on Windows, is more "automatic", because could hide some details as package installation.
On the contrary, TeX Live forces the user to install manually each additional package.  
This difference is due mainly to distribution configuration at setup time, so depending on how you configure the distribution during the installation you will not face this behavior.

TeX Live distribution install a native manager users could run via shell: `tlmgr`.
Via `tlmgr` it is possible to install new packages, updates installed packages or the distribution itself, and more over.

To install a new package, the command is:
```bash
tlmgr install PACKAGE_NAME
```

To update a package, the command is:
```bash
tlmgr update PACKAGE_NAME
```

To update all installed packages, the command is:
```bash
tlmgr update --all
```

To update the distribution itself, the command is:
```bash
tlmgr update --self
```

