# GeoNetwork node for cartopolis


## Installation instruction

* Compile source code
```
git clone --recursive git@github.com:irstv/core-geonetwork.git
cd core-geonetwork
mvn clean install -DskipTests
```

* Create Postgresql database
```
sudo su postgres
createdb geonetwork-cartopolis -O geonetwork
```
Database will be populated on first start.


* Create data directory
```
export GN_DATADIR=/data/data_geonetwork
# export GN_DATADIR=/usr/local/App/apache-tomcat-7.0.21/gn_data_cartopolis
mkdir $GN_DATADIR
mkdir $GN_DATADIR/config
cp -fr web/src/main/webapp/WEB-INF/config-overrides-cartopolis*.xml $GN_DATADIR/.
cp -fr web/src/main/webapp/WEB-INF/data/data $GN_DATADIR/.
cp -fr web/src/main/webapp/WEB-INF/data/config/codelist $GN_DATADIR/config/.
chown -fR tomcat7:tomcat7 $GN_DATADIR
```

* Configure data directory
In catalina.sh (eg. in /usr/share/tomcat7/bin) add JAVA_OPTS in order to set which data directory to use:
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
export CATALINA_HOME=/data/tomcat7/webapp
cp web/target/geonetwork.war $CATALINA_HOME/webapps/.
chown tomcat7:tomcat7 $CATALINA_HOME/webapps/geonetwork.war
```

Start tomcat.
Check the log.


## Backup instruction

Backup the following:
 * the data dir
 * the db
 * optionnaly - the webapp
