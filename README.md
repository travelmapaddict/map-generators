# Tiles

## Goals

* Configure which OSM features will be present in the MVT tiles
* Generate the MVT tiles
* Serve the MVT tiles locally
* Push the generated MVT tiles out to Google cloud buckets
* Push the config data out to external Firestore instances

## Quickstart

Ensure all the executables from the [dependencies](#dependencies) section are accessible in your `PATH`. Then:

```sh
jake        # will take a while...
jake serve  # open http://localhost:8080/ in a browser
jake upload # will prompt you for a google cloud storage path to upload to
```

## Dependencies

* [jake](https://www.npmjs.com/package/jake)
* [yaml2json](https://github.com/bronze1man/yaml2json)
* [get-overpass](https://www.npmjs.com/package/get-overpass)
* [mapshapper](https://www.npmjs.com/package/mapshaper)
* [geojson-cli-bbox](https://www.npmjs.com/package/geojson-cli-bbox)
* [jq](https://stedolan.github.io/jq/)
* [tippecanoe](https://github.com/mapbox/tippecanoe)
* [tileserver-gl-light](https://www.npmjs.com/package/tileserver-gl-light) - for serving locally
* [gsutil](https://cloud.google.com/storage/docs/gsutil) - for uploading

## Cleaning up

Note that `make clean` cleans out everything except the raw downloaded data. To delete the raw downloads as well, use `make fullclean`.

## Setting up a Google Cloud Storage Bucket for uploads

These settings will allow your GC storage bucket to serve uploaded tiles via a public url such that the the mapbox-gl sdk can consume them. Note that these settings are permissive - they allow any website or unauthenticated user read access to the whole bucket. Use with care.

1. Create the cloud bucket

    ```shell
    gsutil mb gs://my-new-bucket-with-a-unique-name
    ```

1. Give everyone read access to the bucket

    ```shell
    gsutil iam ch allUsers:objectViewer gs://my-new-bucket-with-a-unique-name
    ```

1. Open up CORS all the way on the bucket

    ```shell
    echo '[{"origin": ["*"],"method": ["*"]}' > cors.json
    gsutil cors set cors.json gs://my-new-bucket-with-a-unique-name
    rm cors.json
    ```

## Changelog

### 0.2

 * switched to OSM (Open Street Map) as data source
 * Makefile to manage build process

### 0.1

 * WOF (Who's on First) as data source
 * shell scripts to manage build process
