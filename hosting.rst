=======
Hosting
=======

Standard web server
========

On a standard hosting (such as an ordinary company web server) you can very simply host your maps. Just load the directory with tiles to your web hosting and the layer is automatically available.

Once uploaded, the produced maps can be opened in any viewer supporting OGC WMTS standard.

For hosting of MBTiles, you can use an open-souce TileServer (available at: https://github.com/klokantech/tileserver-php/), that can be used with any standard hosting that supports PHP. Upload the created maps and get dozens of popular web viewers with interactivity, including Google Maps API, Leaflet, OpenLayers, WebGL Earth and MapBox JS.

Cloud hosting
=========

CloudPush command can be used for upload to Amazon S3 or Google Cloud Storage hosting. Examples are shown on the S3. If you need to use Google Cloud Storage, just change the “s3” to “google” or “gs”.

Amazon access and secure key is available via IAM service administration interface.

The credentials for the Google Cloud Storage are under "Enable interoperable access” in the menu of the service.

Cloud Push instance is automatically initialized with the first map uploaded via this command utility,
creating empty `index.json`, TileServer in `index.html` and setting WebSite configuration for this bucket.

Upload tiles from an MBTiles file to S3 ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name add filename.mbtiles

List all maps in the cloudpush tile storage ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name list
 
Delete a map ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name delete filename
 
Delete whole cloudpush storage ::

 cloudpush --acces_key ACCESS_KEY --secret_key SECRET_KEY s3://bucket_name destroy
 
Instead of providing the access credentials in every command these can be set as system environment variables: ::

 set AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]

for Amazon S3, or

 set GOOG_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set GOOG_SECRET_ACCESS_KEY=[THE_SECRET_KEY]

for Google Cloud Storage,

and call the utility without these arguments: ::

 cloudpush s3://bucket_name list
 cloudpush s3://bucket_name add filename.mbtiles
 
Region-specific hosting can be set up via environment variable AWS_BUCKET_REGION=[value] or with parameter -R [value].

Example for EU (Ireland) region: ::

 ./cloudpush -R eu-west-1 s3://bucket_name add filename.mbtiles
 
List of S3 regions is provided by the utility with `--more-help` argument or visible at http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region

CloudPush utlity contains also these options: ::

  --create-bucket         automatically create bucket, if not existing
  --no-index-json         do not handle metadata in CloudPush instance index.json
  --raw                   same as --no-index-json
  --basename <path>       set custom basename (default: basename of MBTiles file)
  --private               uploaded objects are private (default: public)

Example for using custom basename: ::

 ./cloudpush --basename myfile s3://bucket_name add filename.mbtiles

uploads tiles with URL format: `myfile/z/x/y.ext`. Custom basename can contains directory separators (slash), for example: ::

 ./cloudpush --basename year/month/myfile s3://bucket_name add filename.mbtiles

resulting tiles have URL like `year/month/myfile/z/x/y.ext`.

For more information run the utility without parameters and it will print all available parameters.



