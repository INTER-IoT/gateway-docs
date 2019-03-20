# Device Controller Extension Development

As explained in the [introduction](../index.md) the Device Controller are physical gateway extensions
specifically designed to add support for new device protocols.

The following guide will show you how to create a basic device controller extension that you can use as a template to create your own.

Requirements:

 - [JDK1.8+](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
 - [Maven3+](https://maven.apache.org/install.html)
 - Installation of the physical and virtual part of the gateway to test your controller, following the [user guide](../user-guide/getting-started.md#installation)

## Generate a basic device controller example

Follow these steps in order to create an example device controller extension:
 
 - Create a directory with the name of your device controller project.
 - In a command console, `cd` to the directory where your project will reside and execute the following command: `mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate -DarchetypeCatalog=http://nexus.inter-iot.eu/repository/maven-releases/archetype-catalog.xml`*.
 - Select the desired version of the gateway that you will develop the extension for.
 - Type in your groupId, artifactId, version and package (optional, defaults to groupId) of your project when the interactive tool asks for them.
 - The project will be generated in your workspace and you can open it as a maven project with your IDE.

*\*change `maven-releases` to `maven-snapshots` in the url if you want to create a extension project for a snapshot version of the gateway*

## Structure of the project

The project will have the following structure:

```text
<artifactId>
├── pom.xml
├── .gitignore
└── src/
    ├── deploy/
    │   ├── conf.d/
    │   │   └── 00.<artifactId>.properties
    │   └── device-examples/
    │       └── <artifactId>-device-01.json.example
    └── main/
        └── java/
            └── <package>/
                ├── Activator.java
                └── ExampleDeviceController.java
```

 - The `pom.xml` file is your maven "[Project Object Model](https://maven.apache.org/pom.html)" containing all the details that maven needs to compile your project. Check in the [developer side notes](side-notes.md#extension-pom) for more details about the interesting parts of this pom.
 - The `src/deploy` folder contains files that will be unpacked in the physical gateway once the extension is installed, such as configuration properties and device examples, see [this section](extension-development.md) for further details.
 - The `main/java/<package>` folder is where the actual Java code resides, as explained in the [extension development](extension-development.md) common documentation, here is where the Activator and other classes of the extension will reside.

## Creating your own device controller

