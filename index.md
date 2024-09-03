---
Title: Home

menus:
  main:
    weight: 1
    title: Home
layout: splash
classes: wide
---

<div class="wrapper">
  <div id="map" class="leafmap" style="border: 1px solid #ccc"></div>
  <div id="slider"></div>
</div>

<!-- Embed Power BI Report -->
<div style="height: 800px;">
  <iframe 
    width="100%" 
    height="100%" 
    src="https://app.powerbi.com/reportEmbed?reportId=9c37f0ab-48da-4ee1-8546-f330b830b3ec&groupId=7068ef98-a139-4ad6-960a-77fa44621a39&autoAuth=true&ctid=YOUR_TENANT_ID" 
    frameborder="0" 
    allowFullScreen="true">
  </iframe>
</div>

<script type="text/javascript" src="assets/GeoJSON/WesternInterconnection.js"></script>
<script type="text/javascript" src="assets/GeoJSON/TexasInterconnection.js"></script>
<script type="text/javascript" src="assets/GeoJSON/NordicGrid.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Russian.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Baltic.js"></script>
<script type="text/javascript" src="assets/GeoJSON/NationalGrid.js"></script>
<script type="text/javascript" src="assets/GeoJSON/ContinentalEurope.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Irish.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Iceland.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Faroe.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Mallorca.js"></script>
<script type="text/javascript" src="assets/GeoJSON/GranCanaria.js"></script>
<script type="text/javascript" src="assets/GeoJSON/SouthAfrica.js"></script>
<script type="text/javascript" src="assets/GeoJSON/Japan.js"></script>

<script src="assets/locations/locations.js"></script>

<script>
// Existing JavaScript for map and other elements
// (Same as in your original code)
</script>

{% include_relative details.md %}
