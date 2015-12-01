# Imports API
Create and run imports into your nation with this API.

## Create/Run Endpoint
Use this endpoint to import into your nation. This will create and enqueue the import to run. Each import should be 50mb or less - please chunk large files and use multiple calls.

```
POST /api/v1/imports
```

### Parameters
- `type` - either "[people](http://nationbuilder.com/how_do_i_import_my_custom_voter_file)" or "[voting_history](http://nationbuilder.com/how_do_i_import_custom_voter_history)", depending on what fields you wish to import with the file. Note that these import types must be imported separately, and voter history should be imported only after voter file information has processed into the nation you are updating.
- `file` - a RFC 4648 base 64 encoded version of the contents of the UTF-8 file you wish to import, using the alphabet defined as "URL and Filename safe Base64 Alphabet" by the standard.
- `is_overwritable` - Set this flag to `true` to overwrite non-empty fields. Default is `false`.

## Show Endpoint
Show the progress status of an import with this endpoint

```
GET /api/v1/imports/:id
```

### Parameters
- `id` - The import ID returned by the import create endpoint.

### Example output

```json
{
  "import": {
    "id": 5,
    "type": "people",
    "status": {
      "name": "queued"
    }
  }
}
```

## Result Endpoint
Returns the detailed import result of a finished imported.

```
GET /api/v1/imports/:id/result
```

### Parameters
- `id` - The import ID returned by the import create endpoint.

### Example Output

```json
{
  "result": {
    "rows_updated": 44,
    "rows_succeeded": 267,
    "rows_failed": 54,
    "failure_csv": "[Base 64 encoded error csv file]"
  }
}
```

## Full Example
In order to import the following file:

```
id,first_name,last_name
1,Byron,Anderson
```

Attributes available are for the person directly, relation fields such as those for addresses and voting histories need to be prefixed by their relations name, for example: `mailing_address.address1` and `voting_history.vh00p1`. Custom fields can be imported as well using the `custom_field` prefix and the custom field slug (ex. `custom_fields.university_name`)

You need to issue the following request:

```
POST https://foobar.nationbuilder.com/api/v1/imports
```

```json
{
  "import": {
    "type": "people",
    "file": "aWQsZmlyc3RfbmFtZSxsYXN0X25hbWUNCjEsQnlyb24sQW5kZXJzb24"
  }
}
```

To get the value to put in the "file" field, you can use a library such as [this for Ruby](http://ruby-doc.org/stdlib-2.0/libdoc/base64/rdoc/Base64.html) or [this for Java](http://download.java.net/jdk8/docs/api/java/util/Base64.html) On a most Unix machines, you can use `base64 -i my_spreadsheet.csv` to base64 encode a CSV file.

Success on this endpoint will return a 200 response and a JSON response including a new "id" field and excluding the encoded file contents:

```json
{
  "import": {
    "id": 5,
    "type": "people",
    "status": {
      "name": "queued"
    }
  }
}
```

This should also queue the import to run, and you can then check for updates on the status of the import with the show endpoint:

```
GET https://foobar.nationbuilder.com/api/v1/imports/5
```

```json
{
  "import": {
    "id": 5,
    "type": "people",
    "status": {
      "name": "working",
      "processed": 100,
      "total": 198
    }
  }
}
```

Possible statuses you can receive are queued, working, and finished, and more information may be available for you to understand the status of your import.

Once the status has indicated that the import is completed, import results endpoint can be used to retreive import result statistics:

```
GET /api/v1/imports/5/result
```

```json
{
  "result": {
    "rows_updated": 44,
    "rows_succeeded": 267,
    "rows_failed": 54,
    "failure_csv": "[Base 64 encoded error csv file]"
  }
}
```

The `failure_csv` is especially important because it is a Base64 encoded csv file with imported rows removed, and leaving only failed rows along with reason on why this row failed to import.
