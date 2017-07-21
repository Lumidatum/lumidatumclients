# Lumidatum Python Client Documentation:

### Quick Start:

To get the client in Python simply run:

`pip install lumidatumclient`

<i>Latest version: 0.11.0</i>

<i>*Currently Python versions 2.6, 2.7, 3.4, 3.5 are supported.</i>

#### Client:

`lumidatumclient.LumidatumClient(auth_token, model_id=None, host_address='https://www.lumidatum.com')`

`model_id` may be a string or int, and if not provided at client instantiation, must be provided in all method calls.

##### Methods:

`lumidatumclient.LumidatumClient.getRecommendations(parameters, model_id=None)`

Input parameters is a single dictionary. The key names and value types vary by model implementation.
If `model_id` is provided both at client instantiation and in a method call, the value provided at the method call is used to call the API.

`lumidatumclient.LumidatumClient.sendUserData(data_string=None, file_path=None, model_id=None)`

Writes/updates user profile data.

Requires either `data_string` or `file_path` to be provided, and `model_id` if `model_id` was not specified at client instantiation.

Returns a requests.models.Response object.

The data string parameter (`data_string`) should in the format below.

<i>(format type 1)</i>
```
{JSON notation}
{JSON notation}
...
{JSON notation}
```

where each line is a string representation of an object in JSON notation, or a single string representation of an array of objects in JSON notation, such as below.

<i>(format type 2)</i>
```
[
  {JSON notation},
  {JSON notation},
  ...
  {JSON notation}
]
```

Alternatively, the local path to a file to be uploaded can be provided, where the contents of the file should be (format type 1). In the case where both `data_string` and `file_path` parameters are provided, the method will prefer `data_string`. If `model_id` is specified in the method call, it will be preferred over the `model_id` provided at client instantiation.

`lumidatumclient.LumidatumClient.sendItemData(data_string=None, file_path=None, model_id=None)`

Writes/updates item profile data.

`lumidatumclient.LumidatumClient.sendTransactionData(data_string=None, file_path=None, model_id=None)`

Writes/updates transaction data.

`lumidatumclient.LumidatumClient.getLTVReportDates(zipped=False, latest=False)`

Gets a list of timestamps indicating when LTV reports for a given model were generated.
If `zipped` is set to `True`, only (a) timestamp(s) for zipped reports will be returned, otherwise all timestamps are for non-zipped reports available for download.
If `latest` is set to `True`, a single timestamp string will be returned. 

`lumidatumclient.LumidatumClient.getLatestLTVReport(download_file_path, sub_type=None, zipped=True, stream_download=True)`

Gets the latest user lifetime value report.

`download_file_path` should be a string and will also be used to set the file name of the download.

`sub_type` should be a string. An optional parameter specifying custom report types within a given report type (LTV/SEG) to download.

`zipped` should be a boolean value indicating whether or not to download a zipped version of the requested report.

`stream_download` should be a boolean value indicating whether or not to read the entire requested report into memory upon download while writing to the specified file path.

`lumidatumclient.LumidatumClient.getSegmentationReportDates(zipped=False, latest=False)`

Gets a list of timestamps indicating when segmentation reports for a given model were generated.
If `zipped` is set to `True`, only (a) timestamp(s) for zipped reports will be returned, otherwise all timestamps are for non-zipped reports available for download.
If `latest` is set to `True`, a single timestamp string will be returned. 

`lumidatumclient.LumidatumClient.getLatestSegmentationReport(download_file_path, sub_type=None, zipped=True, stream_download=True)`

Gets the latest user segmentation report.

##### Examples using the Client:<a name="examples"></a>

###### Getting recommendations 
```
from lumidatumclient import LumidatumClient

auth_token = '<your auth token>'
model_id = <your model id>

client = LumidatumClient(auth_token, model_id)

params = {<your model params>}

recs = client.getRecommendations(params)
```

###### Updating user data
```
user_data_string = '{"id": 123 ... "field": "values, text, whatever\'s fine"}\n{"id": 345, ... "field": "values \\n with new lines"}'

user_data_update_response = client.sendUserData(user_data_string)
```

###### Updating transaction data
```
# Sending data in a request body
transaction_data_string = '...'

transaction_data_update_response = client.sendTransactionData(transaction_data_string)
```

```
# Uploading a file
my_local_file_path = '<my local file path>'

file_upload_response = client.sendTransactionData(file_path=my_local_file_path)
```
