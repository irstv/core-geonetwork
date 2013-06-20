# GeoNetwork node for cartopolis


## Installation instruction

* Compile source code
```
git clone git@github.com:irstv/core-geonetwork.git geonetwork
cd geonetwork
mvn clean install
```

* Create Postgresql database
```
sudo su postgres
createdb geonetwork-cartopolis -O www-data
```
Database will be populated on first start.


* Create data directory
```
export GN_DATADIR=/var/gn_data
# export GN_DATADIR=/usr/local/App/apache-tomcat-7.0.21/gn_data_cartopolis
mkdir $GN_DATADIR
mkdir $GN_DATADIR/config
cp -fr web/src/main/webapp/WEB-INF/config-overrides-cartopolis*.xml $GN_DATADIR/.
cp -fr web/src/main/webapp/WEB-INF/data/data $GN_DATADIR/.
cp -fr web/src/main/webapp/WEB-INF/data/config/codelist $GN_DATADIR/config/.
chown -fR tomcat:tomcat $GN_DATADIR
```

* Configure data directory
In catalina.sh add JAVA_OPTS in order to set which data directory to use:
```
GN_DATADIR=/var/gn_data
JAVA_OPTS="$JAVA_OPTS -Dgeonetwork.dir=$GN_DATADIR -Dgeonetwork.schema.dir=$CATALINA_HOME/webapps/geonetwork/WEB-INF/data/config/schema_plugins -Dgeonetwork.jeeves.configuration.overrides.file=$GN_DATADIR/config-overrides-cartopolis.xml"
```
Here we set :
 * the data directory,
 * the schema plugin directory to use (the webapp one as far as we don't have custom plugins
 * the overriden configuration


* Deploy the webapp
```
export CATALINA_HOME=/usr/local/App/apache-tomcat-7.0.21
cp web/target/geonetwork.war $CATALINA_HOME/webapps/.
```


## Backup instruction

Backup the following:
 * the data dir
 * the db
 * optionnaly - the webapp
