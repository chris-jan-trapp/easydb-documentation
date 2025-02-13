---
title: "155 - Export, deep links and XSLT"
menu:
  main:
    name: "Export, deep links and XSLT"
    identifier: "webfrontend/administration/base-config/export"
    parent: "webfrontend/administration/base-config"
---
# Export, deep links and XSLT

Easydb provides several types of unauthenticated access to the files and data. For the accesses, on the one hand deep links and on the other OAI / PMH are available.

Use deep links when accessing a resource from the easydb directly, OAI / PMH can be used to monitor and load several resources as well as changes to resources.

## Deep link access

The deep link releases are technically solved via the API interface [/api/objects](../../../../technical/api/objects). There you will find explicit information about the structure of the URL. In the front-end, you will find the deep links [Detail]() and the [EAS-Column (parts)]() in various places. Deep links are always authenticated by the user *DeepLink* `deep_link`. Give this user the [ necessary rights](/en/technical/rightsmanagement/) to the data to allow access from outside.


| Settings |  Explanation |
| ------ | -------- |
| Allow | Turning the deep link on and off. |
| Including visible reference to ID | Allows direct access by object ID, because these object IDs are continually assigned, it can be a security risk to unblock this option. A user who is made aware of a deep link can guess further deep links guess. For all deep links, however, the *DeepLink* user must have access to the objects for them to work. |
| Including visible reference to a unique field | Like the ID reference, they specify whether or not one-to-one data fields can be deep-linked |
| Show EAS URLs | This option allows direct file links in the. XML output of the deep links written. These links point directly to a file and are no longer right-managed. These URLs never lose their validity. Without this option, there are other URLs for accessing files in the XML. |
| Embed linked objects | As for the XML Export, linked objects are not loaded and embedded in the XML Document.<br>If the option "All" is selected, all linked objects are loaded and embedded during the export.<br>If "Not included in main search" option is selected, all linked objects that are not included in the main search are loaded.<br>If "None" is selected, no linked objects are loaded, but only the standard is exported.<br>**Note:** this option has only an effect if the [format `xml_easydb`](../../../../technical/api/objects/#path-part-format) is used. |
| Nested depth | When linked objects are loaded and merged, this depth defines how many levels of linked objects are loaded (`1` - `9`). |

### XSLT Formats

You can upload different [XSLT](https://www.w3.org/TR/xslt/) style sheets to transform any XML output in easydb format into different output formats. This includes the [export in XML format](/en/webfrontend/datamanagement/features/export/#data), [deep links in XML format](/en/technical/api/objects/#path-part-format) and the OAI/PMH interface.

In addition to the standard easydb format and [Dublin Core](http://dublincore.org/) (which is mandatory for OAI-PMH), the OAI/PMH interface can provide custom formats (e. g. LIDO). To use Dublin Core, a Dublin Core mapping must be set up in [Metadata Mapping](../../profiles). Then it must be also linked to the corresponding [object type](../../datamodel/objecttype). For these formats, an XSLT must be created that converts the standard easydb format.

The OAI/PMH interface provides a metadata format for each uploaded XSLT. To enable a XSLT style sheet for the use in the OAI/PMH interface, you have to specify a unique name that can be used as the OAI/PMH prefix, and select the checkbox "Use for OAI/PMH". Then, this XSLT style sheet will be provided as an additional [metadata format](/en/technical/protocols/oai-pmh/#metadata-formats).

| Settings |  Explanation |
| ------ |  -------- |
| Name | Technical name. In the OAI / PMH interface, this is used as a metadata format (`metadataFormat=<name>`). In the Deep-Link interface, this XSLT can be applied by using `format/xslt/<name>` |
| Use for OAI/PMH | Allow this XSLT to be used in the OAI/PMH interface. **Note:** since the OAI/PMH standard requires XML, make sure that the XSLT produces valid XML. Otherwise a internal parsing error can occur in the OAI/PMH plugin. |
| Use for Deep-Links | Allow this XSLT to be used in the Deep-Link interface. |
| Schema | XML Schema (optional). |
| Namespace | XML Namespace (optional). |
| Display Name | Display name of the format (optional). |
| Description | Description of the format (optional). |
| XSLT | XSLT file for transforming the data. |


## Publish

Objects can be published to external repositories using the [`publish` API](/en/technical/api/publish/#publish-an-object).

Multiple collectors can be configured:

| Settings | Explanation |
|---|---|
| Display name | Displayname for the collector (optional) |
| Internal name | Name of the collector which triggered the publishing. This internal name is used to identify the collector in the API |
| URL | URL of the repository (optional, must be a valid URL if set) |
| Type | Freetext to identify and group the collectors (optional) |
| Prefix | If the objects are published at a relative URI, this prefix will be used to form the URI of the published object (optional, must be a valid URL if set) |


## OAI / PMH

The OAI / PMH interface is a harvesting interface. For more information, see the [Protocol Description](../../../../technical/protocols/oai-pmh) and [Open Archives](http://www.openarchives.org/).

The searches that perform the interface are performed with the system user *OAI / PMH* (`oai_pmh`). Give this user the [ necessary rights](/en/technical/rightsmanagement/) to see the data that you want to provide over this interface.

| Settings |  Explanation |
| ------ |  -------- |
| Share | Switching the OAI / PMH interface on and off. |
| Repository Name | Name of the OAI / PMH repository. |
| Administrator email | Email specified in OAI responses.
| Namespace | Freely definable OAI Identifier Namespace. Objects can be requested using `oai:<namespace>:<uuid>` in the URL. |
| Tag Sets | Define tag filters to create new OAI / PMH sets. They may be e.g. All objects that have the tag *Internet*. |
| Show EAS URLs | As with the deep links, this determines whether the direct file links will be output in the XML or not. See Deep link. |
| Embed linked objects | As for the XML Export, linked objects are not loaded and embedded in the XML Document.<br>If the option "All" is selected, all linked objects are loaded and embedded during the export.<br>If "Not included in main search" option is selected, all linked objects that are not included in the main search are loaded.<br>If "None" is selected, no linked objects are loaded, but only the standard is exported. |
| Nested depth | When linked objects are loaded and merged, this depth defines how many levels of linked objects are loaded (`1` - `9`). |
