#### Spring Boot Support
The `liberty-maven-plugin` provides support for Spring Boot applications, allowing you to install the [Spring Boot executable jar](https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html) to Open Liberty and WebSphere Liberty runtime versions 18.0.0.2 and above. `spring-boot-maven-plugin` should be configured before `liberty-maven-plugin` to create an executable jar.

###### Additional Parameters

The following are the parameters supported by the `install-apps` goal in addition to the [common server parameters](common-server-parameters.md#common-server-parameters) and the [common parameters](common-parameters.md#common-parameters).

| Parameter | Description | Required |
| --------  | ----------- | -------  |
| appsDirectory | The server's `apps` or `dropins` directory where the application files should be copied. The default value is set to `apps` if the application is defined in the server configuration, otherwise it is set to `dropins`.  | No |
| installAppPackages | The Maven packages to copy to Liberty runtime's application directory. `spring-boot-project` should be configured to this parameter. | Yes |

The `server.xml` provided by the `configFile` parameter should enable the following Spring Boot features.

| Feature | Description | Required |
| --------  | ----------- | -------  |
| springBoot-1.5 | Support Spring Boot applications with Spring Boot version 1.5.x.  | Yes |
| springBoot-2.0 | Support Spring Boot applications with Spring Boot version 2.0.x and above.  | Yes |

The liberty features that support the Spring Boot starters can be found [here](https://www.ibm.com/support/knowledgecenter/SSAW57_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/rwlp_springboot.html). They should be enabled in the `server.xml` along with the appropriate Spring Boot feature.

Example:
To use the `liberty-maven-plugin` to install Spring Boot application, include the appropriate XML in the plugins section of your `pom.xml`.

```xml
<build>
    <plugins> 
        ...   	  
        <plugin>
            <groupId>net.wasdev.wlp.maven.plugins</groupId>
            <artifactId>liberty-maven-plugin</artifactId>
            <executions>
               ...
                <execution>
                    <id>install-apps</id>
                    <phase>pre-integration-test</phase>
                    <goals>
                        <goal>install-apps</goal>
                    </goals>
                    <configuration>
                    	 <appsDirectory>apps</appsDirectory>
                        <installAppPackages>spring-boot-project</installAppPackages>
                    </configuration>
                </execution>
                ...
            </executions>
            <configuration>
               <installDirectory>/opt/ibm/wlp</installDirectory>
               <serverName>test</serverName>
           </configuration>
        </plugin>
    </plugins>
</build>

```
