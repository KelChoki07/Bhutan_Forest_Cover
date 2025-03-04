# Download Bhutan_Forest_Cover

1. Hansen Global Forest Change v1.11 (2000-2023)

var countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
var roi = countries.filter(ee.Filter.eq('country_na', 'Bhutan'));

var dataset = ee.Image('UMD/hansen/global_forest_change_2023_v1_11');

Map.addLayer(dataset.clip(roi), {bands: ['treecover2000'], palette: ['000000', '00FF00'], max:100}, 'Bhutan Tree cover');
Map.addLayer(ee.image().paint(roi, 0,1), {palette: ['red']}, 'Bhutan');

2. ESA Land Cover Data 2021
-Global land cover product at 10 m resolution for 2020 based on Sentinel-1 and 2 data

// Download ESA Land Cover

// 1. Import ESA Data
var esa = ee.ImageCollection("ESA/WorldCover/v100").first();
var visualization = {bands: ['Map']};
Map.addLayer(esa, visualization, 'ESA Land Cover');
Map.centerObject(esa);

// 2. Add country boundary layer
var countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017');
var roi = countries.filter(ee.Filter.eq('country_na', 'Bhutan'));
Map.addLayer(roi, {}, 'Bhutan', false);

// 3. Clip with roi(Region of Interest)
var esa_clip = esa.clip(roi);
Map.addLayer(esa_clip, visualization, 'Bhutan ESA Land Cover');
Map.centerObject(esa_clip);

// 4. Export to Drive 
Export.image.toDrive({
  image: esa_clip,
  description: "ESA_LULC_ROI",
  scale: 10,
  region: roi,
  maxPixels: 1e13
});


Link: https://code.earthengine.google.co.in/aaa53014274476aed8838f6c3caa855c
