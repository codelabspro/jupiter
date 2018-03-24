# jupiter
A Node-Loopback-React app

## Steps


-----------------------------------------------------------------------
Step 1) Install loopback-cli

==> npm install -g strongloop (optional)
==> npm install -g loopback-cli

-----------------------------------------------------------------------
Step 2) Install connector-mysql or connector-postgresql


==> npm install loopback-connector-mysql --save

or

npm install loopback-connector-postgresql --save

-----------------------------------------------------------------------
Step 3) Specify mysql database for datasource


==> lb datasource jupiterdb --connector mysql

or 

==> lb datasource jupiterdb --connector postgresql
-----------------------------------------------------------------------

Step 4) Run lb model to create model class for TodoItem 

==> lb model
==> lb model
? Enter the model name: TodoItem
? Select the datasource to attach TodoItem to: db (mysql)
? Select model's base class PersistedModel
? Expose TodoItem via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let's add some TodoItem properties now.

Ensure that server/datasources.json looks as follows 

for mysql
~~~
{
  "db": {
    "host": "localhost",
    "port": 0,
    "url": "",
    "database": "jupiterdb",
    "password": "qwerty",
    "name": "db",
    "user": "admin",
    "connector": "mysql"
  }
}
~~~

or for postgresql

~~~
"mydb": {
  "name": "mydb",
  "connector": "postgresql"
  "host": "mydbhost",
  "port": 5432,
  "url": "postgres://admin:admin@mydbhost:5432/db1?ssl=false",
  "database": "db1",
  "password": "admin",
  "user": "admin",
  "ssl": false
}
~~~

-----------------------------------------------------------------------

Step 5) Add autoupdate.js migration script under server/boot/autoupdate.js

~~~
module.exports = function(app) {
    var path = require('path');
    var models = require(path.resolve(__dirname, '../model-config.json'));
    var datasources = require(path.resolve(__dirname, '../datasources.json'));

    function autoUpdateAll(){
        Object.keys(models).forEach(function(key) {
            if (typeof models[key].dataSource != 'undefined') {
                if (typeof datasources[models[key].dataSource] != 'undefined') {
                    app.dataSources[models[key].dataSource].autoupdate(key, function (err) {
                        if (err) throw err;
                        console.log('Model ' + key + ' updated');
                    });
                }
            }
        });
    }

    function autoMigrateAll(){
        Object.keys(models).forEach(function(key) {
            if (typeof models[key].dataSource != 'undefined') {
                if (typeof datasources[models[key].dataSource] != 'undefined') {
                    app.dataSources[models[key].dataSource].automigrate(key, function (err) {
                        if (err) throw err;
                        console.log('Model ' + key + ' migrated');
                    });
                }
            }
        });
    }
    //TODO: change to autoUpdateAll when ready for CI deployment to production
    autoMigrateAll();
    //autoUpdateAll();

};
~~~



------------------------------------------------------------------------
Step 6) Start up the server as follows : 


==> node .

------------------------------------------------------------------------
Step 7) Open explorer in browser to view and interact with the API 




------------------------------------------------------------------------
Step 8) Send a curl command to POST to the REST API 


curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \
   "title": "Title", \
   "description": "Desc" \
 }'

------------------------------------------------------------------------
Step 9) Verify that DB has the record for TodoItem



------------------------------------------------------------------------

==> mysql -u admin -h localhost -p



------------------------------------------------------------------------


Step 10) Create a client_src folder and start app in that folder


==> mkdir client_src
==> cd client_src
==> sudo npm install -g create-react-app
==> create-react-app .


------------------------------------------------------------------------

Step 11) Install router 


==> npm install --save react-router react-router-dom


------------------------------------------------------------------------
## Optional

To have launchd start mongodb now and restart at login:
  brew services start mongodb

Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf


## Useful Links

https://www.raymondcamdencom/2016/04/27/loopback-strongloop-and-api-connect-how-in-the-heck-do-they-relate/


https://github.com/strongloop/loopback-connector-postgresql