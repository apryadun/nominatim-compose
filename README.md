# Nominatim docker-compose

This docker-compose project is based on [Nominatim Docker](https://github.com/mediagis/nominatim-docker) using version [3.3](https://github.com/mediagis/nominatim-docker/tree/master/3.3). 
# Steps to launch
1. Download pdf file and place it in ```./data/pbf``` directory
> If you need to import several pbf files first merge all the excerpts and then import the result in one go. Refer [Osmium Tools](https://osmcode.org/osmium-tool/) or [Osmosis](https://wiki.openstreetmap.org/wiki/Osmosis) for help.
2. Rename ```.env-example``` file to ```.env``` and edit ```OSMFILE``` path pointing to the file from previuos step. Alternately change and number of working threads, e.g.:
```
OSMFILE=/data/pbf/monaco-latest.osm.pbf
THREADS=4
```
3. Launch the stack:
```
docker-compose up -d
```
4. Wait till the process of import is done. To check logs:
```
docker-compose logs -f
```
To check the service is up navigate to ```http://<node-ip>:8080```. During the import process apache will show 500 error page, on finish you'll see Nominatim page.
> The process of import can take hours or even days. On successive finish the directory ```./data/postgres/state``` will contain empty file ```DB_READY``` used as a flag. To reimport the database or to start the process from the beginning remove this file. The directory ```./data/postgress/db/main``` is actual postgress database store.
5. To change tile server being used edit the file ```./data/rest/conf/settings.php``` to set ```CONST_Map_Tile_URL``` definition and restart the REST service container:
```
docker-compose restart rest
```