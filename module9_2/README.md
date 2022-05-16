SonarQube:

![image](https://user-images.githubusercontent.com/22905019/168267418-379f474c-dd69-406b-8480-c108bfc3f363.png)

![image](https://user-images.githubusercontent.com/22905019/168267534-792a393a-1ee8-4c57-b769-3e00aa0e8672.png)

Nexus:

![image](https://user-images.githubusercontent.com/22905019/168293525-78ca9005-c567-4574-b6ff-74452d994dfb.png)

maven-metadata.xml:

~~~
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<metadata modelVersion="1.1.0">
<groupId>netology</groupId>
<artifactId>java</artifactId>
<versioning>
<latest>8_282</latest>
<release>8_282</release>
<versions>
<version>8_102</version>
<version>8_282</version>
</versions>
<lastUpdated>20220513131929</lastUpdated>
</versioning>
</metadata>
~~~

Maven:

![image](https://user-images.githubusercontent.com/22905019/168546477-20a1b6c6-2553-42d2-b95f-56cd26898daf.png)

![image](https://user-images.githubusercontent.com/22905019/168546662-3151b5cf-e643-4177-adc6-2b3bd07f532d.png)

pom.xml:

~~~
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.netology.app</groupId>
  <artifactId>simple-app</artifactId>
  <version>1.0-SNAPSHOT</version>
   <repositories>
    <repository>
      <id>my-repo</id>
      <name>maven-public</name>
      <url>http://localhost:8081/repository/maven-public/</url>
    </repository>
  </repositories>
  <dependencies>
      <groupId>netology</groupId>
      <artifactId>java</artifactId>
      <version>8_282</version>
      <classifier>distrib</classifier>
      <type>tar.gz</type>
    </dependency> -->
  </dependencies>
</project>
~~~
