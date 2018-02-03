# [arrangravestock.co.uk](http://arrangravestock.co.uk)
Blog using modified version of [Pixyll](https://github.com/johnotander/pixyll), replacing the old version which does not support markdown posts

## Geting started
Cd to your directory and clone to your path using:
```
git clone https://github.com/ArranGravestock/ArranGravestock.github.io.git my-blog
```

Posts can be found in
```
---|default
---|---|_posts
```

To add a post create a new .md file with name of `yyyy-mm-dd-post-name`, and using a markdown create a header as follows at the first line:
```
layout: post
title: <insert post title>
date: <insert date in format yyy-mm-dd>
summary: <insert summary provided after title>
categories: <insert tags associated with the post>
---
```
The rest of the document after this header will be in markdown format, and you can even use HTML tags in the file.

Markdown cheatsheet can be found [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

It is recommended that you modify the original [pixyll](https://github.com/johnotander/pixyll) rather than cloning my modified version.

The `_config.yml` can also be updated to reflect your website, follow the instructions in the [pixyll](https://github.com/johnotander/pixyll) readme

## License
This project is licensed under the MIT License - see the LICENSE.md files for details
