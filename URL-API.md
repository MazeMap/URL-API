MazeMap URL API
===============

This document describes the basic use of the MazeMap URL API.


0. Specifying the URL API version
--------------------
The URL API supports versioning using the _**v**_ parameter, however it is not necessary to specify, as the API assumes the latest version by default. There are legacy functionality from version 0.5 that still works. More on that further down.

```
http://use.mazemap.com/?v=1
```


1. Specifying a campus
--------------------
To start mazemap with a specific campus in view, use the _**campusid**_ parameter. For other parameters, you must also specify the campus to provide proper context.

```
http://use.mazemap.com/?campusid=1
```
>#####campusid's#####
>* 1 = NTNU
>* 3 = St.Olavs Hospital
>* 5 = UiT
>* ...


Specifying a POI (Point of Interest)
--------------------

### Types of points
There are 3 different types of points:

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


### Specifying a POI destination
To start mazemap with a specific POI in view, use `desttype=poi` and `dest=<POI>`.

```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=593
```

* `desttype` defines the format expected for the `dest` parameter. For a POI, use `poi` here.
* `dest` defines the specific POI ID.

### Specifying a generic point destination
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


4. Defining a path
--------------------
To start mazemap with a predefined path, use the _**starttype**_ and _**start**_ parameters along with the previous _**desttype**_ and _**dest**_.

```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=35994&starttype=point&start=10.4029047,63.4186015,0
```
*   _**starttype**_ defines the type expected for the start parameter. In the example, a geographical _point_ outside is used, as described earlier.
*   _**start**_ defines the geographical point in 4326 comma-separated array as described earlier. Can also be a POI.
*   _**desttype**_ In the example, a POI is used as desttype.
*   _**dest**_ In the example the poiID for MazeMap office is used as destination.


5. Defining a custom name for start/destination
-----------------------------------------------
When starting MazeMap with predefined start or destination, you can input a custom string for the names of these points. I.e. if you you are linking to an office with id 404, you can specify that it should display as 'John's Office'.

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035968,63.4175039,6&starttype=point&start=10.4030281,63.4185463,0&startname=Start%20Here&destname=Johns%20Office
```
*   _**startname**_ defines the string to be used for the start point.
*   _**destname**_ defines the string to be used for the end point.


