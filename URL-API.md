MazeMap URL API
===============

This document describes the basic use of the MazeMap URL API.


Specifying a specific URL API version
--------------------
```
http://use.mazemap.com/?v=1
```
* `v` defines the version, in this case `1`.

The URL API supports versioning using the `v` parameter, however it is not necessary to specify as the API assumes the latest version by default. In the following examples we omit this parameter. There is legacy functionality from version 0.5 that still works. More on that further down.


Specifying a campus
--------------------
To start MazeMap with a specific campus in view:
```
http://use.mazemap.com/?campusid=1
```
* `campusid` defines the campus in view, in this case `1`.

To specify other parameters you must also specify the campus to provide proper context.

#### List of campusids
* 1 = NTNU Gløshaugen
* 3 = St.Olavs Hospital
* 5 = UiT
* ...

[Full list of campus IDs](TODO: provide this URL)


Specifying a POI (Point of Interest)
--------------------

#### POIs in general
There are 3 different types of POIs:

1. `sharepoitype`
2. `starttype`
3. `desttype`

They can be specified in 3 different ways:

1. `poi` defines a MazeMap POI ID
2. `point` defines a generic point by longitude, latitude and floor
3. `identifier` defines a local ID

This defines the format expected by the parameter related to the type of POI.

* `sharepoi` for `sharepoitype`
* `start` for `starttype`
* `dest` for `desttype`

The general syntax is as follows:

```
<sharepoi,start,dest>type=poi&<sharepoi,start,dest>=<MazeMap POI ID>
<sharepoi,start,dest>type=point&<sharepoi,start,dest>=<longitude>,<latitude>,<floor>
<sharepoi,start,dest>type=identifier&<sharepoi,start,dest>=<Lydia Building ID>-<Lydia Room ID>
```


#### Specifying a POI destination
To start MazeMap with a specific POI in view:
```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=593
```

* `desttype` defines the format expected by the `dest` parameter. In this case a POI ID.
* `dest` defines the specific POI ID.

#### Specifying a generic point destination
To start MazeMap with a generic point in view:
```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4026794,63.4183615,0
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035833,63.4178412,3
```
* `desttype` defines the format expected by the `dest` parameter. In this case a generic point.
* `dest` defines the geographical point as a comma-separated list:
* ```
  dest=longitude,latitude,z
  ```
  * `z` is used to specify the _floor_ if the point lies inside a building. If outside, the `z` parameter should be 0, or it can simply be dropped.


Defining a path
--------------------
To start MazeMap with a predefined path:
```
http://use.mazemap.com/?campusid=1&starttype=point&start=10.4029047,63.4186015,0&desttype=poi&dest=35994
```
* `starttype` defines the format expected by the `start` parameter. In this case a geographical point outside.
* `start` defines the geographical point in a comma-separated list.
* `desttype` In the example, a POI is used as destination type.
* `dest` In the example the poiID for MazeMap office is used as destination.


Defining custom names for points
-----------------------------------------------
When starting MazeMap either with predefined points (e.g. if you have defined a path), you can input a custom string for the names of these points. I.e. if you you are linking to an office with id 404, you can specify that it should display as "John's Office". Use `%20` for spaces.

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035968,63.4175039,6&starttype=point&start=10.4030281,63.4185463,0&startname=Start%20Here&destname=John's%20Office
```
* `startname` defines the string to be used for the start point.
* `destname` defines the string to be used for the destination point.

If you defined a `sharepoi`, use `sharepoiname` to customize the name.
```
http://use.mazemap.com/?campusid=1&sharepoitype=point&sharepoi=10.40153,63.41809,1&sharepoiname=Awesome%20Vending%20Machine
```


Defining a custom zoom
----------------------
To override the default zoom:

```
http://use.mazemap.com/?campusid=1&zoom=17
```
* `zoom` can have a value between 1-22


Defining a view
---------------
To override the default center point of the view, use the `viewtype` and `view` parameters. They follow the same syntax as the POIs, where the value of `viewtype` defines the format expected by the `view` parameter.

```
viewtype=poi&view=<MazeMap POI ID>
viewtype=point&view=<longitude>,<latitude>,<floor>
viewtype=identifier&view=<Lydia Building ID>-<Lydia Room ID>
```

To center the starting point of the path defined above (instead of centering the path which is default).

```
http://use.mazemap.com/?campusid=1&viewtype=point&view=10.40290,63.41860,0&starttype=point&start=10.40290,63.41860,0&desttype=identifier&dest=322-620
```
* `viewtype` defines the format expected by the `view` parameter. In this case, a `point`.
* `view` is set to be the same point as `start`


Simulating a position
---------------------
To simulate the position of the user, use the `postype` and `pos` parameters. They follow the same syntax as the POIs, where the value of `postype` defines the format expected by the `pos` parameter.

```
postype=poi&pos=<MazeMap POI ID>
postype=point&pos=<longitude>,<latitude>,<floor>
postype=identifier&pos=<Lydia Building ID>-<Lydia Room ID>
```

```
http://use.mazemap.com/?campusid=1&postype=point&pos=10.40213,63.41879,0
```
* `postype` defines the format expected by the `pos` parameter. In this case, a `point`.
* `pos` defines the geographical point in a comma-separated list.

#### Warning!
If not used in the correct way, simulating positions can easily confuse the user. Be careful using it for other purposes than stationary info screens.


Specifying the placement of 'Open in new window' link
-----------------------------------------------------
```
http://use.mazemap.com/?campusid=1&newtablink=inside
```
* `newtablink` can be either `inside`, `outside` or `none`. It's intended to be used with iframes, e.g. when embedding maps on web sites.


Adding type POIs
-------------
Type POIs are generic types of POIs.

```
http://use.mazemap.com/?campusid=1&typepois=9,27
```
* `typepois` specifies a comma-separated list with `typepoiids`.


#### List of typepoiids
* 4 = PC
* 7 = Study area
* 9 = WC
* 27 = Bus stop
* 28 = Parking
* ...

[Full list of type POI IDs](TODO: provide this URL)


Specifying a language
----------------------
```
http://use.mazemap.com/?campusid=1&lang=nb
```
* `lang` defines which language to use.
  * `en` English
  * `nb` Norsk Bokmål


Defining a search string
------------------------
To start MazeMap with a predefined search string:
```
http://use.mazemap.com/?campusid=1&search=Your%20string
```
* `search` defines the string. It must be [URL encoded](https://en.wikipedia.org/wiki/Percent-encoding). In this example, `%20` is used for the space character.


More on version legacy
----------------------
TODO
