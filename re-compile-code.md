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

For example, if remove com.day.cq.mailer in pom, need to involve mail dependency in specifically.

Particularly,  org.apache.sling.jcr.resource can only be removed until AEM 6.3 as uber-jar solve the dependency.

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