# Tycho KNIME node archetype

[![Build Status Travis-CI](https://travis-ci.org/3D-e-Chem/tycho-knime-node-archetype.svg?branch=master)](https://travis-ci.org/3D-e-Chem/tycho-knime-node-archetype)
[![Build status AppVeyor](https://ci.appveyor.com/api/projects/status/70whq4bsdl0oq94m?svg=true)](https://ci.appveyor.com/project/3D-e-Chem/tycho-knime-node-archetype)
[![Download](https://api.bintray.com/packages/nlesc/tycho-knime-node-archetype/tycho-knime-node-archetype/images/download.svg) ](https://bintray.com/nlesc/tycho-knime-node-archetype/tycho-knime-node-archetype/_latestVersion)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.597989.svg)](https://doi.org/10.5281/zenodo.597989)

Generates [KNIME](http://www.knime.org) workflow node skeleton repository with sample code.

This archetype was made because the instructions to create KNIME nodes at https://www.knime.com/developer-guide, requires interaction with Eclipse wizards. We wanted a way to start and perform node development from the command line and headless.
KNIME nodes are Eclipse plugins. The [Tycho](https://eclipse.org/tycho/) Maven plugin is used to build and handle dependencies of Eclipse plugins, so we use Tycho for KNIME node building.

The [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) will generate a multi-module project with the following structure:

* / - parent Maven project
* /plugin/ - code for KNIME node
* /tests/ - tests of KNIME node
* /feature/ - eclipse feature
* /p2/ - eclipse update site

## Requirements

* Java >=1.8
* Maven >=3.0

The archetype is hosted on a [BinTray repository](https://dl.bintray.com/nlesc/tycho-knime-node-archetype).
Maven does not resolve to this BinTray repository by default so it must be added.

The [~/.m2/settings.xml](https://maven.apache.org/settings.html) should contain the following profile:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <profiles>
    <profile>
      <id>knimearchetype</id>
      <repositories>
        <repository>
          <id>archetype</id>
          <url>https://dl.bintray.com/nlesc/tycho-knime-node-archetype</url>
        </repository>
      </repositories>
    </profile>
  </profiles>
</settings>
```

## Generate

The following command will generate a skeleton project
```sh
mvn archetype:generate -DarchetypeGroupId=nl.esciencecenter \
-DarchetypeArtifactId=tycho-knime-node-archetype \
-DarchetypeVersion=1.7.0 -P knimearchetype
```

The command will ask the following questions:

1. Enter the groupId
2. Enter the artifactId
3. Enter the name of the package under which your code will be created
4. Enter the version of your project, use `x.y.z-SNAPSHOT` format (for example `1.2.3-SNAPSHOT`), where x.y.z is [semantic versioning](http://semver.org/).
5. Enter the GitHub organization name or GitHub username
6. Enter the GitHub repository name
7. Enter the KNIME node name
8. Enter the vendor name
9. For branded property enter `Y` to enable the 3D-e-Chem project branding like splash logo, node category and update site category or enter `N` to skip the branding.
9. Confirm

The skeleton has been generated in a sub-directory named after the artifactId in the current working directory.

The following steps are needed to get a ready to use project.

9. Change directory to generated code.
10. Make skeleton git aware, by running `git init`.
11. Fill in all placeholders (`[Enter ... here.]`) in

    * plugin/src/**/*.xml
    * feature/feature.xml

12. Commit all changes and push to GitHub
13. Optionally, setup Continuous Integration as described in the project README.md file.

Further instructions about generated project can be found in it's README.md file.

## Generate from inside KNIME SDK

1. Start up the KNIME SDK
2. Install the `m2e - Maven Integration for Eclipse` software, (you might need to add the add the Eclipse software site which is `http://download.eclipse.org/releases/neon` for the version 3.5.1 of the KNIME SDK)
3. Register the archetype catalog which contains this archetype

      1. Goto Window > Preferences > Maven > Archetypes
      2. Add a remove catalog with `https://github.com/3D-e-Chem/tycho-knime-node-archetype/raw/master/archetype-catalog.xml`

4. Create a new project based on archetype

      1. Goto File > New > Project ... > Maven
      2. Select Maven Project & press Next button
      3. Use default location & press Next button
      4. Select Catalog you added in 3.2
      5. Select the archetype with artifact id `tycho-knime-node-archetype` & press Next button
      6. Fill in the form & press Finish button

Further instructions about generated project can be found in it's README.md file.

## New release

1. Adjust version in pom.xml
2. Update CHANGELOG.md & README.md & archetype-catalog.xml & CITATION.cff
3. Commit & push
4. Test archetype by running `mvn verify`
5. Create GitHub release
6. Deploy to Bintray, see Deploy chapter below

### Deploy

To deploy current version to Bintray.

1. Add bintray API key to [~/.m2/settings.xml](https://maven.apache.org/settings.html)

```
<servers>
  <server>
    <id>bintray-nlesc-tycho-knime-node-archetype</id>
    <username>************</username>
    <password>********************************</password>
  </server>
<servers>
```

2. Run `mvn deploy`

## Attribution

The https://github.com/open-archetypes/tycho-eclipse-plugin-archetype was used as inspiration for this archetype.
