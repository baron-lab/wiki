---
title: Home
layout: home
nav_order: 1
---

# Welcome to the Baron Lab Wiki! 

Use the navigation bar to find information, or search for topics.

To contribute, use the below instructions.

## Adding pages or editing

- Small changes can be made directly editing files in gitlab. Otherwise:
    1. Clone [https://github.com/baron-lab/wiki](https://github.com/baron-lab/wiki)
    1. Make a new branch
    1. Make your changes 
        - Add new pages to the docs directory
        - New pages should have extension .md
        - New pages need following lines at top:
            ```
            ---
            title: REPLACEWITHPAGETITLE
            layout: default
            ---
            ```
    1. Push your branch to github
    1. Merge if you have permissions for that, or make a merge request 

- Alternatively, send updated .md file to C. Baron (not recommended for frequent contributors)

- Pages will automatically get added to the sidebar on the wiki website

- After committing to main, the website will be automatically updated (can take up to 10 min)

- Use Markdown for formatting. See [https://www.markdownguide.org/cheat-sheet/](https://www.markdownguide.org/cheat-sheet/)

## Building and previewing your site locally

Not required, but nice for seeing a preview before pushing to github. Assuming [Jekyll] and [Bundler] are installed on your computer:

1.  Change your working directory to the root directory of your site.
1.  Run `bundle install`.
1.  Run `bundle exec jekyll serve` to build your site and preview it at `localhost:4000`.

    The built site is stored in the directory `_site`.

## Licensing and Attribution

This repository is licensed under the [MIT License].
This repository was generated using a template from [https://github.com/just-the-docs/just-the-docs](https://github.com/just-the-docs/just-the-docs)

