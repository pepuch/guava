# Guava migration to Java9

## Environment:

- java:
```
java 9
Java(TM) SE Runtime Environment (build 9+181)
Java HotSpot(TM) Server VM (build 9+181, mixed mode)
```

- maven
```
Apache Maven 3.5.0 (ff8f5e7444045639af65f6095c62210b5713f426; 2017-04-03T21:39:06+02:00)
Maven home: /opt/apache-maven-3.5.0
Java version: 9, vendor: Oracle Corporation
Java home: /home/patryk/Applications/jdk-9
Default locale: pl_PL, platform encoding: UTF-8
OS name: "linux", version: "4.4.0-53-generic", arch: "i386", family: "unix"
```

## Migration stages

### Compile in java 9 without code changes or as little as possible

1. Compile without running unit tests (`mvn clean instsall -DskipTests`)

Result: failure

Details:
```
...
[INFO] --- maven-javadoc-plugin:2.10.4:jar (attach-docs) @ guava ---
[WARNING] Error injecting: org.apache.maven.plugin.javadoc.JavadocJar
java.lang.ExceptionInInitializerError
	at org.apache.maven.plugin.javadoc.AbstractJavadocMojo.<clinit>(AbstractJavadocMojo.java:195)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:488)
	at com.google.inject.internal.DefaultConstructionProxyFactory$1.newInstance(DefaultConstructionProxyFactory.java:86)
	at com.google.inject.internal.ConstructorInjector.provision(ConstructorInjector.java:105)
	at com.google.inject.internal.ConstructorInjector.access$000(ConstructorInjector.java:32)
	at com.google.inject.internal.ConstructorInjector$1.call(ConstructorInjector.java:89)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:115)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:133)
	at com.google.inject.internal.ProvisionListenerStackCallback.provision(ProvisionListenerStackCallback.java:68)
	at com.google.inject.internal.ConstructorInjector.construct(ConstructorInjector.java:87)
	at com.google.inject.internal.ConstructorBindingImpl$Factory.get(ConstructorBindingImpl.java:267)
	at com.google.inject.internal.InjectorImpl$2$1.call(InjectorImpl.java:1016)
	at com.google.inject.internal.InjectorImpl.callInContext(InjectorImpl.java:1103)
	at com.google.inject.internal.InjectorImpl$2.get(InjectorImpl.java:1012)
	at com.google.inject.internal.InjectorImpl.getInstance(InjectorImpl.java:1051)
	at org.eclipse.sisu.space.AbstractDeferredClass.get(AbstractDeferredClass.java:48)
	at com.google.inject.internal.ProviderInternalFactory.provision(ProviderInternalFactory.java:81)
	at com.google.inject.internal.InternalFactoryToInitializableAdapter.provision(InternalFactoryToInitializableAdapter.java:53)
	at com.google.inject.internal.ProviderInternalFactory$1.call(ProviderInternalFactory.java:65)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:115)
	at com.google.inject.internal.ProvisionListenerStackCallback$Provision.provision(ProvisionListenerStackCallback.java:133)
	at com.google.inject.internal.ProvisionListenerStackCallback.provision(ProvisionListenerStackCallback.java:68)
	at com.google.inject.internal.ProviderInternalFactory.circularGet(ProviderInternalFactory.java:63)
	at com.google.inject.internal.InternalFactoryToInitializableAdapter.get(InternalFactoryToInitializableAdapter.java:45)
	at com.google.inject.internal.InjectorImpl$2$1.call(InjectorImpl.java:1016)
	at com.google.inject.internal.InjectorImpl.callInContext(InjectorImpl.java:1092)
	at com.google.inject.internal.InjectorImpl$2.get(InjectorImpl.java:1012)
	at org.eclipse.sisu.inject.Guice4$1.get(Guice4.java:162)
	at org.eclipse.sisu.inject.LazyBeanEntry.getValue(LazyBeanEntry.java:81)
	at org.eclipse.sisu.plexus.LazyPlexusBean.getValue(LazyPlexusBean.java:51)
	at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:263)
	at org.codehaus.plexus.DefaultPlexusContainer.lookup(DefaultPlexusContainer.java:255)
	at org.apache.maven.plugin.internal.DefaultMavenPluginManager.getConfiguredMojo(DefaultMavenPluginManager.java:519)
	at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:121)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:208)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:154)
	at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:146)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:117)
	at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:81)
	at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:51)
	at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:128)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:309)
	at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:194)
	at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:107)
	at org.apache.maven.cli.MavenCli.execute(MavenCli.java:993)
	at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:345)
	at org.apache.maven.cli.MavenCli.main(MavenCli.java:191)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:564)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
	at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
	at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
	at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
Caused by: java.lang.StringIndexOutOfBoundsException: begin 0, end 3, length 1
	at java.base/java.lang.String.checkBoundsBeginEnd(String.java:3116)
	at java.base/java.lang.String.substring(String.java:1885)
	at org.apache.commons.lang.SystemUtils.getJavaVersionAsFloat(SystemUtils.java:1133)
	at org.apache.commons.lang.SystemUtils.<clinit>(SystemUtils.java:818)
	... 58 more
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] Guava Maven Parent ................................. SUCCESS [  0.613 s]
[INFO] Guava: Google Core Libraries for Java .............. FAILURE [ 39.829 s]
[INFO] Guava Testing Library .............................. SKIPPED
[INFO] Guava Unit Tests ................................... SKIPPED
[INFO] Guava GWT compatible libs .......................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 41.831 s
[INFO] Finished at: 2017-09-04T18:56:16+02:00
[INFO] Final Memory: 30M/102M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-javadoc-plugin:2.10.4:jar (attach-docs) on project guava: Execution attach-docs of goal org.apache.maven.plugins:maven-javadoc-plugin:2.10.4:jar failed: An API incompatibility was encountered while executing org.apache.maven.plugins:maven-javadoc-plugin:2.10.4:jar: java.lang.ExceptionInInitializerError: null
[ERROR] -----------------------------------------------------
[ERROR] realm =    plugin>org.apache.maven.plugins:maven-javadoc-plugin:2.10.4
[ERROR] strategy = org.codehaus.plexus.classworlds.strategy.SelfFirstStrategy
[ERROR] urls[0] = file:/home/patryk/.m2/repository/org/apache/maven/plugins/maven-javadoc-plugin/2.10.4/maven-javadoc-plugin-2.10.4.jar
[ERROR] urls[1] = file:/home/patryk/.m2/repository/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.jar
[ERROR] urls[2] = file:/home/patryk/.m2/repository/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.jar
[ERROR] urls[3] = file:/home/patryk/.m2/repository/commons-cli/commons-cli/1.2/commons-cli-1.2.jar
[ERROR] urls[4] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-interactivity-api/1.0-alpha-4/plexus-interactivity-api-1.0-alpha-4.jar
[ERROR] urls[5] = file:/home/patryk/.m2/repository/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.jar
[ERROR] urls[6] = file:/home/patryk/.m2/repository/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.jar
[ERROR] urls[7] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.jar
[ERROR] urls[8] = file:/home/patryk/.m2/repository/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.jar
[ERROR] urls[9] = file:/home/patryk/.m2/repository/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.jar
[ERROR] urls[10] = file:/home/patryk/.m2/repository/org/apache/maven/maven-archiver/2.5/maven-archiver-2.5.jar
[ERROR] urls[11] = file:/home/patryk/.m2/repository/org/apache/maven/shared/maven-invoker/2.2/maven-invoker-2.2.jar
[ERROR] urls[12] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.jar
[ERROR] urls[13] = file:/home/patryk/.m2/repository/org/apache/maven/shared/maven-common-artifact-filters/1.3/maven-common-artifact-filters-1.3.jar
[ERROR] urls[14] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-sink-api/1.4/doxia-sink-api-1.4.jar
[ERROR] urls[15] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-logging-api/1.4/doxia-logging-api-1.4.jar
[ERROR] urls[16] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-site-renderer/1.4/doxia-site-renderer-1.4.jar
[ERROR] urls[17] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-core/1.4/doxia-core-1.4.jar
[ERROR] urls[18] = file:/home/patryk/.m2/repository/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1.jar
[ERROR] urls[19] = file:/home/patryk/.m2/repository/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04.jar
[ERROR] urls[20] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-decoration-model/1.4/doxia-decoration-model-1.4.jar
[ERROR] urls[21] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-module-xhtml/1.4/doxia-module-xhtml-1.4.jar
[ERROR] urls[22] = file:/home/patryk/.m2/repository/org/apache/maven/doxia/doxia-module-fml/1.4/doxia-module-fml-1.4.jar
[ERROR] urls[23] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-i18n/1.0-beta-7/plexus-i18n-1.0-beta-7.jar
[ERROR] urls[24] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-velocity/1.1.7/plexus-velocity-1.1.7.jar
[ERROR] urls[25] = file:/home/patryk/.m2/repository/org/apache/velocity/velocity/1.5/velocity-1.5.jar
[ERROR] urls[26] = file:/home/patryk/.m2/repository/oro/oro/2.0.8/oro-2.0.8.jar
[ERROR] urls[27] = file:/home/patryk/.m2/repository/org/apache/velocity/velocity-tools/2.0/velocity-tools-2.0.jar
[ERROR] urls[28] = file:/home/patryk/.m2/repository/commons-beanutils/commons-beanutils/1.7.0/commons-beanutils-1.7.0.jar
[ERROR] urls[29] = file:/home/patryk/.m2/repository/commons-digester/commons-digester/1.8/commons-digester-1.8.jar
[ERROR] urls[30] = file:/home/patryk/.m2/repository/commons-chain/commons-chain/1.1/commons-chain-1.1.jar
[ERROR] urls[31] = file:/home/patryk/.m2/repository/commons-validator/commons-validator/1.3.1/commons-validator-1.3.1.jar
[ERROR] urls[32] = file:/home/patryk/.m2/repository/dom4j/dom4j/1.1/dom4j-1.1.jar
[ERROR] urls[33] = file:/home/patryk/.m2/repository/sslext/sslext/1.2-0/sslext-1.2-0.jar
[ERROR] urls[34] = file:/home/patryk/.m2/repository/org/apache/struts/struts-core/1.3.8/struts-core-1.3.8.jar
[ERROR] urls[35] = file:/home/patryk/.m2/repository/antlr/antlr/2.7.2/antlr-2.7.2.jar
[ERROR] urls[36] = file:/home/patryk/.m2/repository/org/apache/struts/struts-taglib/1.3.8/struts-taglib-1.3.8.jar
[ERROR] urls[37] = file:/home/patryk/.m2/repository/org/apache/struts/struts-tiles/1.3.8/struts-tiles-1.3.8.jar
[ERROR] urls[38] = file:/home/patryk/.m2/repository/commons-collections/commons-collections/3.2.1/commons-collections-3.2.1.jar
[ERROR] urls[39] = file:/home/patryk/.m2/repository/commons-lang/commons-lang/2.4/commons-lang-2.4.jar
[ERROR] urls[40] = file:/home/patryk/.m2/repository/commons-io/commons-io/2.5/commons-io-2.5.jar
[ERROR] urls[41] = file:/home/patryk/.m2/repository/org/apache/httpcomponents/httpclient/4.2.3/httpclient-4.2.3.jar
[ERROR] urls[42] = file:/home/patryk/.m2/repository/org/apache/httpcomponents/httpcore/4.2.2/httpcore-4.2.2.jar
[ERROR] urls[43] = file:/home/patryk/.m2/repository/commons-codec/commons-codec/1.6/commons-codec-1.6.jar
[ERROR] urls[44] = file:/home/patryk/.m2/repository/commons-logging/commons-logging/1.1.1/commons-logging-1.1.1.jar
[ERROR] urls[45] = file:/home/patryk/.m2/repository/log4j/log4j/1.2.14/log4j-1.2.14.jar
[ERROR] urls[46] = file:/home/patryk/.m2/repository/com/thoughtworks/qdox/qdox/1.12.1/qdox-1.12.1.jar
[ERROR] urls[47] = file:/home/patryk/.m2/repository/junit/junit/3.8.1/junit-3.8.1.jar
[ERROR] urls[48] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-archiver/3.3/plexus-archiver-3.3.jar
[ERROR] urls[49] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-io/2.7.1/plexus-io-2.7.1.jar
[ERROR] urls[50] = file:/home/patryk/.m2/repository/org/apache/commons/commons-compress/1.11/commons-compress-1.11.jar
[ERROR] urls[51] = file:/home/patryk/.m2/repository/org/iq80/snappy/snappy/0.4/snappy-0.4.jar
[ERROR] urls[52] = file:/home/patryk/.m2/repository/org/tukaani/xz/1.5/xz-1.5.jar
[ERROR] urls[53] = file:/home/patryk/.m2/repository/org/codehaus/plexus/plexus-utils/3.0.24/plexus-utils-3.0.24.jar
[ERROR] Number of foreign imports: 1
[ERROR] import: Entry[import  from realm ClassRealm[project>com.google.guava:guava:24.0-SNAPSHOT, parent: ClassRealm[maven.api, parent: null]]]
[ERROR] 
[ERROR] -----------------------------------------------------
[ERROR] : begin 0, end 3, length 1
[ERROR] -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginContainerException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <goals> -rf :guava
```

