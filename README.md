
# SIGDEM File format specification

## File

SIGDEM files have the .sigdem file extension. The filename before the extension is the baseName.

**File extension**: sigdem

**Mime Type**: image/x-revolsys-sigdem

SIGDEM files can also be wrapped in a .gz ([baseName].sigdem.gz) or .zip ([baseName].sigdem.zip) file.
The baseName of the .sigdem file inside the zip file must be the same as the baseName of the zip file.

Software implementing the SIGDEM file formats SHOULD be written to directly read .gz and .zip compressed files.

## Header

A SIGDEM file starts with the following 132 byte file header.

| Field              | Type    | Description |
|--------------------|---------|-------------|
| fileId             | char[6] | The file identification characters "SIGDEM" |
| version            | short   | The current version number (e.g. 1) |
| coordinateSystemId | int     | The [EPSG](https://epsg.io) coordinate system id (e.g. [https://epsg.io/26910](26910) NAD83 / UTM zone 10N). Use 0 if there is no EPSG ID for that coordinate system. |
| offsetX            | double  | The offset to add to the x value   |
| scaleX             | double  | The scale factor for x coordinates |
| offsetY            | double  | The offset to add to the y value   |
| scaleY             | double  | The scale factor for y coordinates |
| offsetZ            | double  | The offset to add to the z value   |
| scaleX             | double  | The scale factor for z coordinates |
| minX               | double  | The minimum x coordinate of the file bounding box |
| minY               | double  | The minimum y coordinate of the file bounding box |
| minZ               | double  | The minimum z value of the elevations stored      |
| maxX               | double  | The maximum x coordinate of the file bounding box |
| maxY               | double  | The maximum y coordinate of the file bounding box |
| maxZ               | double  | The maximum z value of the elevations stored      |
| gridWidth          | int     | The number of grid cells in each row |
| gridHeight         | int     | The number of rows in the grid |
| gridCellWidth      | double  | The width of each cell in x units |
| gridCellHeight     | double  | The height of each cell in y units |

## Content

## Supplementary files

For ease of distribution the use of supplementary files should be avoided. Multiple files would
require wrapping all the files in a .zip container.

> **NOTE:** Other supplementary files can be included in a distribution package but these should not
> be expected to be handled by software processing .sigdem files. 


### Projection (.prj) file

Only if there an EPSG coordinateSystemId doesn't exist for the coordinate system a ESRI WKT .prj file
be provided. The .prj filename will be [baseName].prj (e.g. canada.prj for canada.sigdem).

> **NOTE: Implementations and users MUST not generate and ignore this file if the SIGDEM file includes a coordinateSystemId.**

## Data types

The following data types are used. The names are based on the Java type names. All data is stored
using big-endian byte order.

| Name    | Byte Count | Description |
|---------|------------|-------------|
| char[n] | n          | Array of UTF-8 characters. n is the array length. Current use is the text SIGDEM which is also the ASCII subset of UTF-8 |
| short   | 2          | Big-endian 16-bit signed integer number |
| int     | 4          | Big-endian 32-bit signed integer number |
| double  | 8          | Big-endian 64-bit IEEE 754 floating point number |

