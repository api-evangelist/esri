<!-- fetched 2026-10-14 from https://enterprise.arcgis.com/en/server/latest/publish-services/windows/wmts-services.htm by sweep-geospatial.py; text extracted from the provider's own page -->

﻿ WMTS services | ArcGIS Enterprise on Windows and Linux documentation Skip to main content 
 ArcGIS Enterprise on Windows and Linux 
 Table of Contents
 WMTS services
 The Open Geospatial Consortium, Inc. (OGC), Web Map Tile Service (WMTS) specification is an international specification for serving digital maps over the web using cached image tiles. When you create a cached map or image service using ArcGIS Server, the service and its tiles are automatically accessible using the WMTS specification.
 Why use a WMTS service?
 WMTS services are useful if you want to make your cached map or image services available in an open, recognized way across different platforms and clients. Additionally, WMTS services are an effective way to make your ArcGIS Server cached map or image services run faster on OGC clients.
 Scope and compatibility
 Clients built to support the WMTS 1.0.0 specification and RESTful or KVP encoding can view and work with WMTS services. SOAP encoding is not supported.
 Client applications work with a WMTS service by appending parameters to the service's URL. WMTS services derived from cached map or image services support the following operations (RESTful and KVP encoding only):
 Requesting metadata about the service (GetCapabilities)
 Requesting an individual tile resource in the cache (GetTile)
 You can learn more about WMTS services at the Open Geospatial Consortium website. Esri also maintains a Standards and Interoperability web page detailing its support for OGC services in ArcGIS.
 Publishing a WMTS service
 To publish a WMTS service, you need to create a cached map or cached image service. Unlike other types of OGC services, there is no WMTS capability option to enable when creating a cached map or image service; WMTS is always enabled. For a detailed walk-through of the process of creating a cached map or image service, see the following topics:
 Tutorial: Creating a cached image service 
 Consuming WMTS services
 WMTS services running on ArcGIS Server can be consumed in any client that supports making either RESTful or KVP WMTS requests.
 Types of clients
 A web browser is the simplest client of a WMTS service. However, there are additional third-party clients available for consuming WMTS services. To consume WMTS services hosted by ArcGIS Server, the client must support WMTS 1.0.0. Visit the following links for more information about third-party clients capable of consuming WMTS services:
 OpenLayers 
 ArcGIS Maps SDK for JavaScript 
 URL structure
 To consume a WMTS service, you'll need to know the URL, which follows this pattern:
 For cached map services: https://gisserver.example.com:6443/arcgis/rest/services/folder/service/MapServer/WMTS 
 For example, if you created the cached map service WMTSDemoService and published the service in a folder called DemoFolder on server gisserver with the port number 6443, the URL would be as follows:
 https://gisserver:6443/arcgis/rest/services/DemoFolder/WMTSDemoService/MapServer/WMTS 
 For cached image services: https://gisserver.example.com:6443/arcgis/rest/services/fodler/service/ImageServer/WMTS 
 For example, if you created the cached image service WMTSDemoService and published the service in a folder called DemoFolder on server gisserver with the port number 6443, the URL would be as follows:
 https://gisserver:6443/arcgis/rest/services/DemoFolder/WMTSDemoService/ImageServer/WMTS 
 WMTS requests for GetCapabilities and GetTile operations can be issued through HTTP to this URL. Responses and exceptions to these requests are then returned to the client. For more information about how to create a specific WMTS resource URL, see the OGC WMTS 1.0.0 specification or ArcGIS Server REST API WMTS documentation.
 Back to top
