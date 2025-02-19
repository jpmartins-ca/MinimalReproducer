# MinimalReproducer


After significant efforts, stripped it down to a minimal replicator.

This is just an empty project to demonstrate the issue with dependency-check-maven usage I am forced on doing by my company rules. 

Do check the pom.xml and do verify:

`mvn org.owasp:dependency-check-maven:RELEASE:aggregate`

Or if you have a NVD_API_KEY env var the faster:

`mvn org.owasp:dependency-check-maven:RELEASE:aggregate -DnvdApiKey=${NVD_API_KEY}`

And you will get a lot of them, including the:
...
[ERROR] maven-core-3.6.3.jar (pkg:maven/org.apache.maven/maven-core@3.6.3, cpe:2.3:a:apache:maven:3.6.3:*:*:*:*:*:*:*): CVE-2021-26291(9.1)
...

The "apparently" miss-leading [dependency-check-report.html](target/site/dependency-check/dependency-check-report.html) says:

    maven-core-3.6.3.jar
    Referenced In Project/Scope: test (plugins)
    Included by: pkg:maven/org.owasp/dependency-check-maven@12.1.0 (plugins)

The issue originates from the company imposed (huge) parent pom with mandatory, with configuration:

    <failBuildOnCVSS>7</failBuildOnCVSS>
    <scanPlugins>true</scanPlugins>

There is no explicit reference to maven-core 3.6.3 anywhere on my company parent pom or its dependencies. The following returns nothing:

```bash
mvn help:effective-pom | grep 3.6.3
```



