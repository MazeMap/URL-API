MazeMap URL API
===============

This document describes the basic use of the MazeMap URL API.


Specifying the URL API version
--------------------
The URL API supports versioning using the _**v**_ parameter, however it is not necessary to specify, as the API assumes the latest version by default. There are legacy functionality from version 0.5 that still works. More on that further down.

```
http://use.mazemap.com/?v=1
```


Specifying a campus
--------------------
To start mazemap with a specific campus in view, use the _**campusid**_ parameter. For other parameters, you must also specify the campus to provide proper context.

```
http://use.mazemap.com/?campusid=1
```
#### List of campusid's
>* 1 = NTNU
>* 3 = St.Olavs Hospital
>* 5 = UiT
>* ...


Specifying a POI (Point of Interest)
--------------------

#### Types of POIs
There are 3 different types of POIs:

1. `sharepoitype`
2. `starttype`
3. `desttype`

They can be specified in 3 different ways:

1. `poi`
2. `point`
3. `identifier`

The type defines the format expected by the related parameter:

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
To start mazemap with a specific POI in view, use `desttype=poi` and `dest=<POI>`.

```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=593
```

* `desttype` defines the format expected for the `dest` parameter. For a POI, use `poi` here.
* `dest` defines the specific POI ID.

#### Specifying a generic point destination
To start mazemap with a generic point in view, use `desttype=point` to specify longitude, latitude and floor.

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4026794,63.4183615,0
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035833,63.4178412,3
```
* `desttype` defines the format expected for the `dest` parameter. For a generic point, use `point` here.
*  `dest` defines the geographical point in a comma-separated array as shown below.
```
dest=longitude,latitude,z
```
The `z` parameter is used to specify the _floor_ if the point lies inside a building. If outside, the `z` parameter can be dropped, or simply be 0.


Defining a path
--------------------
To start mazemap with a predefined path, use the `starttype` and `start` parameters along with the `desttype` and `dest` parameters (described above).

```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=35994&starttype=point&start=10.4029047,63.4186015,0
```
*   `starttype` defines the format expected for the `start` parameter. In the example above, a geographical _point_ outside is used, as described earlier.
*   `start` defines the geographical point in a comma-separated array as described earlier. It can also be `poi` or `identifier` with the corresponding format for the `start` parameter (described above).
*   `desttype` In the example, a POI is used as destination type.
*   `dest` In the example the poiID for MazeMap office is used as destination.


Defining custom names for points
-----------------------------------------------
When starting MazeMap either with predefined points (e.g. if you have defined a path), you can input a custom string for the names of these points. I.e. if you you are linking to an office with id 404, you can specify that it should display as "John's Office". Use `%20` for spaces.

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035968,63.4175039,6&starttype=point&start=10.4030281,63.4185463,0&startname=Start%20Here&destname=John's%20Office
```
*   `startname` defines the string to be used for the start point.
*   `destname` defines the string to be used for the end point.

If you defined a `sharepoi`, use `sharepoiname` to customize the name.

```
http://use.mazemap.com/?campusid=1&sharepoitype=point&sharepoi=10.40153,63.41809,1&sharepoiname=Awesome%20Vending%20Machine
```


Defining a custom zoom
----------------------
To override the default zoom, use the `zoom` parameter.

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
http://use.mazemap.com/?v=1&campusid=1&viewtype=point&view=10.40290,63.41860,0&starttype=point&start=10.40290,63.41860,0&desttype=identifier&dest=322-620
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
http://use.mazemap.com/?v=1&campusid=1&postype=point&pos=10.40213,63.41879,0
```

#### Warning!
If not used in the correct way, simulating positions can easily confuse the user. Be careful using it for other purposes than stationary info screens.


More on version legacy
----------------------
TODO
