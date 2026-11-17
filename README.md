# Site setup

This repository uses Jekyll to turn Markdown files into a static site. The homepage lists published posts from newest to oldest.

## Write a post

Create `_posts/YYYY-MM-DD-your-slug.md` using the date you want the post to show and a short lowercase slug. Add this header, then write the post below the second set of dashes:

```markdown
---
title: ""
---
```

Put your title inside the quotes. You can also add `summary: ""` for a short description and `tags: []` for topic labels. A title, date, optional summary and tags are already displayed by the layout, so start the body directly and use `##` for its main sections. The filename supplies the date and URL. Finished posts dated in the future are skipped until an eligible build.

For a draft, use `_drafts/your-slug.md` with the same header. Files in `_drafts` are ignored by Git, so drafts stay on your machine. When it's ready, rename the file with a date and move it to `_posts/`. For a post that has already been published, keep its filename if you want to keep its URL.

Markdown supports fenced code blocks (add the language after the opening backticks for highlighting), tables and footnotes. Put images in `assets/images/` and reference them with a base-aware path, for example `![your alt text]({{ '/assets/images/diagram.png' | relative_url }})`.

## Add your site copy

- Set `title` and optionally `description` in `_config.yml`. The title appears on the homepage and in the browser tab, and the description is metadata.
- Write the homepage introduction in `index.md` below its single front matter block. Use Markdown headings such as `##` for sections of the introduction.

## Preview locally

Install Ruby 3.3 and Bundler first (on Windows, use [RubyInstaller with Devkit](https://rubyinstaller.org/downloads/)). From the repository folder:

```sh
bundle install
bundle exec jekyll serve --livereload
```

Open `http://localhost:4000/` and keep the terminal running while you edit. Run `bundle install` on first setup or when the Gemfile changes. To include local drafts, add `--drafts` to the serve command. Restart the server after changing `_config.yml`. Press Ctrl+C to stop it.

## Publish on GitHub Pages

1. Create or choose a GitHub repository and push these files to its `main` branch. Use an empty repository for a new site. A repo named `<username>.github.io` gives you `https://<username>.github.io/`; another name gives you `https://<username>.github.io/<repository>/`.
2. In the repository, open **Settings → Pages**. Set **Source** to **Deploy from a branch**, choose **main** and **/(root)**, then save. GitHub builds the site and shows its URL in Pages settings.
3. Commit and push new posts to `main` (or create them directly on GitHub). Pages rebuilds automatically. There's no index to edit.

GitHub Pages supplies the repository path in its hosted build. The local preview uses the root path. See [GitHub's Pages setup guide](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) for details.
