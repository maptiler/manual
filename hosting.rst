=======
Hosting
=======

Standard web server
========

On a standard hosting (such as an ordinary company web server) you can very simply host your maps. Just load the directory with tiles to your web hosting and the layer is automatically available.

Once uploaded, the produced maps can be opened in any viewer supporting OGC WMTS standard.

For hosting of MBTiles, you can use an open-source TileServer_, that can be used with any standard hosting that supports PHP. Upload the created maps and get dozens of popular web viewers with interactivity, including Google Maps API, Leaflet, OpenLayers, WebGL Earth and MapBox JS. There is a `standalone how-to`_ describing the whole process of hosting with TileServer.

.. _TileServer: https://github.com/klokantech/tileserver-php/
.. _standalone how-to: https://www.maptiler.com/how-to/tileserver-php/

Cloud hosting
=========

The CloudPush command can be used for upload to Amazon S3, Google Cloud Storage or Microsoft Azure Blob hosting. Examples are shown on the S3. If you need to use Google Cloud Storage or Microsoft Azure Blob, just change the “s3” to “gs” or “az”, respectively. Full how-to with visual examples is available as a `how-to article`_.

.. _how-to article: https://www.maptiler.com/how-to/hosting-on-amazon-s3/

Cloud Push instance is initialized with the first uploaded map via this command line utility. It automatically creates an empty `index.json`, TileServer in `index.html` and sets WebSite configuration for this bucket. To get the required credentials, see the section Access credentials below.

Upload tiles from an MBTiles file to S3 ::

 cloudpush --access_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name add filename.mbtiles

List all maps in the cloudpush tile storage ::

 cloudpush --access_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name list

Delete a map ::

 cloudpush --access_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name delete filename

Delete whole cloudpush storage ::

 cloudpush --access_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name destroy


Access credentials
---------

The Amazon access and the secure key are available via `IAM service administration`_ interface.
The credentials for the Google Cloud Storage are under "`Enable interoperable access`_" in the menu of the service. The Azure Blob Storage requires the **Storage account name** as Access Key and the **Key** from the Microsoft `Azure Portal`_ - Storage Accounts - Access keys.

.. _IAM service administration: https://portal.aws.amazon.com/gp/aws/developer/account/index.html?action=access-key
.. _Enable interoperable access: https://storage.cloud.google.com/m
.. _Azure Portal: https://portal.azure.com/


Instead of providing the access credentials in every command these can be set as system environment variables.

Example on Windows OS: ::

 REM for Amazon S3
 set AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 REM or for Google Cloud Storage
 set GOOG_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set GOOG_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 REM or for Microsoft Azure Storage
 set AZURE_STORAGE_ACCOUNT=[ACCOUNT NAME]
 set AZURE_STORAGE_ACCESS_KEY=[KEY]

Example on Linux / macOS: ::

 # for Amazon S3
 export AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 export AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 # or for Google Cloud Storage
 export GOOG_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 export GOOG_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 # or for Microsoft Azure Storage
 export AZURE_STORAGE_ACCOUNT=[ACCOUNT NAME]
 export AZURE_STORAGE_ACCESS_KEY=[KEY]

and call the utility without these arguments: ::

 cloudpush s3://bucket_name list
 cloudpush s3://bucket_name add filename.mbtiles
 cloudpush gs://bucket_name list
 cloudpush az://bucket_name list


Advanced options
---------

It is possible to use further options such as:

  --create-bucket         automatically creates bucket, if not existing
  --no-index-json         not handling metadata in CloudPush instance index.json
  --raw                   same as --no-index-json
  --basename <path>       sets custom basename (default: basename of MBTiles file)
  --private               uploaded objects are private (default: public)
  --emulator              Enable Azure Storage Emulator API

List of available parameters can be displayed by running ./cloudpush without any parameter

Example for using custom basename: ::

 cloudpush --basename myfile s3://bucket_name add filename.mbtiles

uploads tiles with URL format: `myfile/z/x/y.ext`. Custom basename contains directory separators (slash), for example: ::

 cloudpush --basename year/month/myfile s3://bucket_name add filename.mbtiles

result will have URL in format: `year/month/myfile/z/x/y.ext`.


Region-specific hosting can be set up via environment variable AWS_BUCKET_REGION=[value] or with parameter -R [value].

Example for EU (Ireland) region: ::

 cloudpush -R eu-west-1 s3://bucket_name add filename.mbtiles

The list of S3 regions is provided by the utility with `--more-help` argument or visible at https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region


To enable uploading tiles into `Azure Storage Emulator`_, you need to pass the parameter `--emulator` for each command: ::

.. _Azure Storage Emulator: https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator

 cloudpush --emulator az://bucket_name add filename.mbtiles

The Azure Storage uses the API of `the version 2015-02-21`_.

.. _the version 2015-02-21: https://docs.microsoft.com/en-us/rest/api/storageservices/version-2015-02-21