Exception analysis:
Exception contains one important line `org.apache.commons.lang.SystemUtils.getJavaVersionAsFloat(SystemUtils.java:1133)`. It 
looks like problem with parsing java version that has been changed. Java 9 introduced new version-string scheme (http://openjdk.java.net/jeps/223).
Luckily it has been fixed in 2.10.4 version (https://issues.apache.org/jira/browse/MJAVADOC-442) but guava already uses this version.

Solution: 
Try to upgrade javadoc dependency to the latest version 3.0.0-M1

After compiling again (`mvn clean instasll -DskipTests`) maven returns another error:
```
[INFO] --- gwt-maven-plugin:2.8.0:compile (gwt-compile) @ guava-gwt ---
[INFO] [ERROR] Hint: Check that your module inherits 'com.google.gwt.core.Core' either directly or indirectly (most often by inheriting module 'com.google.gwt.user.User')
...
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:31 min
[INFO] Finished at: 2017-09-04T19:02:25+02:00
[INFO] Final Memory: 73M/243M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.codehaus.mojo:gwt-maven-plugin:2.8.0:compile (gwt-compile) on project guava-gwt: Command [[
[ERROR] /bin/sh -c '/home/patryk/Applications/jdk-9/bin/java' '-Xmx512m' 'com.google.gwt.dev.Compiler' '-logLevel' 'WARN' '-war' '/home/patryk/Projects/guava-pepuch/guava-gwt/target/guava-gwt-24.0-SNAPSHOT' '-localWorkers' '4' '-validateOnly' '-failOnError' '-XfragmentCount' '-1' '-sourceLevel' '1.8' '-gen' '/home/patryk/Projects/guava-pepuch/guava-gwt/target/.generated' 'com.google.common.ForceGuavaCompilation'
[ERROR] ]] failed with status 1
```

but javadoc works correctly as we see in result:
```
[INFO] --- maven-javadoc-plugin:3.0.0-M1:jar (attach-docs) @ guava ---
[INFO] 
Loading source files for package com.google.common...
Constructing Javadoc information...
Standard Doclet version 9
Building tree for all the packages and classes...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/annotations/Beta.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/annotations/GwtCompatible.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/annotations/GwtIncompatible.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/annotations/VisibleForTesting.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/base/Ascii.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/base/CaseFormat.html...
Generating /home/patryk/Projects/guava-pepuch/guava/target/apidocs/com/google/common/base/CharMatcher.html...
...
```

Regarding guava gwt problem - I'm not and expert in gwt so let's focus only on building guava library (we can back to it in future).
After running `mvn clean install -DskipTests` in `guava` subdirectory build succeeded!
 
2. Compile with unit tests (`mvn clean install`)

# Guava: Google Core Libraries for Java

[![Build Status](https://travis-ci.org/google/guava.svg?branch=master)](https://travis-ci.org/google/guava)

Guava is a set of core libraries that includes new collection types (such as
multimap and multiset), immutable collections, a graph library, functional
types, an in-memory cache, and APIs/utilities for concurrency, I/O, hashing,
primitives, reflection, string processing, and much more!

Guava comes in two flavors.

*   The main flavor requires JDK 1.8 or higher.
*   If you need support for JDK 1.7 or Android, use the Android flavor. You can
    find the Android Guava source in the [`android` directory].

[`android` directory]: https://github.com/google/guava/tree/master/android

## Latest release

The most recent release is [Guava 23.0][current release], released August 4, 2017.

- 23.0 API Docs: [guava][guava-release-api-docs], [guava-testlib][testlib-release-api-docs]
- 23.0 API Diffs from 22.0: [guava][guava-release-api-diffs]

The Maven group ID is `com.google.guava`, and the artifact ID is `guava`. Use
version `23.0` for the main flavor, or `23.0-android` for the Android flavor.

To add a dependency on Guava using Maven, use the following:

```xml
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>23.0</version>
  <!-- or, for Android: -->
  <version>23.0-android</version>
</dependency>
```

To add a dependency using Gradle:

```
dependencies {
  compile 'com.google.guava:guava:23.0'
  // or, for Android:
  compile 'com.google.guava:guava:23.0-android'
}
```

## Snapshots

Snapshots of Guava built from the `master` branch are available through Maven
using version `24.0-SNAPSHOT`, or `24.0-android-SNAPSHOT` for the Android
flavor.

- Snapshot API Docs: [guava][guava-snapshot-api-docs]
- Snapshot API Diffs: [guava][guava-snapshot-api-diffs]

## Learn about Guava

- Our users' guide, [Guava Explained][]
- [A nice collection](http://www.tfnico.com/presentations/google-guava) of other helpful links

## Links

- [GitHub project](https://github.com/google/guava)
- [Issue tracker: Report a defect or feature request](https://github.com/google/guava/issues/new)
- [StackOverflow: Ask "how-to" and "why-didn't-it-work" questions](https://stackoverflow.com/questions/ask?tags=guava+java)
- [guava-discuss: For open-ended questions and discussion](http://groups.google.com/group/guava-discuss)

## IMPORTANT WARNINGS

1. APIs marked with the `@Beta` annotation at the class or method level
are subject to change. They can be modified in any way, or even
removed, at any time. If your code is a library itself (i.e. it is
used on the CLASSPATH of users outside your own control), you should
not use beta APIs, unless you repackage them (e.g. using ProGuard).

2. Deprecated non-beta APIs will be removed two years after the
release in which they are first deprecated. You must fix your
references before this time. If you don't, any manner of breakage
could result (you are not guaranteed a compilation error).

3. Serialized forms of ALL objects are subject to change unless noted
otherwise. Do not persist these and assume they can be read by a
future version of the library.

4. Our classes are not designed to protect against a malicious caller.
You should not use them for communication between trusted and
untrusted code.

5. For the mainline flavor, we unit-test the libraries using only OpenJDK 1.8 on
Linux. Some features, especially in `com.google.common.io`, may not work
correctly in other environments.

  For the Android flavor, our unit tests run on API level 10 (Gingerbread).

[current release]: https://github.com/google/guava/wiki/Release23
[guava-release-api-docs]: http://google.github.io/guava/releases/23.0/api/docs/
[testlib-release-api-docs]: http://www.javadoc.io/doc/com.google.guava/guava-testlib/23.0
[guava-release-api-diffs]: http://google.github.io/guava/releases/23.0/api/diffs/
[guava-snapshot-api-docs]: http://google.github.io/guava/releases/snapshot/api/docs/
[guava-snapshot-api-diffs]: http://google.github.io/guava/releases/snapshot/api/diffs/
[Guava Explained]: https://github.com/google/guava/wiki/Home
