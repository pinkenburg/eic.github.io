---
title: How-to
name: howto
layout: default
---
{% include layouts/title.md %}

* TOC
{:toc}
<hr/>
#### Overview
The following will be of interest to the collaborators interested in contributing to this
site or involved in its maintenance.

We use GitHub to manage the code for this site. Please take a look at the
{% include navigation/findlink.md name='github_site' tag='GitHub repository' %}
to get an idea of the general organization of the data, layouts and supporing logic.
The idea is to shape the code and content in a way that is easy to navigate
and modify. The following sections explain how this is achieved.
Development of this site involves the following aspects:
* adding and modifying the structured data content
* adding "assets" i.e. documents, images etc - although this needs to be done sparingly: one
needs to keep in mind that there are practical limits on the size of any repository, as well
as hard limits on repositories of sites which are hosted as "GitHub pages"
* interfacing the navigation tools on this site, which for the most part consists
of the top navigation bar and the dropdown menus

<hr/>
#### Navigation Mechanics
##### The Menu System
Entries in the navigation bar on top are named similar to the folders in this
project, for example the "Software" is associated with the folder "_software" etc.
These folders are treated as "collections" by the Jekyll framework and they need to be declared
in the main configuration file
<a href="https://github.com/eic/eic.github.io/blob/master/_config.yml" target="_blank">_config.yml</a>.
As one can see in 
<a href="https://github.com/eic/eic.github.io/blob/master/_includes/layouts/navbar.html" target="_blank">the navigation bar code</a>,
its menu entries are generated automatically based on the content of the file
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target="_blank">_data/menus.yml</a>.
The content and order of top entries in the navigation bar as well as content and ordering of all items in all dropdown
menus are defined in that file.

If a menu entry in
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target=_blank>_data/menus.yml</a>
is an external link, there is no additional source code (i.e. markdown page content is not necessary).
Examples are easy to spot in the menu definition file
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target=_blank>_data/menus.yml</a>.

Menu entry descriptions in *menu.yml* can also have optional attributes:
* **"div"**: to include a divider right above an entry in a dropdown menu the
following line should be added respective section of the _data/menus.yml file: "*div: yes*".
* **"exclude"** -  mainly for development purposes; if a section of the menu
needs to be ignored when building the site one can just easily add "*exclude: yes*" to the repective section.
* **"label"**: to insert a label into a dropdown menu. The label won't have any links or page associations, it's just a visual guide.

##### Front Matter
"Front Matter" is a piece of YAML code (typically short) embedded on the very top of a
Markdown-formatted document which can contain any sort of data, for example variables
used to render the page or define a tag so it can be more easily referenced in this way.
The data in the Front Matter is accessible in a way similar to object attributes.
Importantly, when present the Front Matter serves to inform {% include navigation/findlink.md name='Jekyll' %}
that this particular page needs to be rendered into HTML for inclusion in the site being built. Otherwise
it won't be included in the site. In summary, most of the files used to build the site
are expected to be in the Markdown (MD) format, and **each MD file is to be equipped with the
"front matter" section**, which could look like the excerpt below.
```
---
title: My Cool Software
name: my_cool_software
category: mysoftware
layout: default
---
```

<hr/>
#### Helper macros
##### Links
Frequently used **external** links are located in the following registry:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/links.yml" target=_blank>_data/links.yml</a>.

They can be easily inserted in pages in a consistent manner by their mneumonic name using
a macro as decribed below. This simplifies handling user-unfriendly links and ensures they
remain the same across the site and makes
it easy to point to a different resource when necessary. For example, this piece of code
{% raw %}
{% include navigation/findlink.md name='Jekyll' %}
{% endraw %}
will result in the link: {% include navigation/findlink.md name='Jekyll' %}. If you would like the displayed
link name to be different, the optional "tag" argument for the macro will do that.
Example:
{% raw %}
{% include navigation/findlink.md name='github_site' tag='repository' %}
{% endraw %}
will result in the following link: {% include navigation/findlink.md name='github_site' tag='repository' %}.
Links will automatically open in a new tab.

There is a similar macro for use with links to **internal** pages on the site. Example:
{% raw %}
{% include navigation/pagelink.md folder=site.about name='howto' tag='"How-to" page' %}
{% endraw %}
will result in: {% include navigation/pagelink.md folder=site.about name='howto' tag='"How-to" page' %} (this happens
to point to the page you are reading now).
Primary advantage of this macro is that it allows renaming the source file - while keeping the same "name" attribute
in the Front Matter section the links will still be correct.

##### Images
Users and developers have complete freedom in how they incorporate images into pages on this site.
In many cases handling images will be facilitated by adding an image to the following registry:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/gallery.yml" target=_blank>_data/gallery.yml</a>
and then using a simple macro to refer to the image by its mneumonic name and automatically generate
the correct link, as illustrated in the following example:
{% raw %}
{% include images/image.md name='news_banner' width='400' %}
{% endraw %}
...will produce

{% include images/image.md name='news_banner' width='400' %}

This will help to ensure consistency of image links throughout the site and remove
the need to include cumbersome HTML in the page code. It also makes it possible to move
the content across folders if necessary and only make changes to the path in one place.
Additionally, a macro generating a gallery of images based on selection criteria is
under development.

