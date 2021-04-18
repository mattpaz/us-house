![Civil Services Logo](https://raw.githubusercontent.com/CivilServiceUSA/api/master/docs/img/logo.png "Civil Services Logo")

__Civil Services__ is a collection of tools that make it possible for citizens to be a part of what is happening in their Local, State & Federal Governments.


ATTENTION
===

The GeoJSON files in this directory are automatically generated.  Do not edit these files directly.

Developers:
---

Here is the process I normally use for updating the District Maps, we only need to do this every two years, so the process changes a bit each year, but this is what we did for 117th Congress:

1. Search [ArcGIS](https://arcgis.com) for latest Congressional District Map, e.g. a search for `USA 117th Congressional Districts`
2. Once you find a map, you can view the details for that map and you will end up on a URL that starts with something like `https://services.arcgis.com/`
3. Once you have that URL, you can hop in terminal and run an API call on that URL to download geojson data like this:

    ```bash
    curl -o congress.geojson "https://services.arcgis.com/P3ePLMYs2RVChkJx/ArcGIS/rest/services/USA_117th_Congressional_Districts/FeatureServer/0/query?where=objectid+%3D+objectid&outfields=*&f=geojson"
    ```

4. Once you have a file downloaded, we can reduce the file size a bit by removing a small amount of precision. For that, I use a tool called `geojson-precision`

    ```bash
    npm install -g geojson-precision
    geojson-precision -p 4 congress.geojson us-house-min.geojson
    ```

5. Then we can verify that all the shapes are closed properly by running it through a great tool from MapBox that fixes any issues it finds with the shapes

    ```bash
    npm install -g geojson-rewind
    geojson-rewind us-house-min.geojson > us-house.geojson
    ```

6. Then I just copy that `us-house.geojson` into the `./source/us-house.geojson` and I can run `npm run build-geojson`
