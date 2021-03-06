# Maven BOM Lab

### Config ``` versions-maven-plugin ```

  ```  
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>versions-maven-plugin</artifactId>
        <version>${maven-versions-plugin.version}</version>
        <configuration>
            <allowSnapshots>true</allowSnapshots>
            <processParent>true</processParent>
        </configuration>
    </plugin>
  ```

### Lock down SNAPSHOT versions

- ``` mvn versions:lock-snapshots ```

### Unlock SNAPSHOT versions

- ``` mvn versions:unlock-snapshots ```  

### Revert: Restores the pom from the initial backup

- ``` mvn versions:revert ```

## A BOM Example

  ```
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                                 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
        <modelVersion>4.0.0</modelVersion>
    
        <groupId>com.tecsys</groupId>
        <artifactId>tecsys-all-dependencies</artifactId>
        <version>2021.1.0-0-0-SNAPSHOT</version>
        <packaging>pom</packaging>
    
        <properties>
    
            <!-- Versions of Common Tools -->
            <java.version>15</java.version>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
            <maven.compiler.source>${java.version}</maven.compiler.source>
            <maven.compiler.target>${java.version}</maven.compiler.target>
    
            <!-- <tecsys-base-lang.version>2021.1.0-0-0-SNAPSHOT</tecsys-base-lang.version> -->
    
            <!-- Versions of Plugins -->
            <maven-clean-plugin.version>3.1.0</maven-clean-plugin.version>
            <maven-compiler-plugin.version>3.8.1</maven-compiler-plugin.version>
            <maven-surefire-plugin.version>3.0.0-M5</maven-surefire-plugin.version>
            <maven-failsafe-plugin.version>3.0.0-M5</maven-failsafe-plugin.version>
            <maven-versions-plugin.version>2.8.1</maven-versions-plugin.version>
            <maven-enforcer-plugin.version>3.0.0-M3</maven-enforcer-plugin.version>
    
        </properties>
    
        <dependencyManagement>
    
            <dependencies>
    
                <dependency>
                    <groupId>com.tecsys</groupId>
                    <artifactId>tecsys-base-lang</artifactId>
                    <version>2021.1.0-0-0-SNAPSHOT</version>
                </dependency>
    
                <dependency>
                    <groupId>com.tecsys</groupId>
                    <artifactId>tecsys-base-app</artifactId>
                    <version>2021.1.0-0-0-SNAPSHOT</version>
                </dependency>
    
                <dependency>
                    <groupId>com.tecsys</groupId>
                    <artifactId>tecsys-meta-app</artifactId>
                    <version>2021.1.0-0-0-SNAPSHOT</version>
                </dependency>
    
                <dependency>
                    <groupId>com.tecsys</groupId>
                    <artifactId>tecsys-meta-web</artifactId>
                    <version>2021.1.0-0-0-SNAPSHOT</version>
                </dependency>
    
            </dependencies>
    
        </dependencyManagement>
    
        <build>
    
            <pluginManagement>
    
                <plugins>
    
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-clean-plugin</artifactId>
                        <version>${maven-clean-plugin.version}</version>
                        <executions>
                            <execution>
                                <id>auto-clean</id>
                                <phase>initialize</phase>
                                <goals>
                                    <goal>clean</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>
    
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-compiler-plugin</artifactId>
                        <version>${maven-compiler-plugin.version}</version>
                    </plugin>
    
                    <plugin>
                        <groupId>org.codehaus.mojo</groupId>
                        <artifactId>versions-maven-plugin</artifactId>
                        <version>${maven-versions-plugin.version}</version>
                        <configuration>
                            <excludes>
                                <exclude>org.apache.commons:commons-collections4</exclude>
                            </excludes>
                            <allowSnapshots>true</allowSnapshots>
                            <processParent>true</processParent>
                            <processDependencies>true</processDependencies>
                            <processDependencyManagement>true</processDependencyManagement>
                        </configuration>
                    </plugin>
    
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-enforcer-plugin</artifactId>
                        <version>${maven-enforcer-plugin.version}</version>
                        <executions>
                            <execution>
                                <id>enforce-versions</id>
                                <goals>
                                    <goal>enforce</goal>
                                </goals>
                                <configuration>
                                    <rules>
                                        <requireMavenVersion>
                                            <version>[3.6.3,)</version>
                                        </requireMavenVersion>
                                        <requireJavaVersion>
                                            <version>${java.version}</version>
                                        </requireJavaVersion>
                                        <requireReleaseDeps>
                                            <onlyWhenRelease>true</onlyWhenRelease>
                                            <message>
                                                Release builds must not have on snapshot dependencies
                                            </message>
                                        </requireReleaseDeps>
                                    </rules>
                                </configuration>
                            </execution>
                        </executions>
                    </plugin>
    
                </plugins>
    
            </pluginManagement>
    
        </build>
        
    </project>

  ```

### Resources
  - [github](https://github.com/yulikexuan/tecsys-mvn-lab)