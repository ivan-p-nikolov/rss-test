# Content feed examples

This repo holds different mapping representations from our content model to a feed entry.
Currently, we want to provide the following content fields in the feed:

- title
- short description
- byline
- body
- authors
- publish date

And add data fields to the feed entry not present in the model:

- wordcount

Most of those fields don't have clear definitions in RSS/Atom standard so here are some potential representations:

## RSS

[feed.xml](https://github.com/ivan-p-nikolov/rss-test/blob/master/feed.xml) is an RSS 2.0 example that uses some well-known namespaces and a custom one `xmlns:ftc="http://ft.com/namespace/rss/1.0/content"`
This example is a little hacky. By definition `description` and `content:encoded` are interchangeable and should provide a short item description.
But we use both for different purpose in an effort to make the feed as painless to integrate as possible.
The net effect is that general purpose rss clients would visualize the content body as item description.

### Mapping

| model             | feed item           |
|-------------------|---------------------|
| title             | \<title\>           |
| short description | \<description\>     |
| byline            | \<ftc:byline\>      |
| body              | \<content:encoded\> |
| authors           | \<dc:creator\>      |
| publish date      | \<pubData\>         |
| word count        | \<ftc:wordcount\>   |

## Basic Atom

[atom.xml](https://github.com/ivan-p-nikolov/rss-test/blob/master/atom.xml) is Atom representation of the content feed.
In this example we define custom namespace `xmlns:ftc="http://ft.com/namespace/rss/1.0/content"` to cover the fields not defined in the Atom standard.
The same caveats as `RSS` example apply here. Commercial rss clients treat `summary` and `content` interchangeably and would visualize the `content` tag as item description.

### Mapping

| model             | feed item         |
|-------------------|-------------------|
| title             | \<title\>         |
| short description | \<summary\>       |
| byline            | \<ftc:byline\>    |
| body              | \<content\>       |
| authors           | \<author\>        |
| publish date      | \<updated\>       |
| word count        | \<ftc:wordcount\> |

## Complex Atom

[complex-atom.xml](https://github.com/ivan-p-nikolov/rss-test/blob/master/complex-atom.xml) is more complex representation that relays on the fact that Atom allows for `content` tag to contain more than text. 
In this example we serialize all the fields that we want to provide to the customer in a xml object inside `content` tag of the feed entry.

### Mapping

| model             | feed item         |
|-------------------|-------------------|
| title             | \<title\>         |
| short description | \<summary\>       |
| **content model** | \<content\>       |
| authors           | \<author\>        |
| publish date      | \<updated\>       | 
