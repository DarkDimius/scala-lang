# SCALA-LANG.ORG

This repository contains the _static_ source of [scala-lang.org](http://scala-lang.org). It does not contain the source of any content found under the [docs.scala-lang.org](http://docs.scala-lang.org) subdomain (instead, visit the [scala.github.com repo](http://github.com/scala/scala.github.com) for that source).

It's a static site generated by [Jekyll](https://github.com/mojombo/jekyll), and uses a whole host of open-source tools including a touch of Twitter's Bootstrap.

## Dependencies

This site uses a Jekyll, a Ruby framework. The required Jekyll version is 1.5.1.

## Building the site

There are two ways to run Jekyll to build the site:

1. using Bundler, so Jekyll and accompanying gems are installed only inside this directory
2. using globally installed Jekyll and accompanying gems

The latter method is the one currently actually used on scala-lang.org. The
former method is likely most convenient for users who already have a different
version of Jekyll installed, or who are comfortable using Bundler and who don't
want anything else installed system-wide.

### Option 1) Building with Bundler

`cd` into the directory where you cloned this repository, then install the required gems with `bundle install`. This will automatically put the gems into `./bundle-vendor/bundle`.

Start the server in the context of the bundle:

    bundle exec jekyll serve

from this point, everything else should be the same, regardless of which method
you used to run Jekyll.

### Option 2) Building with global Jekyll

Install Jekyll 1.5.1 on your system using RubyGems:

    gem install jekyll -v 1.5.1

After cloning, cd into the directory where you cloned this repository and run:

    jekyll serve

and watch the output. You should see something like:

     Configuration file: /Users/ben/src/scala.github.com/_config.yml
                 Source: /Users/ben/src/scala.github.com
            Destination: /Users/ben/src/scala.github.com/_site
      Incremental build: enabled
           Generating... done.
      Auto-regeneration: enabled for '/Users/ben/src/scala-lang'

### Windows and UTF-8

If you get `incompatible encoding` errors when generating the site under Windows, then ensure that the
console in which you are running jekyll can work with UTF-8 characters. As described in the blog
[Solving UTF problem with Jekyll on Windows](http://joseoncode.com/2011/11/27/solving-utf-problem-with-jekyll-on-windows/)
you have to execute `chcp 65001`. This command is best added to the `jekyll.bat`-script.

## Viewing the site

Regardless of your method of running Jekyll, the generated site is available at `http://localhost:4000`.

If you add `--watch` to your Jekyll command line, Jekyll will automatically watch for changes on the filesystem. When you change a file, the console will show that jekyll is regenerating. Wait until it says `done` to refresh your browser.

## YAML Front Matter

The "YAML Front Matter" is nothing more than the header on each page that you intend for Jekyll to parse. It contains information such as the name of the HTML template (layout) chosen for the specific document, and the title of the document. An example YAML front matter might look like:

    ---
    layout: page
    title: My page title
    ---

You can use these fields in the YAML front matter later in your document. For example, to make a header with the title of the document, in markdown you would write:

    ---
    layout: page
    title: My page title
    ---

    # {{ page.title }}

    Body text here...

`# {{ page.title }}` would be rendered in HTML as, `<h1>My page title</h1>`.

## Markdown

There are dozens of guides and cheatsheets that cover markdown syntax out there, though this screenshot from the free OSX markdown editor, [Mou](http://mouapp.com/), is an excellent and concise reference:

![Mou screen shot](http://25.io/mou/img/1.png)

### Linking to internal pages

The least error-prone way to link between documents, to link to local images, or anything else: `[link text]({{ site.baseurl }}/path/to/page/page.html)`

Here, `{{ site.baseurl }}` is a site-wide variable that represents the root directory of the static site. So, to display the Scala logo image, located in `img/scala-logo.png`, one must simply write: `![Img alt text]({{ site.baseurl }}/resources/img/scala-logo.png)`



## Resources and Workflow

On every commit to the `scala/scala-lang` repository a [jenkins job](https://scala-webapps.epfl.ch/jenkins/view/All/job/scala-lang.org-builder/) will generate the site using jekyll and copy the resulting files to the webserver. **NOTE**: the `rsync` of this job also deletes whatever is in the webserver directory **with explicit exceptions**: we need to keep the files listed below. Kind of a hack.

There are additional files on the webserver:
  - Subdirectory `scala-lang.org/old` is a static copy of the old website. It was generated once and copied there, and it stays like that.
  - Most of the files in `/home/linuxsoft/archives/scala/` (on chara, accessible through ssh with your LAMP account) are synchronized to the subdirectory `scala-lang.org/files/archive` by another [hourly jenkins job](https://scala-webapps.epfl.ch/jenkins/view/All/job/scala-lang.org-scala-dist-archive-sync/). This folder is used by the nightly and release jenkins jobs to publish scala releases:
    - distribution files (tarballs etc) in `/`
      - older distribution files, RCs in `/old/` (not sure how exactly this is split up..)
    - api docs for distributions in `/api/`
    - nightly builds in `/nightly/distributions/`
    - nightly api builds in `/nightly/docs-xxx/`
    - nightly pdf builds (spec etc) in `/nightly/pdfs`

## Templates

We have the following (general) templates:
_(Note that this is not an exhaustive list.)_

#### page.html

Example YAML front matter with all possible fields:

    ---
    layout: page
    title: I Haz Build: An Autobiography of the Build Kitten
    by: Scala Jenkins (Build Kitty)
    ---

