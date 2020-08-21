# ili2pg_json

Docker-DB
```
docker run --rm --name oereb-db -p 54322:5432 --hostname primary \
-e PG_DATABASE=oereb -e PG_LOCALE=de_CH.UTF-8 -e PG_PRIMARY_PORT=5432 -e PG_MODE=primary \
-e PG_USER=admin -e PG_PASSWORD=admin \
-e PG_PRIMARY_USER=repl -e PG_PRIMARY_PASSWORD=repl \
-e PG_ROOT_PASSWORD=secret \
-e PG_WRITE_USER=gretl -e PG_WRITE_PASSWORD=gretl \
-e PG_READ_USER=ogc_server -e PG_READ_PASSWORD=ogc_server \
sogis/oereb2-db:latest
```

Schema anlegen
```
java -jar /Users/stefan/apps/ili2pg-4.3.1/ili2pg-4.3.1.jar --dbhost localhost --dbport 54322 --dbdatabase oereb --models SO_AfU_Gewaesserschutz_Publikation_20200115 --dbusr admin --dbpwd admin --dbschema afu_gewaesserschutz_pub --coalesceJson --defaultSrsCode 2056 --createGeomIdx --createFk --createFkIdx --createUnique --createEnumTabs --beautifyEnumDispName --createMetaInfo --createNumChecks --nameByTopic --strokeArcs --createEnumTabsWithId	 --schemaimport
```

```
INSERT INTO
    afu_gewaesserschutz_pub.zone_areal 
    (
        typ,
        gesetzeskonform,
        rechtskraftdatum,
        dokumente,
        apolygon 
    )
SELECT
    'S1',
    TRUE,
    '2020-01-01',
    '[{"@type": "SO_AfU_Gewaesserschutz_Publikation_20200115.Gewaesserschutz.Dokument", "URL": "https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/67-Gaensbrunnen/Entscheide/67-8-E.pdf", "Bezeichnung": "RRB 1994/3271"}, {"@type": "SO_AfU_Gewaesserschutz_Publikation_20200115.Gewaesserschutz.Dokument", "URL": "https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/67-Gaensbrunnen/Sonderbauvorschriften/67-8-S.pdf", "Bezeichnung": "Reglement"}, {"@type": "SO_AfU_Gewaesserschutz_Publikation_20200115.Gewaesserschutz.Dokument", "URL": "https://geo.so.ch/docs/ch.so.arp.zonenplaene/Zonenplaene_pdf/67-Gaensbrunnen/Plaene/67-8-P.pdf", "Bezeichnung": "Plan"}]'::jsonb,
    ST_PolygonFromText('POLYGON ((2603470.146 1236902.829, 2603504.029 1236935.157, 2603512.258 1236916.511, 2603479.227 1236891.326, 2603470.146 1236902.829))', 2056)
;
```

Daten exportieren:
```
java -jar /Users/stefan/apps/ili2pg-4.3.1/ili2pg-4.3.1.jar --dbhost localhost --dbport 54322 --dbdatabase oereb --models SO_AfU_Gewaesserschutz_Publikation_20200115 --dbusr admin --dbpwd admin --dbschema afu_gewaesserschutz_pub --export fubar.xtf
```

```
java -jar /Users/stefan/apps/ilivalidator-1.11.6/ilivalidator-1.11.6.jar fubar.xtf
```


## Test wegen --createNumChecks	

java -jar /Users/stefan/apps/ili2pg-4.3.1/ili2pg-4.3.1.jar --dbhost localhost --dbport 54322 --dbdatabase oereb --models SO_AFU_Igel_Publikation_20200429 --dbusr admin --dbpwd admin --dbschema afu_igel_pub --disableValidation  --strokeArcs --createNumChecks --schemaimport 