##### Doecuments
Similar to the logic presented above, it is preferable to create references to documents (both internal to the site
but also as references to cloud storage such as Zenodo) in an organized manner. Similar to examples above,
there is a registry of documents on the site:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/documents.yml" target=_blank>_data/documents.yml</a>.
It is quite small as the time of writing but is expected to grow. Documents can be referred to their names and links
autogeneraged by using a macro such as one in this example:
{% raw %}
{% include documents/doc.md name='Jung-MCValidation.pdf' %}
{% endraw %}

One of the advantages of this approach is that the source of the document can be relocated within the site
or even uploaded to an external resources while the links in the site code will remain valid provided
corrections are made in just one place: _documents.yml.

<hr/>
#### Managing Data
Jekyll is fairly flexible when it comes to storing and manipulating structured data.
The data component of the site can reside in the "front matter" section of the Markdown-formatted
files or in separate YAML (or JSON, CSV etc) data sources. The front matter approach works well
for small sites. For scalability, it is recommended to rely mostly on dedicated data files (i.e.
files in the "<a href="https://github.com/eic/eic.github.io/tree/master/_data" target="_blank">_data</a>" folder)
and keep the content of the front matter sections of individual MD files to a minimum.

The {% include navigation/findlink.md name='Liquid' %} template language
features a variety of filters that can be applied to the data stored in YAML and other data sources
as well as in the front matter blocks of pages, and decent (not perfect) support for collections,
iterators and flow control constructs.

Information assets on this site (e.g. imaged, PDF files etc) are stored in the aptly named
"<a href="https://github.com/eic/eic.github.io/tree/master/assets/" target="_blank">assets</a>"
folder and its subsiduaries. This convention should be kept going forward.

#### Formatting
We aim to provide a uniform look and feel across the site. To that end, whenever possible
the head (title) of each page is formatted by using the standard include layouts/title.md - please
look a the code for examples of its use. It's renders the page title from the front matter sections.

Headers of sections within a page are currently formatted in Markdown as "header level 4" i.e. prepended
with four hash characters like in **"#### My section header"**. Take a look at the header (Formatting)
of this section to get an idea of how it's rendered.

<hr/>
#### Development and Deployment
##### Ruby and Jekyll
To productively participate in the development of this site one needs to learn the basics of the
{% include navigation/findlink.md name='Jekyll' %} framework and install it on
a development machine. This is relatively straighforward but may depend on the OS specifics.

Once the environment is in place, any modification can be validated immediately since
the locally running development server provided by Jekyll will render the site
on the local host. Basic knolwede of the {% include navigation/findlink.md name='Liquid' %}
template language and in particular the "filters" that are part of it is extremely helpful.

##### GitHub Pages
As already mentioned, we use GitHub to manage the code for this site. A fork/pull request process is optimal,
but if you prefer direct push for some reason please get in touch with the group. To get access to the code,
create a clone of the website repository and move to the directory created:

  ```bash
  git clone https://github.com/eic/eic.github.io.git
  cd eic.github.io
  ```

The repository is configured for ''GitHub Pages''.
This means that once modified contents are committed to the repository and you perform *git push*, the
{% include navigation/findlink.md name='github_pages' tag='GitHub pages system' %} will build the
site and make it publicly available at [https://eic.github.io/](https://eic.github.io/). Errors in the
updated code may result either in failed deployment or a broken instance of the site. For that reason
it is desirable to assess the result of changes before publishing them.
To achieve this, you need to install Jekyll on your local machine, as detailed below.

##### Jekyll
Instructions can be found on Jekyll [web site](https://jekyllrb.com/docs/installation/) but the short story is:

* Install {% include navigation/findlink.md name='ruby_downloads' tag='Ruby' %} and {% include navigation/findlink.md name='rubygems_downloads' tag='RubyGems' %}
* Install Bundler (a Ruby package manager): `gem install bundler`
* Install/update your Jekyll installation (must be done regularly):
  ```bash
  bundler update
  ```
* Run Jekyll (in the eic.github.io directory):
  ```bash
  bundler exec jekyll serve
  ```

Once Jekyll has been started you can view the web site by connecting to `localhost:4000`.
Changes made to files are immediately compiled and reflected on the
displayed site (at the next page load). The optimal workflow is to make changes and debug
entirely locally before uploading the final changes to GitHub.

Examples of Jekyll commands
```
# Start a new Jekyll project
## NB. this is a little tricky e.g. you would need to pick a correct
## Gemfile and perhaps use --force
## A little experimentation is in order
jekyll new myblog

# Clean stale CSS etc, also run after modifyig _config.yml.
bundle exec jekyll clean
# Serve content while filtering out some annoying warning messages
bundle exec jekyll serve 2>&1 | grep -v ': Using'
```

If you get this far, you are ready to start modifying the site. A word of caution - please check the output
of the jekyll compiler for error messages. Normally the code compiles and you just reload the page to see
the result, but if there was an error and you are not checking the compiler's windown the behavior of the
page will be puzzling.


[top](#how-to)

