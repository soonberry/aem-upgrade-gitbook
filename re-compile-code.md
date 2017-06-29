# Recompiling

The main work of recompiling is update dependencies in maven pom files.

As in our project, we upgrade code from 6.0 to 6.1, so we keep aem-api 6.0.0.1 as basic dependency to support AEM compiling.

So first of all, need to replace aem-api to uber-jar.

```
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.3.0</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

ACS also update the version. 2.x for 6.1 and 3.x for 6.2 or 6.3.

```
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

Uber-jar include most of necessary dependencies for AEM compiling, so after upgrade to uber-jar, need to remove dependencies in pom:

1. org.apache.sling
2. com.day.cq.\*
3. com.adobe.\*

However, be careful about removing these dependencies. 



