# NotesTechnical

1. How to use one spring boot project in another project?
   - Say you have project A and B and you want to use B jar in A.
   - in A you add a pom dependency as :
   
      	<dependency>
			<groupId>com.cdk.B.sdk</groupId>
			<artifactId>BArtifact</artifactId>
			<version>BVersion</version>
		</dependency>
    
    But to make it work in local, scope and systemPath is required as below in dcm project
    
    	<dependency>
			<groupId>com.cdk.B.sdk</groupId>
			<artifactId>BArtifact</artifactId>
			<version>BVersion</version>
			<scope>system</scope>
			<systemPath>B jar with dependencies path in local</systemPath>
		</dependency>
    
    - Also when you build B project , 1 jar file is generated and in spring boot project it generates in a different way where all classes are in BOOT-INF file and so you won't be able to use it in local.
    - So in B project, tried to comment the below code :

      <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      .......
  
  and add below code which generates jar with dependencies:
  
   <build>
    <plugins>
      <!-- any other plugins -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>3.1.0</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
    </plugins>
  </build>
