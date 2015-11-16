=======
Hosting
=========

Standard web server
========

On a standard hosting (such as an ordinary company web server) you can very simply host your maps. Just load the directory with tiles to your web hosting and the layer is automatically available.

Once uploaded, the produced maps can be opened in any viewer supporting OGC WMTS standard.

For hosting of MBTiles, you can use an open-souce TileServer (available at: https://github.com/klokantech/tileserver-php/), that can be used with any standard hosting that supports PHP. Upload the created maps and get dozens of popular web viewers with interactivity, including Google Maps API, Leaflet, OpenLayers, WebGL Earth and MapBox JS.

Cloud hosting
=========

Cloudpush command can be used for upload to Amazon S3 or Google Cloud Storage hosting. Examples are shown on the S3. If you need to use Google Cloud Storage, just change the “s3” to “google”.

Amazon access and secure key is available via IAM service administration interface.

The credentials for the Google Cloud Storage are under "Enable interoperable access” in the menu of the service.

First, initialize cloudpush tile storage on S3 (it creates index.json) ::

 ￼cloudpush init s3:/bucket_name ACCESS_KEY SECRET_KEY
 
Or the same on Google Cloud Storage ::

 ￼cloudpush init google:/bucket_name ACCESS_KEY SECRET_KEY
 
Upload tiles from an MBTiles file to S3 ::

 cloudpush add s3:/bucket_name ACCESS_KEY SECRET_KEY filename.mbtiles

List all maps in the cloudpush tile storage ::

 cloudpush info s3:/bucket_name ACCESS_KEY SECRET_KEY
 
Delete a map ::

 ￼cloudpush remove s3:/bucket_name ACCESS_KEY SECRET_KEY filename
 
Delete whole cloudpush storage ::

 cloudpush destroy s3:/bucket_name ACCESS_KEY SECRET_KEY
 
Instead of providing the access credentials in every command these can be set as system environment variables: ::

 set AWS_ACCESS_KEY_ID=[THE_ACCESS_KEY]
 set AWS_SECRET_ACCESS_KEY=[THE_SECRET_KEY]

and call the utility with slashes instead of the keys: ::

 cloudpush init s3://bucket_name - -
 cloudpush add s3://bucket_name - - filename.mbtiles
 
Region-specific hosting can be set up via environment variable AWS_BUCKET_REGION=[value] or with parameter -R [value].

Example for EU (Ireland) region: ::

 ./cloudpush -R eu-west-1 add s3://bucket_name - - filename.mbtiles
 
List of S3 regions is provided by the utility or visible at `http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region <http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region>`_

For more information run the utility without parameters and it will print all available parameters.



