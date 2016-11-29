Name Flat & Delimited File Export

 Author	Erwin 't Hoen  
 Type	Module  
 Latest Version	v4.1  
 Package filename	FlatFileExport_Mx5211.mpk
 
 
Description
 

The flat & delimited file export module allows you to export data from your model to fixed length and/or delimited files without the need for java programming. 
The module has been setup in such a way that with a little modeling and some configuration your are able to export data from a microflow.

 

Typical usage scenario
 

When you need to export data to a delimited or fixed length file from a microflow this module is for you
Let's say you have a Person, Location and Organisation entity in your model
The Person is related to the organisation and location in a 1:1 fashion, so the person 'knows' the organisation and the location, Now you have the need to create a delimited or fixed length file based on the person, the related organisation and related location for some interface to an external system.
The data can be exported directly from your Person entity or alternatively you create an interface table that will hold the data (for logging purposes for example), by creating a n:1 relation between the non-persistant ExportBatch entity and your entity holding the data and create a microflow that will perform the action . Seems difficult? Don't worry an example is provide in the module!
Now you finished your modeling and it's time to configure what needs to be exported. 
Features and limitations
 Features

Export fixed length and delimited files
No java work needed, only modeling and configuration
Export direct attributes
Export related attributes
Full logging in the runtime
Padding (left/right)
Optionally add headers to the exported data
Allow the module to write the result to a server directory or alternatively allow the user to download the export file

Limitations:

Only referenced objects that are related with the 1 side on the entity data to be exported can be used for exporting
Installation
 

See the general instructions under How to Install.

Dependencies
 

Mendix 5.21.1 environment
The MxModelReflection module needs to be installed
Configuration
General:

 

Connect the ExportConfiguration_Overview form in the implementation folder to your navigation
Assign module permissions (Configurator & Export_User) to the appropriate user roles
Synchronize your model in the MxModelReflection module
 
Testing (optional):

 

Connect the overview forms in the example/forms folder to your navigation
Synchronize your MxModelReflection module (make sure to select the FlatFileExport module to be synchronized)
Create at least 1 record from the TestExportEntity_Overview form and if you want to test the reference export create a record from the TestExportRefEntity_Overview and connect the record to the one created earlier.
Create your configuration from the ExportConfiguration_Overview and use the name TEST (for details on the configuration setup see the Configuration Setup section)
Try the export200 or export selected buttons on the TestExportEntity_Overview form to get the data exported
If you want to try to export some more records there is a function on the TestExportEntity_Overview form that will create 199 copies of the record you created.
 
 
Configuration setup:
General
Name: Unique name for the configuration, to be used in the calling microflow (see example)
File Type: A choice of delimited or fixed length files
Delimiter: The delimiting character to be used when exporting delimited files
Download Result: This option will create a FileDocument that can be downloaded by the user.
Directory: The directory where the resulting files are saved on the server (only available when Download Result is false!) If the result should be stored in the tmp directory just add a /  in the field, if a sub directory is needed fill the field with /<subdir name>/
File encoding: Options are UTF-8 and Cp1252(default) depending on your needs
File Name: The name for the files as <name>.<extension>, files will be created with a unique name based on the time of exporting
Use Headers: This option will allow the configurator to define headers for the resulting export. Definition of these headers is done in the Column Definitions
Select Entity: Select the entity that will holds the data to be exported
 
After saving the data entered above you will be able to define the columns to be exported
 
Columns:
Column Number: Auto generated number for the column indicating the position in the resulting file
Column Header: The header to be used in the export file for this column
Pad: choice of padding the data:
No Padding
Left Padding
Right Padding
Pad with: When padding left or rigth this field holds the character used in the padding
Length: the length of the data to be exported and optionally padded
Is Reference: Select this if the attribute you are defining is stored in a related entity
Select Attribute: Select one of the attributes from the entity definined for exporting (see General section)
Select Reference: If the attribute you are defining is stored in a related entity this allow you to choose the relation to be used for the related entity
Select Reference Attribute: Select the attribute from the related entity
Date Time Format: If the attribute selected is a datetime field you need to supply the definition with a format to be used for the datetime value (default is: dd-MM-yyyy HH:mm:ss)
 
Ready your own entities for exporting:

To allow your own entities to be exported with a microflow follow the next steps.

Create a referenceset type relation from the FlatFileExport.ExportBatch entity to your entity (the N is on the ExportBatch side, see example)
Synchronize the MxModelReflection module
Create your Export Configuration
Create a microflow (1) that retrieves a list of your entity's data (the data to be exported), creates a new exportbatch record with the correct reference set with the list and call the export microflow (see example in the module)
Create a second microflow (2) that can be copied from the module's execution folder (ExportFlow) and change the first action so that your configuration name and defined entity are used)
Connect your microflow (1) to a button or other microflow for execution from the client and test your export.