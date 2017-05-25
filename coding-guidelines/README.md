# Smartcat coding guidelines

## General rules
Inside this folder you can find formatter we use and checkstyle which should be enabled for all projects we do. Checkstyle automates lot of things we use, here we will outline guidelines which need further explaining.

## Test naming
Try to explain thing you test as good as you can in test name. Do not start with 'test' since it does not hold any value. Start with 'should' since it makes you really think what are you testing and you will end up with clear sentence explaining test case. Use underscores instead of camel case in test names.

## Checkstyle checks
In this folder there is [checkstyle.xml](checkstyle.xml) which provides ruleset we follow. It uses `SupressionFilter` module which references [checkstyle-suppressions.xml](checkstyle-suppressions.xml) which has exceptions for certain directories. Glue is [checkstyle.properties](checkstyle.properties) where you define `suppression_file` location. This is done this way since checkstyle maven plugin can resolve relative paths, and we provide default path to suppression file which is at same level as checkstyle.xml and using this variable you can override it in your favorite IDE (i.e. in eclipse in checkstyle properties you can add additional property `suppression_file` which will point to absolute path on your computer).

Maven plugin for checkstyle resolves path as relative, here is part of maven `pom.xml` which configures checkstyle plugin (assumption is that `checkstyle.xml`, `checkstyle-suppressions.xml` and `checkstyle.properties` are in root of project):

```
<!-- Check style on build. -->
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-checkstyle-plugin</artifactId>
	<version>${version.plugin.checkstyle}</version>
	<executions>
		<execution>
			<phase>validate</phase>
			<goals>
				<goal>check</goal>
			</goals>
			<configuration>
				<configLocation>checkstyle.xml</configLocation>
				<!-- Apply rules for test sources as well -->
				<includeTestSourceDirectory>true</includeTestSourceDirectory>
				<consoleOutput>true</consoleOutput>
				<failsOnError>true</failsOnError>
				<!-- Define path to checkstyle properties -->
				<propertiesLocation>checkstyle.properties</propertiesLocation>
				<excludes>**/generated/**/*</excludes>
			</configuration>
		</execution>
	</executions>
</plugin>
```
## Code formatter
Inside this directory you can find [smartcat-formatter.xml](smartcat-formatter.xml) we use on our projects.

## Releasing
Inside this directory you can find [release](release) script used for releasing of a new version.
Script represents basic idea and procedure for releasing, each project might have some specifics, but it can use this script and extend upon it.
