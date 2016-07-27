# Embedding MazeMap using iframes

This document describes how to embed MazeMap on your website using iframes.

## The basic iframe

Use the HTML `<iframe>` tag and give it a `src` attribute:

```
<iframe src="http://use.mazemap.com/embed.html?YOUR-PARAMETER-SPECIFICATION"
  width="100%" height="420" frameborder="0" marginheight="0" marginwidth="0"
  scrolling="no"></iframe>
```

* `src` should be given the URL with your wanted parameters specified. See the [URL API documentation](https://github.com/MazeMap/URL-API/blob/master/URL-API.md) for details on which parameters can be specified. The `src` attribute should always start with `http://use.mazemap.com/embed.html?`.
* `width`. We reccomend using `width="100%"` to make the iframe more responsive. If you don't use %, but instead specify the width in pixels, it might not look optimal on e.g. small screens.

See the [`<iframe>` documentation](https://developer.mozilla.org/en/docs/Web/HTML/Element/iframe) for all attributes (`width`, `height`, `frameborder`, etc.) that can be specified.


## Placement of the 'Open in new window' link

```
http://use.mazemap.com/embed.html?newtablink=inside
```

* `newtablink` can have the following values:
  * `outside` (default)
  * `inside`
  * `false` (don't display the link)
