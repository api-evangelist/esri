<!-- fetched 2026-10-14 from https://enterprise.arcgis.com/en/server/latest/publish-services/windows/ogc-features-service.htm by sweep-geospatial.py; text extracted from the provider's own page -->

﻿ Work with OGC Features services | ArcGIS Enterprise on Windows and Linux documentation Skip to main content 
 ArcGIS Enterprise on Windows and Linux 
 Table of Contents
 Work with OGC Features services
 You can publish services that comply with the Open Geospatial Consortium (OGC), OGC API Features specification. This is an open specification built on the legacy OGC Web Feature Services standards to serve geographic features over the web.
 How to create an OGC Features service
 You can create an OGC Features service from a map in ArcGIS Pro or by enabling the OGC Features capability on an existing map service by checking the OGC Features check box in ArcGIS Server Manager.
 When you create an OGC Features service, this creates a URL that any OGC Features client can use to access the service. The URL for the OGC Features landing page follows this pattern:
 https://gisserver.example.com/<context>/rest/services/<folder (if applicable)>/<service name>/OGCFeatureServer 
 Publish an OGC Features service from a map
 You can create an OGC Features service from ArcGIS Pro by publishing the layers in your map to ArcGIS Enterprise or to a stand-alone ArcGIS Server deployment. When prompted for the capabilities you want to enable when publishing, check OGC Features to create an OGC Features service.
 Create an OGC Features service from ArcGIS Pro
 This workflow explains how to share a web feature layer with the OGC Features capability enabled:
 To share all usable layers in the map as a web layer, on the Share tab, in the Share As group, click the Web Layer drop-down arrow and click Publish Web Layer . The Share As Web Layer pane appears.
 Provide a name for the web layer.
 Complete the Summary and Tags fields.
 If your active portal is ArcGIS Enterprise and you want to reference datasets in an enterprise geodatabase, confirm that Map Image is selected under Reference registered data . If you choose the Copy all data option, all layers are copied to the hosting server's managed database.
 Under Additional Layers , check the OGC Features box to create an OGC Features service.
 Under Share with , choose the level of permissions that your service will have.
 Click the Content tab to view the new web layer or layers and all associated components.
 Under Finish Sharing , click Analyze to review for problems.
 After you have resolved errors, and optionally, resolved warnings, click Publish to share the web layer.
 Types of clients
 OGC Features services can be used in any client that supports OGC API Features. A web browser can be a basic client of an OGC Features service where requests can be issued through HTTP and responses or exceptions are returned through the browser.
 Third-party clients may be available for consuming OGC Features services in addition to ArcGIS clients. Currently, the following OGC API Features clients have been implemented in ArcGIS:
 ArcGIS Pro 2.8 or later
 ArcGIS Online Map Viewer
 ArcGIS Maps SDKs for Native Apps
 ArcGIS Maps SDK for JavaScript
 Technical notes
 Keep the following in mind when creating an OGC Features service:
 The name of the layer in the map will be used as the collection name in the service.
 If publishing the data by reference from an enterprise geodatabase, the data must be registered with the geodatabase.
 Since OGC Features only works with features and tables, any raster layers in the map are excluded from the service.
 An OGC Features service only supports simple features and does not support geodatabase functionality such as relationship classes, joins, networks, and rules.
 If a feature class in your map uses a spatial reference that cannot be represented with an EPSG (European Petroleum Survey Group) code, WGS84 is used as the spatial reference for that feature class. Feature classes in your map or enterprise geodatabase that use an unknown spatial reference system are ignored by the service.
 The are currently no OGC Features-specific properties available to configure in the publishing workflow.
 By default, the page size limit for each collection is set to 3000.
 Back to top
