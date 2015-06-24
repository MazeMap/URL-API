# Embedding MazeMap

This document describes how to embed MazeMap on your website.

## The basic iframe

Use the HTML `<iframe>` tag and give it a `src` attribute:

```
<iframe src="http://use.mazemap.com/embed-single.html?YOUR-PARAMETER-SPECIFICATION"
  width="600" height="420" frameborder="0" scrolling="no" marginheight="0"
  marginwidth="0" style="border: 1px solid grey"></iframe>
```

* `src` should be given the URL with your wanted parameters specified. See the [URL API documentation](https://github.com/MazeMap/URL-API/blob/master/URL-API.md) for details on which parameters can be specified. The `src` should always start with `http://use.mazemap.com/embed-single.html?`.

The above code looks like this:

<iframe src="http://use.mazemap.com/embed-single.html?"
  width="600" height="420" frameborder="0" scrolling="no" marginheight="0"
  marginwidth="0" style="border: 1px solid grey"></iframe>


## Placement of the 'Open in new window' link

```
http://use.mazemap.com/embed-single.html?newtablink=inside
```

* `newtablink` can be either `inside`, `outside` or `none`.

