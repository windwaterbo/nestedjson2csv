# Convert nested json to csv

Sources from the [json2csv](https://www.npmjs.org/package/json2csv) to improve. Converts nested json into csv with column titles and proper line endings.

## Installation

```bash
$ npm install nestedjson2csv
```

## Features

- Uses proper line endings on various operating systems
- Handles double quotes
- Allows custom column selection
- Reads column selection from file
- Pretty writing to stdout
- Supports optional custom delimiters
- Not create CSV column title by passing hasCSVColumnTitle: false, into params.
- If field is not exist in object then the field value in CSV will be empty.
- **Supported Nested JSON**

## API

```javascript
var nestedjson2csv = require('nestedjson2csv');

nestedjson2csv({data: someJSONData, fields: ['field1', 'field2', 'field3'],fieldNames:['field1Name','field2Name','field3Name']}, function(err, csv) {
  if (err) console.log(err);
  console.log(csv);
});
```
### fields
collection name of object
### fieldsNames
CSV column title

## Example 1

```javascript
var nestedjson2csv = require('nestedjson2csv');

var json = [
{
  "logger" : {
        "datetime" : "2014-11-26 13:25:33",
        "who" : "macy",
        "hostname" : "127.0.0.1",
        "cat" : {
            "name" : "tom",
            "age" : "1",
            "gender" : "M"
        }
    },
    "logger" : {
        "datetime" : "2014-11-26 14:20:11",
        "who" : "ka",
        "hostname" : "127.0.0.1",
        "cat" : {
            "name" : "didi",
            "age" : "0.8",
            "gender" : "M"
        }
    }
}];

nestedjson2csv({data: json,  fields: ['logger.datetime','logger.who', 'logger.cat.name','logger.cat.age','logger.cat.gender']}, function(err, csv) {
  if (err) console.log(err);
  console.log(csv);
});
```

The content of the "file.csv" should be

```
logger.datetime, logger.who, logger.cat.name, logger.cat.age, logger.cat.gender
2014-11-26 13:25:33, macy, tom, 1, M
2014-11-26 14:20:11, ka, didi, 0.8, M
```

## Example 2

You can choose custom column names for the exported file.

```javascript
nestedjson2csv({
data: json, 
fields: ['logger.datetime','logger.who', 'logger.cat.name','logger.cat.age','logger.cat.gender'],
fieldNames: ['datetime','host','pet name','pet age','pet gender']
}, function(err, csv) {
  if (err) console.log(err);
  fs.writeFile('file.csv', csv, function(err) {
    if (err) throw err;
    console.log('file saved');
  });
});
```

Results in

```
datetime, host, pet name, pet age, pet gender
2014-11-26 13:25:33, macy, tom, 1, M
2014-11-26 14:20:11, ka, didi, 0.8, M
```

## Example 3

Use a custom delimiter to create tsv files. Add it as the value of the del property on the parameters:

```javascript
nestedjson2csv({data: json, fields: ['logger.datetime','logger.who', 'logger.cat.name','logger.cat.age','logger.cat.gender'],
fieldNames: ['datetime','host','pet name','pet age','pet gender'],
del: '\t'}, function(err, tsv) {
  if (err) console.log(err);
  console.log(tsv);
});
```

Will output:

```
datetime host pet name pet age pet gender
2014-11-26 13:25:33 macy tom 1 M
2014-11-26 14:20:11 ka didi 0.8 M
```

If no delimiter is specified, the default `,` is used

## update log

- 2015.07.09 : fix text containing literal \r\n strings @v2.0.1


