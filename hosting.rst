=======
Hosting
=======

Standard web server
========

On a standard hosting (such as an ordinary company web server) you can very simply host your maps. Just load the directory with tiles to your web hosting and the layer is automatically available.

Once uploaded, the produced maps can be opened in any viewer supporting OGC WMTS standard.

For hosting of MBTiles, you can use an open-source TileServer_, that can be used with any standard hosting that supports PHP. Upload the created maps and get dozens of popular web viewers with interactivity, including Google Maps API, Leaflet, OpenLayers, WebGL Earth and MapBox JS. There is a `standalone how-to`_ describing the whole process of hosting with TileServer.

.. _TileServer: https://github.com/klokantech/tileserver-php/
.. _standalone how-to: http://www.maptiler.com/how-to/tileserver-php/

Cloud hosting
=========

The CloudPush command can be used for upload to Amazon S3 or Google Cloud Storage hosting. Examples are shown on the S3. If you need to use Google Cloud Storage, just change the “s3” to “google” or “gs”. Full how-to with visual examples is available as a `how-to article`_.

.. _how-to article: http://www.maptiler.com/how-to/hosting-on-amazon-s3/

Amazon access and the secure key are available via IAM service administration interface.

The credentials for the Google Cloud Storage are under "Enable interoperable access” in the menu of the service.

Cloud Push instance is initialized with the first uploaded map via this command line utility. It automatically creates an empty `index.json`, TileServer in `index.html` and sets WebSite configuration for this bucket.

Upload tiles from an MBTiles file to S3 ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name add filename.mbtiles

List all maps in the cloudpush tile storage ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name list

Delete a map ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name delete filename

Delete whole cloudpush storage ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name destroy

Instead of providing the access credentials in every command these can be set as system environment variables.

Example on Windows OS: ::

 REM for Amazon S3
 set AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 REM or for Google Cloud Storage
 set GOOG_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set GOOG_SECRET_ACCESS_KEY=[THE_SECRET_KEY]

Example on Linux / macOS: ::

 # for Amazon S3
 export AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 export AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]
 # or for Google Cloud Storage
 export GOOG_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 export GOOG_SECRET_ACCESS_KEY=[THE_SECRET_KEY]

and call the utility without these arguments: ::

 cloudpush s3://bucket_name list
 cloudpush s3://bucket_name add filename.mbtiles

It is possible to use further options such as:

  --create-bucket         automatically creates bucket, if not existing
  --no-index-json         not handling metadata in CloudPush instance index.json
  --raw                   same as --no-index-json
  --basename <path>       sets custom basename (default: basename of MBTiles file)
  --private               uploaded objects are private (default: public)

List of available parameters can be displayed by running ./cloudpush without any parameter

Example for using custom basename: ::

 cloudpush --basename myfile s3://bucket_name add filename.mbtiles

uploads tiles with URL format: `myfile/z/x/y.ext`. Custom basename contains directory separators (slash), for example: ::

 cloudpush --basename year/month/myfile s3://bucket_name add filename.mbtiles

result will have URL in format: `year/month/myfile/z/x/y.ext`.


Region-specific hosting can be set up via environment variable AWS_BUCKET_REGION=[value] or with parameter -R [value].

Example for EU (Ireland) region: ::

 cloudpush -R eu-west-1 s3://bucket_name add filename.mbtiles

List of S3 regions is provided by the utility with `--more-help` argument or visible at http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region
