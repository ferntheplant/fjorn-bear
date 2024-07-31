# Fjorn's new blog

The blog is now hosted via [Bear](https://bearblog.dev/) for its simplicity and ease of use.

## Writing a new blog post

TODO: setup script to create new markdown file in `blogs/` with frontmatter filled out. Specifically have `make_discoverable: false` to turn off the toast up vote thing.

Once a blog post is written, publish it through the Bear UI by copy-pasting the markdown (including frontmatter) into the provided textarea. Copy-paste the frontmatter into the "Walmart" brand frontmatter section of the Bear UI and leave a single `---` line at the top of the blog post content.

### Adding a TOC

Use [markdown-toc](https://github.com/jonschlinkert/markdown-toc) to add a table of contents to any post.

1. Edit the post to have `<!-- toc -->` at the top of the contents
2. Run `markdown-toc --append=$'\n<br></br>' -i blogs/<blog-link-name>`
3. Add string "Table of contents" to top of blog and edit whitespace around TOC as needed

## Adding to the Good list

Simply add a new item to the Markdown list with the relevant icon and a time tag prepended.

TODO: find a nicer way to do this via bear posts or some automation script

```html
<!-- publish date -->
<i><time datetime="2024-03-27-04:00" /></i>
<!--
  - 4 for March - Nov (EDT)
  - 5 for Nov - March (EST)
  - must wrap in <i> so other <time> elements don't get the "opaque" style
-->
<!-- article icon -->
<span class="reading-list-icon"><i class="gg-file-document"></i></span>

<!-- book icon -->
<span class="reading-list-icon"><i class="gg-readme"></i></span>

<!-- youtube video icon -->
<span class="reading-list-icon"><i class="gg-youtube"></i></span>
```

## Scripts

Custom scripts can be included in the `Settings > Header and footer directives` section of the dashboard. Copy-paste the script tag in `header.html` to the text box.

Custom scripting should be kept to an absolute minimum for basic styling needs.

### Current Scripting Uses

- override the `/blog/` page to have custom `<p>` tag inserted before blog list
- "active menu" nav selections
- auto-format dates in `<time>` tags to make adding to "good" list more convenient

## Styling

Copy and paste the `theme.css` file into the styling theme on the [Bear Dashboard](https://bearblog.dev/dashboard/).
TODO: restructure the CSS and commend it for future me
