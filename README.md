WebSphere Liberty – Installation, Deployment, Datasource & SSL Configuration (Quick Guide)
Today, I worked on WebSphere Liberty and noted down some important steps that can help anyone getting started with Liberty—especially those coming from traditional WebSphere (tWAS).
🚀 1. Liberty Installation
🔹 Download Liberty (Open Liberty or WebSphere Liberty)
 🔹 Install using ZIP/JAR:

unzip wlp-runtime.zip -d /opt/IBM/
# OR
java -jar wlp-install.jar --acceptLicense /opt/IBM/wlp
🔹 Create a server

/opt/IBM/wlp/bin/server create myServer
🔹 Start the server

server start myServer
📦 2. Application Deployment
✅ Dropins Deployment (Very Easy)
Copy WAR file into:

/opt/IBM/wlp/usr/servers/myServer/dropins/
✅ server.xml Deployment

<application id="MyApp" location="MyApp.war" type="war"/>
Place WAR in:

/opt/IBM/wlp/usr/servers/myServer/apps/
🗄 3. JDBC Datasource Configuration
➤ Add required features:

<featureManager>
 <feature>jdbc-4.3</feature>
</featureManager>
➤ Configure DataSource:

<dataSource id="MyDS" jndiName="jdbc/MyDS">
 <jdbcDriver libraryRef="OracleLib"/>
 <properties.oracle 
 user="dbuser"
 password="dbpass"
 url="jdbc:oracle:thin:@//host:1521/ORCL"/>
</dataSource>

<library id="OracleLib">
 <fileset dir="/opt/IBM/wlp/usr/shared/resources/dbdrivers" includes="*.jar"/>
</library>
🔐 4. SSL Certificate Configuration
➤ Create or import keystore:

keytool -genkeypair -alias default -keyalg RSA \
 -keystore myKey.p12 -storetype PKCS12 -storepass changeit
➤ server.xml SSL config:

<keyStore id="defaultKeyStore"
 location="resources/security/myKey.p12"
 password="changeit"/>

<ssl id="defaultSSLConfig"
 keyStoreRef="defaultKeyStore"/>

<httpEndpoint 
 httpPort="9080" 
 httpsPort="9443"
 sslRef="defaultSSLConfig"/>

⭐ Why Liberty?
Lightweight & extremely fast startup
Feature-based server.xml configuration
Cloud & container friendly
Easy DevOps integration
Much simpler than traditional WebSphere
