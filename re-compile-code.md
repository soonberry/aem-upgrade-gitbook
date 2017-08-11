# Recompiling Code for AEM 6.3 Upgrade

The main work of recompiling is update dependencies in maven pom files.

### Uber Jar

From 6.1 to 6.3, the main change is support from AEM local compiling. AEM used to provide aem-api and a list of dependencies which needed to be involve in pom file for compiling. After version of 6.1, AEM integrates all the dependencies to uber-jar to reduce developer's effort. Each version of AEM has corresponding version of uber-jar.

For AEM 6.3, we use user-jar 6.3.0.

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.3.0</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

ACS also update the version. 2.x for 6.1 and 3.x for 6.2 or 6.3.

```xml
<dependency>
    <groupId>com.adobe.acs</groupId>
    <artifactId>acs-aem-commons-content</artifactId>
    <version>3.9.2</version>
    <type>content-package</type>
</dependency>

<dependency>
    <groupId>com.adobe.acs</groupId>
    <artifactId>acs-aem-commons-bundle</artifactId>
    <version>3.9.2</version>
</dependency>
```

### Remove duplicate dependencies

Uber-jar include most of necessary dependencies for AEM compiling, so after upgrade to uber-jar, need to remove dependencies in pom:

1. org.apache.sling.*
2. com.day.cq.\*
3. com.adobe.\*

However, be careful about removing these dependencies. 

For example, if remove com.day.cq.mailer in pom, need to involve commons-email dependency in specifically.

javax.inject is connected with geronimo-atinject\_1.0\_spec

```xml
<dependency>
	<groupId>org.apache.geronimo.specs</groupId>
  	<artifactId>geronimo-atinject_1.0_spec</artifactId>
    <version>1.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
	<groupId>javax.inject</groupId>
  	<artifactId>javax.inject</artifactId>
  	<version>1.0</version>
    <scope>provided</scope>
</dependency>
```

Particularly,  org.apache.sling.jcr.resource can only be removed until AEM 6.3 as uber-jar solve the dependency.

Do remember to check `<scope>` and `<version>`  for each dependency. Maybe some of the java class which passed in local but failed on AEM instance is caused by the scope or version conflict.

### Unit Test

For unit test, to mock AEM APIs, use aem-mock for different version depnending on AEM version.

- AEM 6.0 or 6.1: use AEM Mock 1.x
- AEM 6.2 or above: use AEM Mock 2.x

```xml
<dependency>
  <groupId>io.wcm</groupId>
  <artifactId>io.wcm.testing.aem-mock</artifactId>
  <version>2.2.6</version>
  <scope>test</scope>
</dependency
```
But in our project we didn't apply this mock because of legacy code will fail.

### Profile

Profile should be declared in each child pom file and will be triggered by parameters.

6.1 and 6.3 specific dependencies should be manage by their own profile. The commons use dependencies can be put outside.

All the dependencies are declared in `<dependencyManagement>` in parent pom. If one dependency has two different versions, should be declared too.

```xml
<dependency>
  <groupId>com.adobe.aem</groupId>
  <artifactId>uber-jar</artifactId>
  <version>6.1.0, 6.3.0</version>
  <scope>provided</scope>
</dependency>
```
In our project, 6.1 profile is set to default activation.

```xml
<profiles>
  <profile>
    <id>aemVersion-61</id>
    <activation>
      <activeByDefault>true</activeByDefault>
    </activation>
    <dependencies>
          <dependency>
              <groupId>com.adobe.aem</groupId>
              <artifactId>uber-jar</artifactId>
              <version>6.1.0</version>
              <scope>provided</scope>
          </dependency>
    </dependencies>
  </profile>
  <profile>
    <id>aemVersion-63</id>
    <dependencies>
          <dependency>
              <groupId>com.adobe.aem</groupId>
              <artifactId>uber-jar</artifactId>
              <version>6.3.0</version>
              <scope>provided</scope>
          </dependency>
    </dependencies>
  </profile>
</profiles>
```

