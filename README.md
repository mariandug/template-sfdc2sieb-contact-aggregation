
# Anypoint Template: Salesforce and Siebel Contact Aggregation

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
	* [Siebel Considerations](#siebelconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
I want to aggregate contacts from Salesforce and Oracle Siebel Business Objects (Siebel), compare them to see what contacts can only be found in one of the two and what contacts are in both instances. 

For practical purposes this Template will generate the result in the format of a CSV Report sent by email.

This Template should serve as a foundation for extracting data from two systems, aggregating data, comparing values of fields for the objects, and generating a report on the differences. 

As implemented, it gets contacts from Salesforce and Siebel, compares them by the email address of the contacts, and generates a CSV file which shows contacts in Salesforce, contacts in Siebel, and contacts in both Salesforce and Siebel. The report is then emailed to a configured group of email addresses.

# Considerations <a name="considerations"/>

To make this Anypoint Template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made in order for all to run smoothly. **Failing to do so could lead to unexpected behavior of the template.**



## Salesforce Considerations <a name="salesforceconsiderations"/>

There may be a few things that you need to know regarding Salesforce for this template to work.

To have this template work as expected, you should be aware of your own Salesforce field configuration.

### FAQ

 - Where can I check that the field configuration for my Salesforce instance is the right one?

    [Salesforce: Checking Field Accessibility for a Particular Field][1]

- Can I modify the Field Access Settings? How?

    [Salesforce: Modifying Field Access Settings][2]


[1]: https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US
[2]: https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US

### As source of data

If the user configured in the template for the source system does not have at least *read only* permissions for the fields that are fetched, then an *InvalidFieldFault* API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```


## Siebel Considerations <a name="siebelconsiderations"/>

There may be a few things that you need to know regarding Siebel for this template to work.

This Anypoint Template may be using date time/timestamp fields from the Siebel to do comparisons and take further actions.
While the template handles the time zone by sending all such fields in a neutral time zone, it cannot discover the time zone in which the Siebel instance is on.
It is up to the user of this template to provide such information. For more about Siebel time zones, see [link](http://docs.oracle.com/cd/B40099_02/books/Fundamentals/Fund_settingoptions3.html)



### As destination of data

In order to make the siebel connector work smoothly you have to provide the correct version of the siebel jars (Siebel.jar, SiebelJI_enu.jar) that works with your Siebel installation.








# Run it! <a name="runit"/>
Simple steps to get Salesforce and Siebel Contact Aggregation running.


## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Mule Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Anypoint Studio from this [location](https://www.mulesoft.com/lp/dl/studio)
+ You can download Mule runtime from this [location](https://www.mulesoft.com/platform/mule)


### Importing an Anypoint Template into Studio
Anypoint Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio Project from a file system
+ Packaged Mule application (.jar)

You can find a detailed description on how to do so in this [documentation page](https://docs.mulesoft.com/).


### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click the Anypoint Template project folder 
+ Hover your mouse over `"Run as"`
+ Click `"Mule Application (configure)"`
+ Inside the dialog, select Environment and set the variable `"mule.env"` to the value `"dev"`
+ Click `"Run"`


### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Complete all properties in one of the property files, for example in [mule.prod.properties] (../master/src/main/resources/mule.prod.properties) and run your app with the corresponding environment variable to use it. To follow the example, this will be `mule.env=prod`. 
After this, to trigger the use case you just need to hit the local HTTP listener with the port you configured in your file. If this is, for instance, `9090` then you should hit: `http://localhost:9090/generatereport` and this will create a CSV report and send it to the mails set.

## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](https://docs.mulesoft.com/runtime-manager/) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.
Once your app is all set and started, supposing you choose as domain name `template-sfdc2sieb-contact-aggregation` to trigger the use case you just need to hit `http://template-sfdc2sieb-contact-aggregation.cloudhub.io/generatereport` and report will be sent to the emails configured.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Studio provides an easy way to deploy your template directly to CloudHub, for the specific steps to do so check this [link](https://docs.mulesoft.com/runtime-manager/deployment-strategies)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
To use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:
### Application configuration
+ http.port `9090` 

### Salesforce Connector configuration
+ sfdc.username `bob.dylan@sfdc`
+ sfdc.password `DylanPassword123`
+ sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

### Oracle Siebel Connector configuration
+ sieb.user `user`
+ sieb.password `secret`
+ sieb.server `server`
+ sieb.serverName `serverName`
+ sieb.objectManager `objectManager`
+ sieb.port `2321`

### SMTP Services configuration
+ smtp.host `smtp.gmail.com`
+ smtp.port `587`
+ smtp.user `exampleuser@gmail.com`
+ smtp.password `ExamplePassword456`

### Mail details
+ mail.from `exampleuser@gmail.com`
+ mail.to `woody.guthrie@gmail.com`
+ mail.subject `SFDC Contacts Report`
+ mail.body `Contacts report comparing contacts from SFDC and Siebel Contacts`
+ attachment.name `OrderedReport.csv`

# API Calls <a name="apicalls"/>
Salesforce imposes limits on the number of API Calls that can be made. However, we make API call to Salesforce only once during aggregation.


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As Mule applications are based on XML files, this page is organized by describing all the XML that conform the Anypoint Template.
Of course more files can be found such as test classes and Mule application files, but to keep it simple, we focus on the XMLs.

Here is a list of the main XML files you can find in this application:

* [config.xml](#configxml)
* [endpoints.xml](#endpointsxml)
* [businessLogic.xml](#businesslogicxml)
* [errorHandling.xml](#errorhandlingxml)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Configuration Properties](https://docs.mulesoft.com/mule4-user-guide/v/4.1/configuring-properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor, they can be found on the *Global Element* tab.


## businessLogic.xml<a name="businesslogicxml"/>
Functional aspect of the Template is implemented in this XML, directed by one flow responsible of conducting the aggregation of data, comparing records and finally formatting the output, in this case a report.

Using Scatter-Gather component we are querying the data in different systems. After that the aggregation is implemented in DataWeave 2 script using Transform component. Aggregated results are sorted by source of existence:

1. Contacts only in Salesforce
2. Contacts only in Siebel
3. Contacts in both Salesforce and Siebel

and transformed to CSV format. Final report in CSV format is sent to email, that you configured in mule.\*.properties file. 
Contacts will be matched by email address, that is to say, a record in both organisations with same email address is considered the same contact.



## endpoints.xml<a name="endpointsxml"/>
This is the file where you will found the endpoint to start the aggregation. This Template has an HTTP Inbound Endpoint as the way to trigger the use case.
### Trigger Flow
**HTTP Inbound Endpoint** - Start Report Generation
+ `${http.port}` is set as a property to be defined either on a property file or in CloudHub environment variables.
+ The path configured by default is `generatereport` and you are free to change for the one you prefer.
+ The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub will then route requests from your application domain URL to the endpoint.



## errorHandling.xml<a name="errorhandlingxml"/>
This is the right place to handle how your integration will react depending on the different exceptions. 
This file provides [Error Handling](https://docs.mulesoft.com/mule4-user-guide/v/4.1/error-handling) that is referenced by the main flow in the business logic.



