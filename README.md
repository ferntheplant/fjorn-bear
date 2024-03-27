# Fjorn's new blog

The blog is now hosted via [Bear](https://bearblog.dev/) for its simplicity and ease of use.

TODO: upgrade to premium to point to fjorn.dev

## Writing a new blog post

TODO: setup script to create new markdown file in `blogs/` with frontmatter filled out. Specifically have `make_discoverable: false` to turn off the toast upvote thing.

Once a blog post is written publish it through the Bear UI by copy-pasting the markdown (including frontmatter) into the provided textarea. Copy-paste the frontmatter into the "wal-mart" brand frontmatter section of the Bear UI and leave a single `---` line at the top of the blog post content.

### Adding a TOC

Use [markdown-toc](https://github.com/jonschlinkert/markdown-toc) to add a table of contents to any post.

1. Edit the post to have `<!-- toc -->` at the top of the contents
2. Run `markdown-toc --append=$'\n<br></br>' -i blogs/<blog-link-name>`
3. Add string "Table of contents" to top of blog and edit whitespace around TOC as needed

## Styling

Copy and paste the `theme.css` file into the styling theme on the [Bear Dashboard](https://bearblog.dev/dashboard/).
TODO: restructure the css and commend it for future me
