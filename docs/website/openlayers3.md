---
layout: docs
category: website
title: OpenLayers 3
description: OpenLayers 3
keywords: Openlayers3
---

This is an example how to use for OpenLayers 3 using the [ol-mapbox-style](https://github.com/boundlessgeo/ol-mapbox-style) extension together with OpenMapTiles. Check out the code [here](https://github.com/openmaptiles/www.openmaptiles.org/tree/master/maps).

<iframe src="/maps/ol3.html" frameborder="0" scrolling="0" width="100%" height="540px" style="margin-bottom:25px;"></iframe>

### Convert Mapbox GL style to OpenLayers style functions

The [ol-mapbox-style](https://github.com/boundlessgeo/ol-mapbox-style) converts the original Mapbox GL style specification into OpenLayers style functions. Thanks to this extension you are able to design
maps for vector tiles using the existing tools and then switch
to a different renderer.

### index.html

Create a HTML page and include OpenLayers 3 and the standalone build
of [ol-mapbox-style](https://github.com/boundlessgeo/ol-mapbox-style) `omls.js`. Since OpenLayers 3 draws on the browser canvas you also need
to include the fonts used in the style in the page.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Klokantech Basic GL Style using ol-mapbox-style preview</title>
  <link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Open+Sans" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/ol3/3.19.1/ol.css">
  <style>
    html, body {
      height: 100%;
      margin: 0;
    }
    #map {
      width: 100%;
      height: 100%;
      background-color: #f8f4f0;
    }
  </style>
</head>
<body>
  <div id="map"></div>
  <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=fetch,Promise"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/ol3/3.19.1/ol.js"></script>
  <script src="olms.js"></script>
  <script src="ol3.js"></script>
</body>
```

### ol3.js

Create a new MVT source (`url` of `ol.source.VectorTile`) which points
to the CDN of OpenMapTiles or your own tileserver.

We fetch our GL style specification (`fetch` function) and turn
it into OpenLayers functions which we can apply to
our vector tile layer.

```javascript
var tilegrid = ol.tilegrid.createXYZ({tileSize: 512, maxZoom: 22});

var layer = new ol.layer.VectorTile({
  source: new ol.source.VectorTile({
    attributions: '© <a href="https://openmaptiles.org/">OpenMapTiles</a> ' +
      '© <a href="http://www.openstreetmap.org/copyright">' +
      'OpenStreetMap contributors</a>',
    format: new ol.format.MVT(),
    tileGrid: tilegrid,
    tilePixelRatio: 8,
    url: 'https://free-0.tilehosting.com/data/v3/{z}/{x}/{y}.pbf?key=tXiQqN3lIgskyDErJCeY'
  })
});

var view = new ol.View({
  center: [732602.1417165294, 5864590.06411005],
  resolution: 2445,
  maxResolution: 78271.51696402048
});

var map = new ol.Map({
  target: 'map',
  view: view
});

fetch('https://openmaptiles.github.io/klokantech-basic-gl-style/style-cdn.json').then(function(response) {
  response.json().then(function(glStyle) {
    olms.applyStyle(layer, glStyle, 'openmaptiles').then(function() {
      map.addLayer(layer);
    });
  });
});
```
