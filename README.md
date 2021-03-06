# JSON-stat Command Line Conversion Tools

Command line tools for converting [JSON-stat](https://json-stat.org) documents.

```
npm install -g jsonstat-conv
```

Available commands:

* [csv2jsonstat](#csv2jsonstat) - converts CSV into JSON-stat
* [jsonstat2array](#jsonstat2array) - converts JSON-stat into an array of arrays
* [jsonstat2arrobj](#jsonstat2arrobj) - converts JSON-stat into an array of objects
* [jsonstat2csv](#jsonstat2csv) - converts JSON-stat into CSV
* [jsonstat2object](#jsonstat2object) - converts JSON-stat into an object
* [jsonstatslice (aka jsonstat2jsonstat)](#jsonstatslice-aka-jsonstat2jsonstat) - creates JSON-stat from JSON-stat
* [sdmx2jsonstat](#sdmx2jsonstat) - converts SDMX into JSON-stat

## Example

Get unemployment rate time series by country from Eurostat and convert it to CSV.

```
curl https://ec.europa.eu/eurostat/wdds/rest/data/v2.1/json/en/tesem120?precision=1 -o unr.jsonstat

jsonstat2csv unr.jsonstat unr.csv
```

Or using the stream interface:

```
curl https://ec.europa.eu/eurostat/wdds/rest/data/v2.1/json/en/tesem120?precision=1 | jsonstat2csv > unr.csv -t
```

More in the [Examples page](https://github.com/badosa/JSON-stat-conv/wiki/Examples).

## Shared options

### --help (-h)

Shows command's help.

```
jsonstat2csv --help
```

### --stream (-t)

Use the stream interface:

```
jsonstat2csv < oecd.json > oecd.csv -t
```

### --version

Shows jsonstat-conv version.

```
jsonstat2csv --version
```

## csv2jsonstat

Converts CSV into JSON-stat.

```
csv2jsonstat galicia.csv galicia.json
```

All options are ignored when the input is a rich CSV in the [JSON-stat Comma-Separated Values format](https://github.com/badosa/CSV-stat) (CSV-stat or JSV for short). See [jsonstat2csv](#jsonstat2csv).

#### --vlabel (-v)

String. Specifies the name of the value column. When not provided, the value column must be the last one.

```
csv2jsonstat galicia.csv galicia.json --vlabel val
```

#### --slabel (-l)

String. Specifies the name of the status column. Default is "Status". If no column has the specified name, no status information will be included in the output.

```
csv2jsonstat oecd.csv oecd.json --slabel stat
```

#### --column (-c)

String. Column separator. Default is ",".

```
csv2jsonstat galicia.ssv galicia.json --column ";"
```

#### --decimal (-d)

String. Decimal separator. Default is ".", unless column separator is ";" (then default is ",").

```
csv2jsonstat galicia.tsv galicia.json --column "\t" --decimal ","
```

#### --label (-a)

String. Dataset label.

```
csv2jsonstat galicia.csv galicia.json --label "Imported from galicia.csv"
```

#### --ovalue (-o)

Boolean. Includes a "value" property of type array instead of object.

#### --ostatus (-s)

Boolean. When status information is available, includes a "status" property of type array instead of object.

## jsonstat2array

Converts JSON-stat into an array of arrays.

```
jsonstat2array oecd.json oecd-array.json
```

### Options

#### --status (-s)

Boolean. Includes status information.

```
jsonstat2array oecd.json oecd-array.json --status
```

#### --vlabel (-v)

String. Specifies the label of the value field. Default is "Value".

```
jsonstat2array oecd.json oecd-array.json --vlabel val
```

#### --slabel (-l)

String. Specifies the label of the status field. Default is "Status".

```
jsonstat2array oecd.json oecd-array.json --status --slabel stat
```

#### --fid (-f)

Boolean. Identifies dimensions, value and status by ID instead of label.

```
jsonstat2array oecd.json oecd-array.json --fid
```

#### --cid (-c)

Boolean. Identifies categories by ID instead of label.

```
jsonstat2array oecd.json oecd-array.json --cid
```

## jsonstat2arrobj

Converts JSON-stat into an array of objects.

```
jsonstat2arrobj oecd.json oecd-arrobj.json
```

#### --status (-s)

Boolean. Includes status information.

```
jsonstat2arrobj oecd.json oecd-arrobj.json --status
```

#### --cid (-c)

Boolean. Identifies categories by ID instead of label.

```
jsonstat2arrobj oecd.json oecd-arrobj.json --cid
```

#### --unit (-u)

Boolean. Includes unit information when available.

```
jsonstat2arrobj oecd.json oecd-arrobj.json --unit
```

#### --meta (-m)

Boolean. Returns a metadata-enriched output (object of objects, instead of array of objects).

```
jsonstat2arrobj oecd.json oecd-objobj.json --meta
```

#### --comma (-k)

Boolean. Represents values as strings instead of numbers with comma as the decimal mark.

```
jsonstat2arrobj canada.json canada-comma.json --comma
```

#### --by (-b)

String. Transposes data by the specified dimension. String must be an existing dimension ID. Otherwise it will be ignored.

```
jsonstat2arrobj oecd.json oecd-transp.json --by area
```

#### --bylabel (-l)

Boolean. Uses labels instead of IDs to identify categories of the transposed dimension, unless --cid has been specified.

```
jsonstat2arrobj oecd.json oecd-transp.json --by area --label
```

#### --prefix (-p)

String. Text to be used as a prefix in the transposed categories. Only valid in combination with --by.

```
jsonstat2arrobj oecd.json oecd-transp.json --by area --prefix "AREA:"
```

#### --drop (-d)

String. Comma-separated dimension IDs to be dropped from the output. Dimensions with more than one category cannot be dropped. Only valid in combination with --by.

```
jsonstat2arrobj canada.json canada-transp.json --by concept --drop country,year
```

## jsonstat2csv

Converts JSON-stat into CSV.

```
jsonstat2csv oecd.json oecd.csv
```

#### --status (-s)

Boolean. Includes status information.

```
jsonstat2csv oecd.json oecd.csv --status
```

#### --vlabel (-v)

String. Specifies the label of the value field. Default is "Value".

```
jsonstat2csv oecd.json oecd.csv --vlabel val
```

#### --slabel (l)

String. Specifies the label of the status field. Default is "Status".

```
jsonstat2csv oecd.json oecd.csv --status --slabel stat
```

#### --na (-n)

String. Not available text. Default is "n/a".

```
jsonstat2csv oecd.json oecd.csv --na ":"
```

#### --column (-c)

String. Column separator. Default is ",".

```
jsonstat2csv oecd.json oecd.ssv --column ";"
```

#### --decimal (-d)

String. Decimal separator. Default is ".", unless column separator is ";" (then default is ",").

```
jsonstat2csv oecd.json oecd.tsv --column "\t" --decimal ","
```

#### --rich (-r)

Boolean. Output is a rich CSV in the [JSON-stat Comma-Separated Values format](https://github.com/badosa/CSV-stat) (CSV-stat, or JSV for short). CSV-stat is CSV plus a metadata header. CSV-stat supports all the JSON-stat dataset core semantics. This means that CSV-stat can be converted back to JSON-stat (using [csv2jsonstat](#csv2jsonstat)) without loss of information (only the *note*, *link*, *child*, *coordinates* and *extension* properties are not currently supported).

When this option is set, **--status**, **--vlabel** and **--slabel** are ignored.

```
jsonstat2csv oecd.json oecd.jsv --rich
```

## jsonstat2object

Converts JSON-stat into an object of arrays in the [Google DataTable format](https://developers.google.com/chart/interactive/docs/reference#dataparam).

```
jsonstat2object oecd.json oecd-object.json
```

#### --status (-s)

Boolean. Includes status information.

```
jsonstat2object oecd.json oecd-object.json --status
```

#### --vlabel (-v)

String. Specifies the label of the value field. Default is "Value".

```
jsonstat2object oecd.json oecd-object.json --vlabel val
```

#### --slabel (-l)

String. Specifies the label of the status field. Default is "Status".

```
jsonstat2object oecd.json oecd-object.json --status --slabel stat
```

#### --fid (-f)

Boolean. Identifies dimensions, value and status by ID instead of label.

```
jsonstat2object oecd.json oecd-object.json --fid
```

#### --cid (-c)

Boolean. Identifies categories by ID instead of label.

```
jsonstat2object oecd.json oecd-object.json --cid
```

## jsonstatslice (aka jsonstat2jsonstat)

Creates JSON-stat from JSON-stat. It can be used to convert old JSON-stat to JSON-stat version 2.0. Because it supports a filter parameter, it can also be used to creates a JSON-stat subset from a JSON-stat dataset. A JSON-stat subset has the same dimensions as the original dataset but with some of them fixed for a certain category.

```
jsonstatslice oecd.json oecd-subset.json -f area=DE,year=2014
```

In the previous example, oecd-subset.json only contains data for Germany in 2014.

#### --filter (-f)

String. Specifies a filter. When no filter is specified, the original JSON-stat will be returned unless it was not JSON-stat 2.0: in such case, it will be updated to version 2.0.

```
jsonstat2jsonstat jsonstat10.json jsonstat20.json
```

A filter is a comma-separated list of selection criteria. Each criterion must follow the pattern *{dimension id}={category id}*. Because dimension ids and category ids are strings that can contain whitespaces, they should be double-quoted.

```
"area"="DE","year"="2014"
```

forces the subset to keep only category "DE" from dimension "area" and category "2014" from dimension "year". Because these ids do not contain whitespaces, double quotes are not strictly necessary.

```
jsonstatslice oecd.json oecd-subset.json -f area=DE,year=2014
```

#### --modify (-m)

Boolean. When this option is not set, the *value* and *status* types of the input dataset are kept. When this option is set, the type of these two properties is transformed taking into account **--ovalue** and **--ostatus**.

#### --ovalue (-o)

Boolean. When **--modify** is set, it includes a "value" property of type object instead of array. When **--modify** is set and **--ovalue** is not, it includes a "value" property of type array.

#### --ostatus (-s)

Boolean. When status information is available and **--modify** is set, includes a "status" property of type object instead of array. When **--modify** is set and **--ostatus** is not, it includes a "value" property of type array.

## sdmx2jsonstat

Creates JSON-stat from SDMX-JSON. The input must be in the SDMX-JSON format. Inputs with more than one dataset are not supported. Intermediate grouping ("time series", "cross-section") is not supported, either. Only SDMX-JSON with a flat list of observations (*dimensionAtObservation=allDimensions*) is supported. This is sometimes referred to as the **SDMX-JSON flat format flavor**.

```
sdmx2jsonstat sdmx.json stat.json
```

### Options

#### --ovalue (-o)

Boolean. Includes a "value" property of type object instead of array.

#### --ostatus (-s)

Boolean. When status information is available, includes a "status" property of type object instead of array.
