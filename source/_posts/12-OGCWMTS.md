---
title: '12. OGCWMTS'
author: cyclezone
img: /medias/banner/3.jpeg
top: false
cover: false
coverImg: /medias/banner/1.jpeg
password: 8d969eef6
toc: false
mathjax: false
summary: ' OGCWMTS'
categories: cycle zone
tags:
  - cycle
date: 2021-12-19 20:52:48
---
# 12. OGCWMTS


1. OGC WMTS (Web MAP Tile Service) is a standard for web map; 
   but for different map vendor,they may use it vary from each other,especially for the non-mainstream vendor;
2. for mainstream vendor,they list the readME document in the easy access website usually; 
   Follow the the readMe document,we can use the tile service easily with not that much time and effort;

   but for no-mainstream vendor,How can we do?

3.for example ,we get the server instance url:
  https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/img08/wmts/2CA54B6D242305ABF3822EC38D121CD9/WMTSCapabilities.xml 

  when we access the url above in chrome explore,we get some information about the server capabilities information,like this:


    <ows:ServiceIdentification>
    ...
    </ows:ServiceIdentification>
    <ows:ServiceProvider>
    ...
    </ows:ServiceProvider>
    <ows:OperationsMetadata>
    ...
    </ows:OperationsMetadata>
    <Contents>
    <Layer>
    <Format>tiles</Format>
    <TileMatrixSetLink>
    <TileMatrixSet>default028mm</TileMatrixSet>
    </TileMatrixSetLink>
    <TileMatrixSetLink>
    <TileMatrixSet>nativeTileMatrixSet</TileMatrixSet>
    </TileMatrixSetLink>
    </Layer>
    <TileMatrixSet>
    ...
    </TileMatrixSet>
    <TileMatrixSet>
    ...
    </TileMatrixSet>
    </Contents>
    </Capabilities>


  the server url parse ENGINE parse the the above url  to url:

    https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/img08/wmts?service=WMTS&version=1.0.0&request=GetCapabilities

    OR

    https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/wmts?layer=img08&service=WMTS&version=1.0.0&request=GetCapabilities

    so we get the capabilities information;

    then we want to get the tile image buffer stream, HOW?

4. for Tianditu WMTS,we get the instruction from '  http://lbs.tianditu.gov.cn/server/MapService.html ' ,


 the example url is :

        http://t0.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={x}&TILECOL={y}&tk=yourtoken
     
  According the url above,we make the target url:

        https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/img08/wmts/?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img08&STYLE=default&FORMAT=tiles&TILEMATRIXSET=nativeTileMatrixSet&TILEMATRIX=1&TILEROW=0&TILECOL=1

        or

        https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/img08/wmts/2CA54B6D242305ABF3822EC38D121CD9?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img08&STYLE=default&FORMAT=tiles&TILEMATRIXSET=nativeTileMatrixSet&TILEMATRIX=1&TILEROW=0&TILECOL=1

  access them in chrome explore, Both ERROR;

 5. add the token? no token;
   access the original url in Arcgis pro 2.5;we get the tile stream and display normally;
   WHY? perhaps the wrong target url;
   How to get the right url;
6. use fiddler  (https://www.telerik.com/fiddler);
   run fiddler,at the same time,refresh the target layer in Arcgis pro 2.5;
   the fiddler get some useful information,in fiddler,pick the target line;
   get :

        data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/wmts/2CA54B6D242305ABF3822EC38D121CD9?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img08&STYLE=default&FORMAT=tiles&TILEMATRIXSET=nativeTileMatrixSet&TILEMATRIX=4&TILEROW=2&TILECOL=12

   append host site https://fxpc.mem.gov.cn/ ;

        https://fxpc.mem.gov.cn/data_preparation/a12eadf6-1a57-43fe-9054-2e22277bd553/8c6a43c6-fa48-4f2b-9296-f2c02c4474a7/wmts/2CA54B6D242305ABF3822EC38D121CD9?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img08&STYLE=default&FORMAT=tiles&TILEMATRIXSET=nativeTileMatrixSet&TILEMATRIX=4&TILEROW=2&TILECOL=12

   access the above url in chrome explore,OK, we get it,we get the tile image stream;
   


