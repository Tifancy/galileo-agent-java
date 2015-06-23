# Mashape Analytics Java Agent


Java Agent to report HTTP traffic to [MashapeAPI Analytics](https://www.apianalytics.com/) using [API Log Format](https://github.com/Mashape/api-log-format/).Analytics Java Agent is a custom servlet filter which intercepts the request and response and sends it to API Analytics server asynchronously to generate analytics information. It needs a web container to run, to send data from a app running in Java SE environment please use our standalone proxy or use one of our data collection APIs.


## Installation 

	
### With Maven


```xml
<dependency>
  <groupId>com.mashape.analytics.agent</groupId>
  <artifactId>mashape-analytics</artifactId>
  <version>1.0.0</version>
</dependency>
``` 

### Without Maven



Application depends on `javax.servlet-api-3.0.1`, `jeromq-0.3.4`, `gson-1.2.17`, `log4j-1.2.17` and `guava-14.0.1`. For testing it depends on `jmockit-1.7`, `junit-4.12`, `unirest-java-1.4.5` and embedded Jetty
	
You can download the analytics jar from: <https://oss.sonatype.org/content/repositories/releases/com/mashape/analytics/agent/mashape-analytics/1.0.0/mashape-analytics-1.0.0.jar>
or clone the project from github: <https://github.com/Mashape/analytics-agent-java>
	
Dependencies

```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.0.1</version>
	<scope>provided</scope>
</dependency>
<dependency>
	<groupId>org.zeromq</groupId>
	<artifactId>jeromq</artifactId>
	<version>0.3.4</version>
</dependency>
<dependency>
	<artifactId>guava</artifactId>
	<groupId>com.google.guava</groupId>
	<type>jar</type>
	<version>14.0.1</version>
</dependency>
<dependency>
	<groupId>com.google.code.gson</groupId>
	<artifactId>gson</artifactId>
	<version>2.3.1</version>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>

<!-- Test dependencies -->
<dependency>
	<groupId>com.googlecode.jmockit</groupId>
	<artifactId>jmockit</artifactId>
	<version>1.7</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.12</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>org.eclipse.jetty</groupId>
	<artifactId>jetty-server</artifactId>
	<version>9.3.0.M1</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>org.eclipse.jetty</groupId>
	<artifactId>jetty-webapp</artifactId>
	<version>9.3.0.M1</version>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>com.mashape.unirest</groupId>
	<artifactId>unirest-java</artifactId>
	<version>1.4.5</version>
	<scope>test</scope>
</dependency>
```


## Configuration for Server

Filter has been tested on tomcat and should work with Jetty, Jboss and other servers supporting http servlet api. 
To use the filter you would need to add Analytics filter to web descriptor and set few VM arguments in the server.

Add following arguments to the server

     Property | Value
     -------- |	------
     analytics.token | Api analytics token from https://analytics.mashape.com 
     analytics.socket.min |Minimum number of threads/sockets to opened for connection to analytics server, default is 10
     analytics.socket.max | Maximum number of threads/sockets allowed to live in pool, default is 20
     analytics.socket.keepalivetime | When the number of threads are greater than the min, this is the maximum time that excess idle threads will wait for new tasks before terminating.
     analytics.queue.size | Size of the queue for holding the tasks of transferring data to analytics server, default is 100 
     analytics.enabled.flag | Set it true to enable analytics
     analytics.environment | Server environment name, default is a empty string
	
Update web.xml on server

```xml
<filter>
  <filter-name>apianalytics-filter</filter-name>
  <filter-class>com.mashape.analytics.agent.filter.AnalyticsFilter</filter-class>
  <init-param>
    <param-name>analytics.server.url</param-name>
    <param-value>socket.analytics.mashape.com</param-value>
  </init-param>
  <init-param>
    <param-name>analytics.server.port</param-name>
    <param-value>5000</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>apianalytics-filter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping> 
```
