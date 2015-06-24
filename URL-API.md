# MazeMap URL API

This document describes the basic use of the MazeMap URL API.


## Specifying a specific URL API version

```
http://use.mazemap.com/?v=1
```

* `v` defines the version, in this case `1`.

The URL API supports versioning using the `v` parameter, however it is not necessary to specify as the API assumes the latest version by default. In the following examples we omit this parameter.


## Specifying a campus

To start MazeMap with a specific campus in view:

```
http://use.mazemap.com/?campusid=1
```

* `campusid` defines the campus in view, in this case `1`.

To specify other parameters you must also specify the campus to provide proper context.

#### Examples of `campusid` values

* `1` NTNU Gløshaugen
* `3` St. Olavs Hospital
* `4` OSL
* `5` UiT
* `0` Special value that tells the application to start with no particular campus by default.

A full list of campus IDs migth be available in the future.

#### Omitting campusid
As of 2015-06-18, if you specify a POI that is _**globally**_ unique, the `campusid` parameter can be omitted. Note however that POIs defined using `identifier` are not globally unique, and therefore needs either the `campusid` or `campuses` parameter for proper context. One of these must be provided.


## Limiting the list of locations

For some institutions, it is possible to only show the institution's campuses in the 'Choose location' list that is shown when the view is zoomed out enough.

```
http://use.mazemap.com/?campuses=ntnu&zoom=12
```

* `campuses` defines which group of campuses to show in the list.
* `zoom` defines a specific zoom value. It is explained in detail later.

#### Examples of `campuses` values

* `ntnu` NTNU
* `uit` University of Tromsø
* `uib` University of Bergen


## Specifying a POI (Point of Interest)

#### POIs in general

There are 3 different types of POIs:

1. `sharepoitype` defines a sharing point. Use to share a single point.
2. `starttype` defines a starting point. Use as the start of a path.
3. `desttype` defines a destination point. Use as the destination of a path or as a single destination point.

They can be specified in 3 different ways:

1. `poi` defines a MazeMap POI ID
2. `point` defines a generic point by longitude, latitude and floor
3. `identifier` defines a local ID

This defines the format expected by the parameter related to the type of POI.

* `sharepoi` for `sharepoitype`
* `start` for `starttype`
* `dest` for `desttype`

The syntax is as follows:

```
<sharepoi,start,dest>type=poi&<sharepoi,start,dest>=<MazeMap POI ID>
<sharepoi,start,dest>type=point&<sharepoi,start,dest>=<longitude>,<latitude>,<floor>
<sharepoi,start,dest>type=identifier&<sharepoi,start,dest>=<Customer defined ID>
```


#### Specifying destination by POI ID

To start MazeMap with a specific POI in view:

```
http://use.mazemap.com/?campusid=1&desttype=poi&dest=593
```

* `desttype` defines the format expected by the `dest` parameter. In this case a POI ID.
* `dest` defines the specific POI ID.

#### Specifying destination by a generic point

To start MazeMap with a generic point in view:

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4026794,63.4183615,0
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035833,63.4178412,3
```

* `desttype` defines the format expected by the `dest` parameter. In this case a generic point.
* `dest` defines the geographical point as a comma-separated list `dest=longitude,latitude,z-index` where `z-index` is used to specify the floor, if the point lies indoors. The value of `z-index` _usually_ matches the floor, but this might vary from building to building. If outdoors, the `z-index` parameter should be 0, or it can simply be dropped.

#### Specifying destination by a customer defined ID

To start MazeMap with a customer defined point in view:

```
http://use.mazemap.com/?campusid=18&desttype=identifier&dest=810-2425
```

* `desttype` defines the format expected by the `dest` parameter. In this case an identifier.
* `dest` defines the customer defined ID of the POI. In this case room 8102425 at NTNU Dragvoll.


## Specifying a path

To start MazeMap with a predefined path:

```
http://use.mazemap.com/?campusid=1&starttype=point&start=10.4029047,63.4186015,0&desttype=poi&dest=35994
```

* `starttype` defines the format expected by the `start` parameter. In this case a geographical point outside.
* `start` defines the geographical point in a comma-separated list.
* `desttype` In the example, a POI is used as destination type.
* `dest` In the example the POI ID for MazeMap office is used as destination.


## Specifying custom names for points

When starting MazeMap either with predefined points (e.g. if you have defined a path), you can input a custom string for the names of these points. I.e. if you you are linking to an office with id 404, you can specify that it should display as "John's Office".

```
http://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035968,63.4175039,6&starttype=point&start=10.4030281,63.4185463,0&startname=Start Here&destname=John's Office
```

* `startname` defines the string to be used for the start point.
* `destname` defines the string to be used for the destination point.

If you defined a `sharepoi`, use `sharepoiname` to customize the name.

```
http://use.mazemap.com/?campusid=1&sharepoitype=point&sharepoi=10.40153,63.41809,1&sharepoiname=Vending Machine X
```


## Specifying a custom zoom

To override the default zoom:

```
http://use.mazemap.com/?campusid=1&zoom=17
```

* `zoom` defines a specific zoom value. It can have a value between 1-22.


## Specifying a custom view

To override the default center point of the view, use the `viewtype` and `view` parameters. The syntax was discussed in general under 'POIs in general'. The value of `viewtype` defines the format expected by the `view` parameter.

```
viewtype=poi&view=<MazeMap POI ID>
viewtype=point&view=<longitude>,<latitude>,<floor>
viewtype=identifier&view=<Customer defined ID>
```

To center the starting point of the path defined above (instead of centering the path which is default):

```
http://use.mazemap.com/?campusid=1&viewtype=point&view=10.40290,63.41860,0&starttype=point&start=10.40290,63.41860,0&desttype=identifier&dest=322-620
```

* `viewtype` defines the format expected by the `view` parameter. In this case, a `point`.
* `view` is set to be the same point as `start`


## Simulating a position

To simulate the position of the user, use the `postype` and `pos` parameters. The syntax was discussed in general under 'POIs in general'. The value of `postype` defines the format expected by the `pos` parameter.

```
postype=poi&pos=<MazeMap POI ID>
postype=point&pos=<longitude>,<latitude>,<floor>
postype=identifier&pos=<Customer defined ID>
```

```
http://use.mazemap.com/?campusid=1&postype=point&pos=10.40213,63.41879,0
```

* `postype` defines the format expected by the `pos` parameter. In this case, a `point`.
* `pos` defines the geographical point in a comma-separated list.

#### Warning!

If not used in the correct way, simulating positions can easily confuse the user. Be careful using it for other purposes than stationary info screens.


## Adding type POIs

Type POIs are generic types of POIs.

```
http://use.mazemap.com/?campusid=1&typepois=9,27
```

* `typepois` specifies a comma-separated list with `typepoiids`.


#### Examples of typepoiids

* `4` PC
* `7` Study area
* `9` WC
* `27` Bus stop
* `28` Parking
* `114` Power outlets

A full list of type POI IDs migth be available in the future.


## Specifying a language

```
http://use.mazemap.com/?campusid=1&lang=nb
```

* `lang` defines which language to use.
  * `en` English
  * `nb` Norsk Bokmål


## Specifying a search string

To start MazeMap with a predefined search string:

```
http://use.mazemap.com/?campusid=1&search=Your string
```

* `search` defines the string.


## Specifying a custom floor level

To override the default floor level:

```
http://use.mazemap.com/?campusid=1&zlevel=5
```

* `zlevel` defines which floor the view is set to. This might be different from building to building, i.e. `zlevel=2` might not always mean the 2nd floor.
