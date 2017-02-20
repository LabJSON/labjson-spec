### Contents

*   [1\. Introduction](#introduction)
    *   [1.1\. Examples](#examples)
    *   [1.2\. Definitions](#definitions)
*   [2\. LabJSON Objects](#labjson-objects)
    *   [2.1\. Geometry Objects](#geometry-objects)
        *   [2.1.1\. Positions](#positions)
        *   [2.1.2\. Point](#point)
        *   [2.1.3\. MultiPoint](#multipoint)
        *   [2.1.4\. LineString](#linestring)
        *   [2.1.5\. MultiLineString](#multilinestring)
        *   [2.1.6\. Polygon](#polygon)
        *   [2.1.7\. MultiPolygon](#multipolygon)
        *   [2.1.8\. Geometry Collection](#geometry-collection)
    *   [2.2\. Sample Objects](#sample-objects)
    *   [2.3\. Sample Collection Objects](#sample-collection-objects)
*   [3\. Coordinate Reference System Objects](#coordinate-reference-system-objects)
    *   [3.1\. Named CRS](#named-crs)
    *   [3.2\. Linked CRS](#linked-crs)
        *   [3.2.1\. Link Objects](#link-objects)
*   [4\. Bounding Boxes](#bounding-boxes)
*   [Appendix A. Geometry Examples](#appendix-a-geometry-examples)
    *   [Point](#id2)
    *   [LineString](#id3)
    *   [Polygon](#id4)
    *   [MultiPoint](#id5)
    *   [MultiLineString](#id6)
    *   [MultiPolygon](#id7)
    *   [GeometryCollection](#geometrycollection)
*   [Appendix B. Contributors](#appendix-b-contributors)



# The LabJSON Format Specification

<table class="docinfo">

<tbody>

<tr>

<th>Authors</th>

<td>Jason Dalton, Kyle Kalwarski (Azimuth1)</td>

</tr>

<tr>

<th>Revision</th>

<td>0.1</td>

</tr>

<tr>

<th>Date</th>

<td>16 June 2016</td>

</tr>

<tr>

<th>Abstract</th>

<td>LabJSON is an environmenatal laboratory data interchange format based on JavaScript Object Notation (JSON).</td>

</tr>

<tr>

<th>Copyright</th>

<td>Copyright © 2016 by the Authors. This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).</td>

</tr>

<tr>

<th>Status</th>

<td>Initial proposed draft.</td>

</tr>

</tbody>

</table>

## [1\. Introduction](#introduction)

LabJSON is a format for encoding a variety of recorded analyses of environmental samples. A LabJSON object may represent a soil, water, soil vapor, or air sample analysis, or a collection of samples. LabJSON supports the following attribute types: Unique sample identifier, Site or Project name, Sampling station identifier, Date, Top Depth, Bottom Depth, Basis, Matrix, Compound, Dilution Factor, Value, and Units. Samples in LabJSON contain analytical results for a sample and additional properties, and a sample collection represents a list of samples.

A complete LabJSON data structure is always an object (in JSON terms). In LabJSON, an object consists of a collection of name/value pairs -- also called members. For each member, the name is always a string. Member values are either a string, number, object, array or one of the literals: `true`, `false`, and `null`. An array consists of elements where each element is a value as described above.

### [1.1\. Examples](#examples)

A LabJSON sample collection:

<figure class="highlight">

      { "type": "SampleCollection",
        "samples": [
          { "type": "Sample",
            "geometry": {"type": "Point", "coordinates": [102.0, 0.5]},
            "properties": {"prop0": "value0"}
            },
          { "type": "Sample",
            "geometry": {
              "type": "LineString",
              "coordinates": [
                [102.0, 0.0], [103.0, 1.0], [104.0, 0.0], [105.0, 1.0]
                ]
              },
            "properties": {
              "prop0": "value0",
              "prop1": 0.0
              }
            },
          { "type": "Sample",
             "geometry": {
               "type": "Polygon",
               "coordinates": [
                 [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0],
                   [100.0, 1.0], [100.0, 0.0] ]
                 ]
             },
             "properties": {
               "prop0": "value0",
               "prop1": {"this": "that"}
               }
             }
           ]
         }

</figure>

### [1.2\. Definitions](#definitions)

*   JavaScript Object Notation (JSON), and the terms object, name, value, array, and number, are defined in [IETF RFC 4627](http://www.ietf.org/rfc/rfc4627.txt).

*   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [IETF RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

## [2\. LabJSON Objects](#labjson-objects)

LabJSON always consists of a single object. This object (referred to as the LabJSON object below) represents a collection of samples.

*   The LabJSON object may have any number of members (name/value pairs).

*   The LabJSON object must have a member with the name `"type"`. This member's value is a string that determines the type of the LabJSON object.

*   The value of the type member must be one of: `"Soil"`, `"Water"`, `"Air"`, `"Vapor"`, `"Sample"`, or `"SampleCollection"`. The case of the type member values must be as shown here.

*   A LabJSON object may have an optional `"crs"` member, the value of which must be a coordinate reference system object (see [3\. Coordinate Reference System Objects](#coordinate-reference-system-objects)).

### [2.1 Sample Objects](#geometry-objects)

A geometry is a LabJSON object where the type member's value is one of the following strings: `"Point"`, `"MultiPoint"`, `"LineString"`, `"MultiLineString"`, `"Polygon"`, `"MultiPolygon"`, or `"GeometryCollection"`.

A LabJSON geometry object of any type other than `"GeometryCollection"` must have a member with the name `"coordinates"`. The value of the coordinates member is always an array. The structure for the elements in this array is determined by the type of geometry.

#### [2.1.1\. Positions](#positions)

A position is the fundamental geometry construct. The `"coordinates"` member of a geometry object is composed of one position (in the case of a Point geometry), an array of positions (LineString or MultiPoint geometries), an array of arrays of positions (Polygons, MultiLineStrings), or a multidimensional array of positions (MultiPolygon).

A position is represented by an array of numbers. There must be at least two elements, and may be more. The order of elements must follow x, y, z order (easting, northing, altitude for coordinates in a projected coordinate reference system, or longitude, latitude, altitude for coordinates in a geographic coordinate reference system). Any number of additional elements are allowed -- interpretation and meaning of additional elements is beyond the scope of this specification.

Examples of positions and geometries are provided in [Appendix A. Geometry Examples](#appendix-a-geometry-examples).

#### [2.1.2\. Point](#point)

For type `"Point"`, the `"coordinates"` member must be a single position.

#### [2.1.3\. MultiPoint](#multipoint)

For type `"MultiPoint"`, the `"coordinates"` member must be an array of positions.

#### [2.1.4\. LineString](#linestring)

For type `"LineString"`, the `"coordinates"` member must be an array of two or more positions.

A LinearRing is closed LineString with 4 or more positions. The first and last positions are equivalent (they represent equivalent points). Though a LinearRing is not explicitly represented as a LabJSON geometry type, it is referred to in the Polygon geometry type definition.

#### [2.1.5\. MultiLineString](#multilinestring)

For type `"MultiLineString"`, the `"coordinates"` member must be an array of LineString coordinate arrays.

#### [2.1.6\. Polygon](#polygon)

For type `"Polygon"`, the `"coordinates"` member must be an array of LinearRing coordinate arrays. For Polygons with multiple rings, the first must be the exterior ring and any others must be interior rings or holes.

#### [2.1.7\. MultiPolygon](#multipolygon)

For type `"MultiPolygon"`, the `"coordinates"` member must be an array of Polygon coordinate arrays.

#### [2.1.8 Geometry Collection](#geometry-collection)

A LabJSON object with type `"GeometryCollection"` is a geometry object which represents a collection of geometry objects.

A geometry collection must have a member with the name `"geometries"`. The value corresponding to `"geometries"` is an array. Each element in this array is a LabJSON geometry object.

### [2.2\. Sample Objects](#sample-objects)

A LabJSON object with the type `"Sample"` is a sample object.

*   A sample object must have a member with the name `"geometry"`. The value of the geometry member is a geometry object as defined above or a JSON null value.

*   A sample object must have a member with the name `"properties"`. The value of the properties member is an object (any JSON object or a JSON null value).

*   If a sample has a commonly used identifier, that identifier should be included as a member of the sample object with the name `"id"`.

### [2.3\. Sample Collection Objects](#sample-collection-objects)

A LabJSON object with the type `"SampleCollection"` is a sample collection object.

An object of type `"SampleCollection"` must have a member with the name `"samples"`. The value corresponding to `"samples"` is an array. Each element in the array is a sample object as defined above.

## [3\. Coordinate Reference System Objects](#coordinate-reference-system-objects)

The coordinate reference system (CRS) of a LabJSON object is determined by its `"crs"` member (referred to as the CRS object below). If an object has no crs member, then its parent or grandparent object's crs member may be acquired. If no crs member can be so acquired, the default CRS shall apply to the LabJSON object.

*   The default CRS is a geographic coordinate reference system, using the WGS84 datum, and with longitude and latitude units of decimal degrees.

*   The value of a member named `"crs"` must be a JSON object (referred to as the CRS object below) or JSON null. If the value of CRS is null, no CRS can be assumed.

*   The crs member should be on the top-level LabJSON object in a hierarchy (in sample collection, sample, geometry order) and should not be repeated or overridden on children or grandchildren of the object.

*   A non-null CRS object has two mandatory members: `"type"` and `"properties"`.

*   The value of the type member must be a string, indicating the type of CRS object.

*   The value of the properties member must be an object.

*   CRS shall not change coordinate ordering (see [2.1.1\. Positions](#positions)).

### [3.1\. Named CRS](#named-crs)

A CRS object may indicate a coordinate reference system by name. In this case, the value of its `"type"` member must be the string `"name"`. The value of its `"properties"` member must be an object containing a `"name"` member. The value of that `"name"` member must be a string identifying a coordinate reference system. OGC CRS URNs such as `"urn:ogc:def:crs:OGC:1.3:CRS84"` shall be preferred over legacy identifiers such as `"EPSG:4326"`:

<figure class="highlight">

      "crs": {
        "type": "name",
        "properties": {
          "name": "urn:ogc:def:crs:OGC:1.3:CRS84"
          }
        }

</figure>

### [3.2\. Linked CRS](#linked-crs)

A CRS object may link to CRS parameters on the Web. In this case, the value of its `"type"` member must be the string `"link"`, and the value of its `"properties"` member must be a Link object (see [3.2.1\. Link Objects](#link-objects)).

#### [3.2.1\. Link Objects](#link-objects)

A link object has one required member: `"href"`, and one optional member: `"type"`.

The value of the required `"href"` member must be a dereferenceable URI.

The value of the optional `"type"` member must be a string that hints at the format used to represent CRS parameters at the provided URI. Suggested values are: `"proj4"`, `"ogcwkt"`, `"esriwkt"`, but others can be used:

<figure class="highlight">

      "crs": {
        "type": "link",
        "properties": {
          "href": "http://example.com/crs/42",
          "type": "proj4"
          }
        }

</figure>

Relative links may be used to direct processors to CRS parameters in an auxiliary file:

<figure class="highlight">

      "crs": {
        "type": "link",
        "properties": {
          "href": "data.crs",
          "type": "ogcwkt"
          }
        }

</figure>

## [4\. Bounding Boxes](#bounding-boxes)

To include information on the coordinate range for geometries, samples, or sample collections, a LabJSON object may have a member named `"bbox"`. The value of the bbox member must be a 2*n array where n is the number of dimensions represented in the contained geometries, with the lowest values for all axes followed by the highest values. The axes order of a bbox follows the axes order of geometries. In addition, the coordinate reference system for the bbox is assumed to match the coordinate reference system of the LabJSON object of which it is a member.

Example of a bbox member on a sample:

<figure class="highlight">

      { "type": "Sample",
        "bbox": [-10.0, -10.0, 10.0, 10.0],
        "geometry": {
          "type": "Polygon",
          "coordinates": [[
            [-10.0, -10.0], [10.0, -10.0], [10.0, 10.0], [-10.0, 10.0]
            ]]
          }
        ...
        }

</figure>

Example of a bbox member on a sample collection:

<figure class="highlight">

      { "type": "SampleCollection",
        "bbox": [100.0, 0.0, 105.0, 1.0],
        "samples": [
          ...
          ]
        }

</figure>

## [Appendix A. Geometry Examples](#appendix-a-geometry-examples)

Each of the examples below represents a complete LabJSON object. Note that unquoted whitespace is not significant in JSON. Whitespace is used in the examples to help illustrate the data structures, but is not required.

### [Point](#id2)

Point coordinates are in x, y order (easting, northing for projected coordinates, longitude, latitude for geographic coordinates):

<figure class="highlight">

      { "type": "Point", "coordinates": [100.0, 0.0] }

</figure>

### [LineString](#id3)

Coordinates of LineString are an array of positions (see [2.1.1\. Positions](#positions)):

<figure class="highlight">

      { "type": "LineString",
        "coordinates": [ [100.0, 0.0], [101.0, 1.0] ]
        }

</figure>

### [Polygon](#id4)

Coordinates of a Polygon are an array of LinearRing coordinate arrays. The first element in the array represents the exterior ring. Any subsequent elements represent interior rings (or holes).

No holes:

<figure class="highlight">

      { "type": "Polygon",
        "coordinates": [
          [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0], [100.0, 1.0], [100.0, 0.0] ]
          ]
       }

</figure>

With holes:

<figure class="highlight">

      { "type": "Polygon",
        "coordinates": [
          [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0], [100.0, 1.0], [100.0, 0.0] ],
          [ [100.2, 0.2], [100.8, 0.2], [100.8, 0.8], [100.2, 0.8], [100.2, 0.2] ]
          ]
       }

</figure>

### [MultiPoint](#id5)

Coordinates of a MultiPoint are an array of positions:

<figure class="highlight">

      { "type": "MultiPoint",
        "coordinates": [ [100.0, 0.0], [101.0, 1.0] ]
        }

</figure>

### [MultiLineString](#id6)

Coordinates of a MultiLineString are an array of LineString coordinate arrays:

<figure class="highlight">

      { "type": "MultiLineString",
        "coordinates": [
            [ [100.0, 0.0], [101.0, 1.0] ],
            [ [102.0, 2.0], [103.0, 3.0] ]
          ]
        }

</figure>

### [MultiPolygon](#id7)

Coordinates of a MultiPolygon are an array of Polygon coordinate arrays:

<figure class="highlight">

      { "type": "MultiPolygon",
        "coordinates": [
          [[[102.0, 2.0], [103.0, 2.0], [103.0, 3.0], [102.0, 3.0], [102.0, 2.0]]],
          [[[100.0, 0.0], [101.0, 0.0], [101.0, 1.0], [100.0, 1.0], [100.0, 0.0]],
           [[100.2, 0.2], [100.8, 0.2], [100.8, 0.8], [100.2, 0.8], [100.2, 0.2]]]
          ]
        }

</figure>

### [GeometryCollection](#geometrycollection)

Each element in the geometries array of a GeometryCollection is one of the geometry objects described above:

<figure class="highlight">

      { "type": "GeometryCollection",
        "geometries": [
          { "type": "Point",
            "coordinates": [100.0, 0.0]
            },
          { "type": "LineString",
            "coordinates": [ [101.0, 0.0], [102.0, 1.0] ]
            }
        ]
      }

</figure>

## [Appendix B. Contributors](#appendix-b-contributors)

The LabJSON format specification is the product of discussions with environmental lab information systems users and vendors.

</div>

</section>
