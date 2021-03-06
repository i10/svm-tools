# SVM Tools


## Installation

* Install [libsvm`](http://www.csie.ntu.edu.tw/~cjlin/libsvm/)
* Ensure that `svm-train` and `svm-predict` are in the executable path
* Install node and npm (if you haven't already)
* Do `npm i`


## CLI Usage

Generate a model:
```
svm-gen [train.csv] [model.json]
```

Test a generated model:

```
svm-test [test.csv] [model.json] [report.csv]
```

Train/Test File Format (CSV):

* First element is the zone label
* Other elements are beacon measurements of the form BEACONID:RSSI

Example:

```
1,1-2:-71,1-1:-69,1-3:-86,1-4:-88
```

Model File Format

```json
{
  "simple": {
    "mapping": {
      BEACON_ID: ZONE_ID
    }
  },
  "svm": {
    "mapping": {
      BEACON_ID: SVM_FEATURE_ID
    },
    "zones": [ZONE_ID]
    "model": SVM_MODEL_DEF
  }
}
```

## Programmatic Usage

```
var svmTools = require('svm-tools');

// generate a model
svmTools.train(data, progressCallback).then(function (model) { ... });

// test a model
svmTools.test(data, model).then(function (report) { ... });
```

Testdata Object:

```json
[{
  "zone": ZONE_ID,
  "data": [{
    "date": TIMESTAMP (optional),
    "gps": {
      "east": GPS_EAST (optional),
      "north": GPS_NORTH (optional),
      "accuracy": GPS_ACCURACY (optional)
    },
    "beacons": [{
      "uuid": BEACON_UUID,
      "major": BEACON_MAJOR,
      "minor": BEACON_MINOR,
      "rssi": BEACON_RSSI
    }]
  }]
}]
```

Model Object (see description in section CLI Usage).

## Conversion scripts

* `convert.js` converts a legacy .csv file (from the BeaconScanner, see that repository) into one that can be used by svm-gen
* `convertCSV.js` converts the result of convert.js into a json that can be imported into an inform-location-server (see that repository)
