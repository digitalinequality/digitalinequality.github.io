



### Folder Purposes

  * ImbaDivn: store Imbalance Index data for each country in a file, name: {alph3}.json. World.json is world level data.
  * Shape: store GeoShape data for each country and their level 1 division. Name: {alph3}.geojson. World.geojson is the world level data. 
  * ViewConf: for each country, there is {alph3}.json file that stores vissualization configuration information used by OpenLayers. **Currently, not used**
  * ImbaGrid: store grid level visualization data.
    - Config: the grid visualization configure information for each country. Name: {alph3}.json.
    - Shape: grid shape files. The subfolders contain the grid shapes for each country. Folder name: {alph3}. The name of the grid shape file in a folder: {row}x{col}.geojson.
    - ImbaIndex: the Imbalance Index data files, one folder for each country, and the folder name is: {alph3}. The data files in a folder are: {row}x{col}.json.

### File Format

There are 3 file formats:

  * Shape data in GeoJSON format
  * Imbalance Index data in JSON format
  * Config information in JSON format

The GeoJSON format for world wide countries (World.geojson):

  * Feature: id => alpha3
  * name: capital title
  * geometry: only used by OpenLayers API

The GeoJSON format for level 1 divisions of a country:

  * id: division title
  * name: division title
  * geometry: only used by OpenLayers API

The GeoJSON format for grids of a country:

  * id: the grid index as int, r*nCols + c
  * geometry: only used by OpenLayers API

The vissualization configure file for each country, used by OpenLayers. The file name pattern is {alpha3}.json in the ViewConf folder.

```json
{
    "View": {
        "center": [0, 0],
        "zoom": 0,
    }
}
```

**New ImbaIndex Format**
In this format, it is assumed that the population does not change. We only use a p field to store population data (as int). Monthly base station statistics are stored in a list, instead of a dictionary, in order to avoid storing keys. The order of the data in the array is determined by MonthList (Python). You can use the same index to get the corresponding month. In JavaScript, VisConfig stores StartMonth, EndMonth and MonthStep. By Utiltiy.ListYearMonthWithStep, we can get the corresponding month list. Then the month data will be accessed by indices, instead of month name.

Imbalance Index JSON format in World.json:

```json
{
    "{alph3}": {
        "p": population, int,
        "b": number of BS, int[],
    },

    "{alph3}": {....}
}
```

Division Imbalance Index JSON format of a country:

  * File name is {alpha3}.json, recognized by programs
  * Inner format as follows

```json
{
    "{Division ID}": {
        "p": population, int,
        "b": number of BS, int[],
    },

    "{Division ID}": {....}
}
```

Grid Level Imbalance Index JSON format of a country:

  * Contained in a folder whose name is {alpha3}
  * File name is {r}x{c}.json, which means the grids with r rows and c columns, recognized by programs
  * Inner format as follows
  * Grid id is an int number, whose value is r*nCols + c

```json
{
    "{Grid id}": {
        "p": population, int,
        "b": number of BS, int[],
    },

    "{Grid id}": {....}
}
```

The Config json format: a dictionary:

```json
{
    "Setup": ["{r}x{c}", "{r}x{c}", "{r}x{c}"]
}
```


### Out-dated format

Imbalance Index JSON format in World.json:

```json
{
    "{alph3}": {
        "YYYY-MM": {
            "p": population, int,
            "b": number of BS, int,
            "i": imbalance index, float
        },

        "YYYY-MM": {
            ....
        }
    },

    "{alph3}": {....}
}
```

Division Imbalance Index JSON format of a country:

  * File name is {alpha3}.json, recognized by programs
  * Inner format as follows

```json
{
    "{Division Title}": {
        "YYYY-MM": {
            "p": population, int,
            "b": number of BS, int,
            "i": imbalance index, float
        },

        "YYYY-MM": {
            ....
        }
    },

    "{Division Title}": {....}
}
```

Grid Level Imbalance Index JSON format of a country:

  * Contained in a folder whose name is {alpha3}
  * File name is {r}x{c}.json, which means the grids with r rows and c columns, recognized by programs
  * Inner format as follows
  * Grid id is an int number, whose value is r*nCols + c

```json
{
    "{Grid id}": {
        "YYYY-MM": {
            "p": population, int,
            "b": number of BS, int,
            "i": imbalance index, float
        },

        "YYYY-MM": {
            ....
        }
    },

    "{Grid id}": {....}
}
```

### Future Extension

If the level 2 division visualization is considered, please add new folders, for example:

  * Level2Divn
     - Shape > {alpha}.geojson
     - ImbaIndex > {alpha}.json