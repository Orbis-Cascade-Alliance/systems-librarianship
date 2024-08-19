# Systems Librarianship 101
This repository generates the website for the FY2025 series of lessons on hard and soft skills for new systems librarians.

## Adding Pages to the Site

Add and edit pages within the /_pages directory.

Set the titles, permalinks, and order in the frontmatter. (Between the lines of three dashes at the top.)

All pages will appear in the menu to the left. For example, the page with an order of 1 will appear first in the list, followed by the page with order 2, etc.

## Markdown

All pages are written in Markdown. See [Basic Syntax](https://www.markdownguide.org/basic-syntax/)

## Multimedia

To add images to a page, add them to the /assets/img folder. Use the complete path in the image source, including the repository. Example:

    ![Artistic rendering of computers\](/systems-librarianship/assets/img/binary-monitor-particles-600px.jpg)
	
To embed YouTube videos, use the iframe embed code. Example:

    <iframe width="560" height="315" src="https://www.youtube.com/embed/08VDn3vdD4A?si=EBxc050TnfEXUjAV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen\></iframe>

## Testing

If editing the website on Github.com, preview page contents using the button at the top of the editing window. The website will update soon after changes are committed to the source branch.

To run the website on another machine for development, see the [Jekyll Installation](https://jekyllrb.com/docs/installation/) documentation and [Testing your GitHub Pages site locally with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll). Clone this repository rather than creating a new site.
