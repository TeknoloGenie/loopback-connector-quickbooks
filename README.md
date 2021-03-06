# loopback-connector-quickbooks

**This connector is currently under development and should not be used in production, get involved and help us speed up 
development by taking a look at the TODO section**

The purpose of this connector is to act as the middle and end man in the process of communication to QuickBooks Web 
Connector used to communicate to QuickBooks Desktop software. Traditionally QuickBooks Web Connector requires a SOAP 
server to send and receive XML requests to view, create or update QuickBooks data. This connector has a built in SOAP 
server built in. The connector itself is responsible intaking a traditional RESTful API request, and using the SOAP 
service to communicate it to the QuickBooks Web Connector.

## Thank you
Big thanks to [johnballantyne](https://github.com/johnballantyne?tab=overview&from=2016-08-01&to=2016-08-31&utf8=%E2%9C%93). 
The built in SOAP service is based upon his implementation of [Node.js QBWebConnector service](https://github.com/johnballantyne/qbws) 
with modifications to support dynamic queries.

Another big thanks goes to the creator of the project [quickbooks-php](https://github.com/consolibyte/quickbooks-php), 
Keith Palmer from [ConsoliBYTE](https://github.com/consolibyte), LLC. Without his guidance on an issue i had when implementing the `Mod` and `Add` 
operations i would have pulled my hair out and OD'd on coffee. The project `quickbooks-php` and their usage of schema 
files for validation was a key part to this `connector`

## How it Works
 Once you have installed the connector and added the datasource the rest is magic. Since QuickBooks data is not 
 `schemaless` the connector is in charge of automatically building all required Loopback Models, meaning right out of 
 the box once you serve up your API you will have access to `Customers`, `UnitOfMeasureSets`, `Employees` and many more 
 (full list bellow). Upon serving your API all the data from QuickBooks will be available to query. You can use the 
 `reSync()` method to ask QuickBooks for updated data.    

## Install connector from NPM

    npm install loopback-connector-quickbooks --save

## Configuring QuickBooks connector
Edit **datasources.json** and add:

     "quickbooksService": {
        "name": "quickbooksService",
        "connector": "loopback-connector-quickbooks",
        "username": "qbuser",
        "password": "pas***rd1234"
        "companyFile": "C:\\Users\\Public\\Documents\\Intuit\\QuickBooks\\Company Files\\NodeGeeks LLC.qbw",
        "enableServiceLog": true,
        "config": {
          "verbosity": 2
        }
      }
    
Settings:
---------
- **name:** The name of the datasource must be `quickbooksService`
- **username:** Username used in the .QWC file created for your server
- **password:** Password set in QuickBooks Web Connector
- **companyFile:** Directory on your local machine where your QuickBooks company file is located
- **enableServiceLog:** Enabled debug logging, this coupled with the config.verbosity determines how deep logging is enabled  


## TODO
  
- [ ] Create tests for all 3 request ops (Query, Mod, Add) using `Customer` (QB List object) and `Estimate` (QB Transaction object). A complete test of every QuickBooks Object and Operations for those Objects should eventually be created.
- [ ] Cleanup code and add comments throughout the project to better document the code. 
- [ ] Add dual QuickBooks API support, currently only Desktop is supported, the QB Online API should also be added.
- [ ] Currently the `Employee` Object extends `User` to allow for Employee login via Loopbacks User Model, a couple of things should be done when creating the Model, first ACL policies must be implemented, and second we need to automate the process in which they choose a password for there Employee record, thus also automating determining what permissions/roles that Employee may or may not have.
- [ ] Refactor `QBWebService.reSync` to support to determine if each index value in `models`  param is a string for a simple query or if it is an object with a `filter` property that defines a more advanced query.

Because this is a `Loopback Connector` this project is to conform the coding style, commit message and terminology guidelines provided by `Loopback`
- [ ] Refactor to inherit [Loopbacks Coding Style](http://loopback.io/doc/en/contrib/style-guide.html) guidelines
- [ ] Create `CONTRIBUTING.md` file to set contributing guidelines and agreement

new todo's will be added periodically and until this is closed and this `loopback-connector` is marked `ready for production`.