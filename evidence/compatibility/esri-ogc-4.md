<!-- fetched 2026-10-14 from https://enterprise.arcgis.com/en/server/latest/publish-services/windows/wfs-services.htm by sweep-geospatial.py; text extracted from the provider's own page -->

﻿ WFS services | ArcGIS Enterprise on Windows and Linux documentation Skip to main content 
 ArcGIS Enterprise on Windows and Linux 
 Table of Contents
 WFS services
 You can publish services that comply with the Open Geospatial Consortium (OGC), Web Feature Server (WFS) specification. This is an open specification for serving geographic features over the web.
 When to use a WFS service
 Serving your data through a WFS service allows any application that can work with web services to access geographic features from your map or enterprise geodatabase. Unlike the OGC Web Map Service (WMS), which returns an image of a map, the WFS service returns features with geometry and attributes that clients can use in geospatial analysis. WFS services also support filters that allow you to perform spatial and attribute queries on the data.
 Note:
 The WFS services you create are compliant with the WFS 1.1.0 and 2.0.0 specifications. They also support the WFS 1.0.0 specification on a read-only basis.
 WFS services use Geography Markup Language (GML) to encode the feature data. GML allows you to use XML to represent geographic information. The GML used by ArcGIS Server WFS services uses the Simple Features profile.
 Create a WFS service
 You can create a WFS service from a map in ArcGIS Pro.
 Note:
 If a feature class in your map or enterprise geodatabase uses a spatial reference that cannot be represented with an EPSG (European Petroleum Survey Group) code, WGS84 is used as the spatial reference for that feature class. Feature classes in your map or enterprise geodatabase that use an unknown spatial reference system are ignored by the WFS service.
 Creating a WFS service from a map
 You can create a WFS service by publishing your web feature layer to ArcGIS Server. When prompted for the capabilities you want to enable when publishing, check WFS . This creates a URL that any WFS client can use to access the service.
 The map is just a specification of the layers that will be available in your WFS service. Things like symbology and coded value domain descriptions do not transfer to the WFS service, because the purpose of the service is to expose the features in the data. To expose the visual properties of your map through OGC specifications, use a WMS service .
 Remember the following items when publishing a WFS service from a map:
 If you want the WFS service to support transactions for editing (WFS-T), the source data for all the layers in the map must come from the same enterprise geodatabase and the data must be registered with the server. The map cannot contain layers from multiple sources.
 The name of the layer is the type name returned from the WFS service.
 If publishing the data by reference from an enterprise geodatabase, the data must be registered with the geodatabase.
 Since WFS only works with features and tables, any raster layers in the map are excluded from the service.
 WFS services only support simple features and do not support geodatabase functionality such as relationship classes, joins, networks, and rules.
 Create a WFS service from ArcGIS Pro
 This workflow explains how to share a web feature layer with the WFS capability enabled.
 To share all usable layers in the map as a web layer, on the Share tab, in the Share As group, click the Web Layer drop-down arrow and click Publish Web Layer .
 The Share As Web Layer pane appears.
 Provide a name for the web layer.
 Complete the Summary and Tags fields.
 If your active portal is ArcGIS Enterprise and you want to reference datasets in an enterprise geodatabase, confirm that Map Image is selected under Reference registered data . If you choose to Copy all data , all layers are copied to the hosting server's managed database.
 Under Additional Layers , check the WFS box to create a WFS service.
 To modify or set any of the available WFS properties , click the Configure Web Layer Properties button .
 Under Share with , check the box to share with Everyone , since WFS services must be shared with the public.
 Click the Content tab to view the new web layer or layers and all associated components.
 Under Finish Sharing , click Analyze to review for problems.
 After you have resolved errors, and optionally, resolved warnings, click Publish to share the web layer.
 Configuring WFS service properties
 A WFS service's properties are reflected in its capabilities files. When publishing a WFS service with system-generated capabilities files (the default), it is recommended that you populate the WFS service properties. For information about each WFS service property you can set, see Available WFS service properties .
 In addition to the WFS properties that are available when publishing, additional service properties can be set after the service is published. This can be done by editing the WFS extension properties of the JSON representation of the service configuration through the admin API. Properties of interest include the ability to use field aliases instead of field names (" ignoreAliases ") or set the geometry precision of output coordinates (" geometryPrecision "). There are also properties available to optimize performance on large datasets such as skipping slow queries to the database and the ability to enable or disable streaming. Setting the optimizedNumberReturned property to true allows the server to skip time consuming queries to the database that slow down performance when populating the Number Returned WFS parameter for GetFeature responses. To be compliant with OGC standards, this property is set to false by default. Another performance related property is the disableStreaming property. By default, streaming is enabled and WFS responses are delivered via a response streamer. This can help improve performance when working with very large datasets. However, there are some limitations where you may need to disable streaming for your WFS service, for example, using a proxy server or deploying an SOI.
 Setting WFS properties using an external capabilities file
 Another way to define the metadata for a WFS service is by using an external capabilities file. This allows you to include additional projections for your feature types other than the defaults. The defaults include the coordinate system of the layer or feature class and WGS84 (EPSG 4326). See Use external capabilities files with WFS services for more information.
 Securing WFS services
 A WFS service exposes an ArcGIS Server map service to WFS consumers. The security for a WFS service is managed by controlling the security of its parent map service. If a particular role—for example, Planners—is denied access to a map, Planners will not be able to access the map through SOAP, REST, or WFS interfaces.
 ArcGIS Server supports a number of authentication schemes. Services that are expected to be accessed via OGC interfaces should be secured using HTTP Basic or HTTP Digest. Most OGC clients (both Esri and third-party clients) will understand and work with these widespread standard authentication schemes.
 ArcGIS Server supports a number of authentication schemes. Services that are expected to be accessed via OGC interfaces should be secured using Integrated Windows Authentication, HTTP Basic, or HTTP Digest. Most OGC clients (both Esri and third-party clients) will understand and work with these widespread standard authentication schemes.
 Web editing with WFS services (WFS-T)
 A transactional WFS service (sometimes known as WFS-T) allows WFS editors to apply changes to the data in the source database through the WFS service. To enable WFS-T, the data must be from an enterprise geodatabase connected as a user that has editing privileges on the data and published by reference. If those conditions are met, check the Enable transactions box when creating a WFS service from ArcGIS Pro. Transactions can be enabled on services that have versioned data or nonversioned data. If you choose to use versioned data, it is also recommended that you publish the service from a nondefault version.
 For examples of POST requests to insert, update, or delete features, see Communicating with a WFS service in a browser .
 Additional notes
 Some additional notes to keep in mind when working with WFS-T services:
 When you enable editor tracking on a feature class, ensure that the edits are set to be recorded in UTC. WFS-T services do not support Database Time as the time zone for tracking edits.
 The DBMS user that ArcGIS Server uses to connect to the enterprise geodatabase must be granted permissions to create tables in the DBMS to successfully use WFS-T.
 ArcGIS does not have a mechanism for transactional WFS editing, meaning you must use a third-party client to edit features using WFS.
 Feature classes that are z aware cannot be edited using WFS-T.
 Back to top
