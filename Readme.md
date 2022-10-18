# How to install

1. Install [Jekyll](https://jekyllrb.com/)
1. Checkout Repository
1. In repository directory run `bundle install` - installs all needed components
1. In repository directory run `bundle exec jekyll serve` or the `serve` bash script included in the repo
1. Browse [127.0.0.1:4000](http://127.0.0.1:4000)


## Issues
* outdated gem files, remove Gemfile.lock and run `bundle install` again
* nokogiri missing zlib install with system libraries `gem install nokogiri -- --use-system-libraries` or `bundle config build.nokogiri --use-system-libraries` then `bundle install`

## Blogging
To add a blog post put a new file in the `_posts` directory using the `YYYY-MM-DD-title.md` file format.

## About
Simple enough, just edit the about.md file in the root director

## Projects
Any github repositories you want to showcase should go here and will be displayed in descending date order under the github icon tab.  Just add a new file using the `YYYY-MM-DD-title.md` file format to the projects directory.

## Portfolio
Any published applications that you've worked on should go here.  Just add a new file using the `YYYY-MM-DD-title.md` file format to the projects directory.

## Tutorials
Experimental section for building a referential tutorial blogging system

## Resume
Resume is a bit more difficult as it's build using the `resume.json` file and converted via the [JSON Resume](https://jsonresume.org/) tools.  You'll need to install those tools and use them to re-generate after update the file.

* `npm i resume-cli`

There are 2 HTML versions of the resume `resume.html` which uses the **Elegant** theme and `resume2.html` which uses the **Kendall** theme.
You will need to install the themes:

* `npm i jsonresume-theme-elegant`
* `npm i jsonresume-theme-kendall`
* `npm i jsonresume-theme-paper`

And to generate the HTML version use `npx resume export --theme paper resume.html`, `npx resume export --theme elegant resume2.html`, and `npx resume export --theme kendall resume3.html`
