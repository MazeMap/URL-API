# MazeMap URL API

This document describes the basic use of the MazeMap URL API.

<br>

## Specifying a specific URL API version

```
https://use.mazemap.com/?v=1
```

* `v` defines the version, in this case `1`.

The URL API supports versioning using the `v` parameter, however it is _**not necessary**_ to specify as the API assumes the latest version by default. In the following examples we omit this parameter.

<br>
---
<br>

## Specifying a campus

To start MazeMap with a specific campus in view:

```
https://use.mazemap.com/?campusid=1
```

* `campusid` defines the campus in view, in this case `1`.

To specify other parameters you must also specify the campus to provide proper context.
Note: `0` is a special value that tells the application to start with no particular campus by default.

#### Omitting `campusid`
As of 2015-06-18, if you specify a POI that is _**globally unique**_, the `campusid` parameter can be omitted. Note however that POIs defined using `identifier` are not globally unique, and therefore needs either the `campusid` or `campuses` parameter for proper context. One of these must be provided.

<br>
---
<br>

## Specifying a limited campus context

For some use-cases, you want to launch the app with only a specifc campus available fro browsing. This is done with the `campuses` parameter. This is a pre-configured tag that must be configured by the MazeMap team. Contact MazeMap to do this.

```
https://use.mazemap.com/?campuses=ntnu
```

* `campuses` defines which group of campuses to show in the list using the configured tag string.

<br>
---
<br>

## Specifying a POI (Point of Interest)

<a name="pois-in-general"></a>
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


#### Specifying a destination by a POI ID

To start MazeMap with a specific POI in view:

```
https://use.mazemap.com/?campusid=1&desttype=poi&dest=593
```

* `desttype` defines the format expected by the `dest` parameter. In this case a POI ID.
* `dest` defines the specific POI ID.

#### Specifying a destination by a generic point

To start MazeMap with a generic point in view:

```
https://use.mazemap.com/?campusid=1&desttype=point&dest=10.4026794,63.4183615,0
https://use.mazemap.com/?campusid=1&desttype=point&dest=10.4035833,63.4178412,3
```

* `desttype` defines the format expected by the `dest` parameter. In this case a generic point.
* `dest` defines the geographical point as a comma-separated list `dest=longitude,latitude,z-index` where `z-index` is used to specify the floor, if the point lies indoors. The value of `z-index` _usually_ matches the floor, but this might vary from building to building. If outdoors, the `z-index` parameter should be 0, or it can simply be dropped.

#### Specifying a destination by a customer specific reference ID

Every POI/room has an internal reference identifier. To link to a specific POI identifier, in view:

```
https://use.mazemap.com/?campusid=18&desttype=identifier&dest=810-2425
```

* `desttype` defines the format expected by the `dest` parameter. In this case an identifier.
* `dest` defines the lokal reference ID of the POI. In this case room 810-2425 at NTNU Dragvoll.

#### Facility management reference ID's, from Lydia, Tririga, etc
Some organizations have IDs to buildings/floors from an internal system, such as Lydia or IBM Tririga, etc. Such IDs could be synchronized with MazeMap and used in links in the following way:

* A specific "referenceprovider" must be created in MazeMap
* The reference ID is then used to identify the specific resource

Example link to a *building* with a Lydia reference ID set up for NTNU customer:
* The building reference id is `301`
```
https://use.mazemap.com/?campuses=ntnu&campusid=1&referenceprovider=ntnulydia&sharepoitype=building&sharepoi=301
```

Example link to a *floor* with a Lydia reference ID set up for NTNU customer:
The building reference id is `301` and the floor id is `k`, resulting in the reference `301k`
```
https://use.mazemap.com/?campuses=ntnu&campusid=1&referenceprovider=ntnulydia&sharepoitype=floor&sharepoi=301k
```


<br>
---
<br>

## Specifying a building by MazeMap internal building ID's

To start MazeMap with a predefined building in view:

```
https://use.mazemap.com/?v=1&sharepoitype=bld&sharepoi=57&campuses=ntnu
```

