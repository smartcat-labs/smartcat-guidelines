# Smartcat coding guidelines

## General rules
Inside this folder you can find formatter we use and checkstyle which should be enabled for all projects we do. Checkstyle automates lot of things we use, here we will outline guidelines which need further explaining.

## Test naming
Try to explain thing you test as good as you can in test name. Do not start with 'test' since it does not hold any value. Start with 'should' since it makes you really think what are you testing and you will end up with clear sentence explaining test case. Use underscores instead of camel case in test names.

## Checkstyle checks
In this folder there is (checkstyle.xml)[https://github.com/smartcat-labs/smartcat-guidelines/blob/master/coding-guidelines/checkstyle.xml] which provides ruleset we follow. It uses `SupressionFilter` module which references (checkstyle-suppressions.xml)[https://github.com/smartcat-labs/smartcat-guidelines/blob/master/coding-guidelines/checkstyle-suppressions.xml] which has exceptions for certain directories. Reference uses `config_loc` variable, it can be used by IDE to point to absolute path of suppression file. IDE plugins for checkstyle use absolute path here, that is reason why `config_loc` is necessary to define per project absolute path prefix.

Maven plugin for checkstyle does not have `config_loc` variable, so you must define it. Here is part of maven `pom.xml` which configures checkstyle plugin (assumption is that both `checkstyle.xml` and checkstyle-suppressions.xml` are in root of project):

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
				<suppressionsFileExpression>checkstyle.suppression.file</suppressionsFileExpression>
				<!-- Apply rules for test sources as well -->
				<includeTestSourceDirectory>true</includeTestSourceDirectory>
				<consoleOutput>true</consoleOutput>
				<failsOnError>true</failsOnError>
				<!-- Define config_loc variable to point to place where suppression file is located-->
				<propertyExpansion>config_loc=./</propertyExpansion>
				<excludes>**/generated/**/*</excludes>
			</configuration>
		</execution>
	</executions>
</plugin>
```
##Code formatter
Inside this directory you can find (smartcat-formatter.xml)[https://github.com/smartcat-labs/smartcat-guidelines/blob/master/coding-guidelines/smartcat-formatter.xml] we use on our projects.

