# tomcatsslcertificate


1.	generate ssl key

	keytool -genkey -alias aliasname -keyalg RSA -keystore d:\localssl(path)
	keytool -genkey -alias sslalias -keyalg RSA -keystore d:\localssl
	password: samplessl
	
	keytool -importkeystore -srckeystore d:\localssl -destkeystore d:\localssl -deststoretype pkcs12
	
	
2.	remove the 8080 in url,	Edit the server.xml file

	Connector port="8080" protocol="HTTP/1.1"   connectionTimeout="20000" redirectPort="8443" /
	   to 
	Connector port="80" protocol="HTTP/1.1"  connectionTimeout="20000" redirectPort="443" /
	
	restart the server

3.	Add key in server.xml

<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
              maxThreads="150" scheme="https" secure="true"
              clientAuth="false" sslProtocol="TLS"
	       keystoreFile="d:\localssl"
	       keystorePass="samplessl" />
        keystoreFile : keystore path
	keystorePass  : keystore password		  

4. remove projet name in url : server.xml
				
		<Host name="localhost"  appBase="webapps"          unpackWARs="true" autoDeploy="true">
			<Alias>localhost</Alias>
		  <Context path="" docBase="o2" debug="0" privileged="true" />
		  <Valve className="org.apache.catalina.valves.AccessLogValve"
				 directory="logs"   prefix="localhost_access_log." suffix=".txt"
				 pattern="%h %l %u %t &quot;%r&quot; %s %b" resolveHosts="false" />   
		</Host>	
	remove the old Host Tag Details
	restart the server and check the url https://localhost/
	
	
5. http to https: web.xml
	<security-constraint>

		<web-resource-collection>
			<web-resource-name>Secured</web-resource-name>
			<url-pattern>/*</url-pattern>
		</web-resource-collection>
	   <user-data-constraint>
			<transport-guarantee>CONFIDENTIAL</transport-guarantee>
		</user-data-constraint>

	</security-constraint>
	before the </webapp> tag.
6. ssl certificate install into keystore
	
	1.Install the root certificate:	AddTrustExternalCARoot.crt
	2.Install the intermediate certificate:	USERTrustRSAAddTrustCA.crt
	3.Install the Main(domain) certificate:	domainname.crt
	
	
	
  
