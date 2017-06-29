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