* `sharepoitype` defines the format expected by the `sharepoi` parameter. In this case we specify a 'bld' type
* `sharepoi` defines the ID of the building. This is an internal MazeMap ID, and might not be persistent forever.

<br>
---
<br>

## Specifying a path

To start MazeMap with a predefined path:

```
https://use.mazemap.com/?campusid=1&starttype=point&start=10.4029047,63.4186015,0&desttype=poi&dest=35994
```

* `starttype` defines the format expected by the `start` parameter. In this case a geographical point outside.
* `start` defines the geographical point in a comma-separated list.
* `desttype` In the example, a POI is used as destination type.
* `dest` In the example the POI ID for MazeMap office is used as destination.

<br>
---
<br>


## Specifying a custom zoom

To override the default zoom:

```
https://use.mazemap.com/?campusid=1&zoom=17
```

* `zoom` defines a specific zoom value. It can have a value between 1-22.

<br>
---
<br>



## Specifying a custom bearing/rotation

To override the default north rotated map:

```
https://use.mazemap.com/?campusid=1&zoom=17&bearing=45
```

* `bearing` defines a specific rotation value in degrees from north.

<br>
---
<br>


## Adding POI Categories

POI categories are generic types of POIs.

```
https://use.mazemap.com/?campusid=1&typepois=9,27
```

* `typepois` specifies a comma-separated list with IDs.


#### Examples of `typepois` ids

* `4` PC
* `7` Study area
* `9` WC
* `27` Bus stop
* `28` Parking
* `114` Power outlets

POI type category IDs are configured per-campus. Contact MazeMap for a list of your category IDs, or instructions on how to set-up these yourself.

<br>
---
<br>

## Forcing a language

To override the user's language settings:

```
https://use.mazemap.com/?campusid=1&lang=nb
```

* `lang` defines which language to use.
  * `en` English
  * `nb` Norsk Bokmål

<br>
---
<br>

## Specifying a search string

To start MazeMap with a predefined search string:

```
https://use.mazemap.com/?campusid=1&search=Your string
```

* `search` defines the string.

<br>
---
<br>

## Specifying a custom floor level

To override the default floor level:

```
https://use.mazemap.com/?campusid=1&zlevel=5
```

* `zlevel` defines which floor the view is set to. This might be different from building to building, i.e. `zlevel=2` might not always mean the 2nd floor.

<br>
---
<br>

## Disabling 'Scroll to zoom' functionality

```
https://use.mazemap.com/?wheelzoom=false
```

* `wheelzoom` can have the following values:
  * `true` (default)
  * `false`

<br>
---
<br>

## Modifying 'share map' header button behaviour

#### Disabling 'share map' functionality
Will hide the share button from the header
```
https://use.mazemap.com/?sharemode=false
```


#### Enable URL Scheme functionality
For integration with native apps
```
https://use.mazemap.com/?sharemode=url_scheme
```

See the spesific document that describes this functionality with example iOS / Android code
[Native URL Scheme Implementation Documentation](https://github.com/MazeMap/URL-API/blob/master/UrlSchemeSharing.md)
<br>
---
<br>


# List of all parameters

* `v` specifies the url version number.
* `campusid` specifies the campus ID.
* `campuses` limits the list of campuses with a pre-configured tag string.
* `sharepoitype` specifies what format `sharepoi` should expect.
* `sharepoi` specifies a POI either by poi, identifier or point.
* `starttype` specifies what format `start` should expect.
* `start` specifies a POI either by poi, identifier or point.
* `desttype` specifies what format `dest` should expect.
* `dest` specifies a POI either by poi, identifier or point.
* `zoom` specifies a custom zoom level (1-22).
* `typepois` specifies a comma-separated list with IDs of generic POIs (WC, Bus stops, etc.)
* `lang` overrides user's language settings.
* `search` start application with a predefined search string.
* `zlevel` override default floor level.
* `wheelzoom` specifies whether zooming by scrolling is enabled
* `sharemode` specifies the behaviour of the share map button in the app header
* `positioning` if set to 'disabled', allows for disabling the "find my location" functionality. If set to `true`, it will auto-start the positioning.
* `bearing` specifies a view rotation in degrees from north.
