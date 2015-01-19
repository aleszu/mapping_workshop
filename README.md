# Nieman mapping workshop
This workshop is going to be fun fun fun

##Datasets:
Ambient air pollution by city and country 2014 from WHO: here
John Snow cholera deaths and water pumps dataset: here

##Ambient air quality map
Download .xls file here
Open in Excel or Numbers or LibreOffice or Open with Google Sheets
1. Go to countries sheet
or 2. Go to cities sheet
Copy entirety and paste into a new spreadsheet, name it and save it.
Condense titles into one single row along the top
Delete Region column
Make sure we have data for 'Reference for air quality' column 
If not, copy and paste from original .xls 
Make sure it's clean (enough)
Upload to CartoDB (automatically geocoding)
In map view, enable Choropleth
Enable hover infowindow and select country, PM2.5, PM10, and data source
Back to data view to see null in geometry (talk about shapefiles)

Bolivia 
{"type": "MultiPolygon", "coordinates": [[[[-65.19, -22.09], [-65.75, -22.11], [-66.22, -21.78], [-67.18, -22.82], [-67.88, -22.83], [-68.76, -20.41], [-68.44, -19.43], [-69.48, -17.64], [-69.5, -17.51], [-68.82, -16.34], [-69.42, -15.62], [-68.67, -12.5], [-69.57, -10.95], [-68.58, -11.11], [-66.63, -9.91], [-65.38, -9.7], [-64.99, -12.01], [-60.47, -13.81], [-60.16, -16.26], [-58.33, -16.28], [-58.4, -17.25], [-57.52, -18.2], [-58.16, -20.17], [-59.1, -19.35], [-61.74, -19.65], [-62.64, -22.24], [-63.94, -22], [-64.32, -22.87], [-64.59, -22.21], [-65.19, -22.09]]]]}

Find the multipolygon coordinates data (here) for Venezuela and input it. This is written in JSON. More granular coordinates can be found here.

##Design in mind
Projections
Data sources
Point vs. Regional data
Consider elements of design (leading and misleading, typography, palette, the color blind)

##John Snow map: How To with CartoDB
Tutorial here




CartoCSS (documentation here)

To do
import zip files (cholera deaths and pumps)
bubble visualization
radius 6 to 12, give or take
retitle infowindow count to 'Deaths'

SQL
delete what's in there and add this.

SELECT cartodb_id, the_geom_webmercator, count, 'cholera' as layer FROM cholera_deaths
UNION ALL
SELECT cartodb_id, the_geom_webmercator, NULL as count, 'pump' as layer FROM pumps

This is... "selectingthe_geom_webmercator from each table. From the cholera_deaths table we are selecting count so that we can base our bubble size on the value. When we UNION tables we must have the same columns in each table, so for the pumps table we have to include a fake count column with a value of NULL in every row. Finally, We included a column in the results for both tables that indicates a name for the layer the row belongs to."

CartoCSS

/** bubble visualization */

#cholera_deaths{
  marker-fill-opacity: 0.9;
  marker-line-color: #FFF;
  marker-line-width: 0.5;
  marker-line-opacity: 1;
  marker-placement: point;
  marker-multi-policy: largest;
  marker-type: ellipse;
  marker-fill: #000000;
  marker-allow-overlap: true;
  marker-clip: false;
  marker-comp-op: overlay;
}
#cholera_deaths [layer='pump'] {
  marker-width: 12.0;
  marker-fill: #3399FF;
  marker-line-color: black;
  marker-line-width: 0;
  marker-opacity: 1;
  marker-placement: point;
  marker-type: ellipse;
  marker-allow-overlap: true;
}
#cholera_deaths [ count <= 15] {
   marker-width: 12.0;
}
#cholera_deaths [ count <= 8] {
   marker-width: 11.3;
}
#cholera_deaths [ count <= 8] {
   marker-width: 10.7;
}
#cholera_deaths [ count <= 7] {
   marker-width: 10.0;
}
#cholera_deaths [ count <= 5] {
   marker-width: 9.3;
}
#cholera_deaths [ count <= 5] {
   marker-width: 8.7;
}
#cholera_deaths [ count <= 4] {
   marker-width: 8.0;
}
#cholera_deaths [ count <= 3] {
   marker-width: 7.3;
}
#cholera_deaths [ count <= 2] {
   marker-width: 6.7;
}
#cholera_deaths [ count <= 2] {
   marker-width: 6.0;
}

