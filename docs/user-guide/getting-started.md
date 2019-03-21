# Getting Started

## Installation

### Prerequisites

 - Java 1.8 ([JRE](https://www.oracle.com/technetwork/es/java/javase/downloads/jre8-downloads-2133155.html) or [JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) if you want to develop extensions)
 - [Download](https://github.com/INTER-IoT/gateway/releases) and unpack the latest version of the Physical or Virtual Gateway. You can also [downlad and compile it from the source code](advanced-documentation#compiling-from-source) if you prefer.

### Installation files and folder structure

```text
(physical|virtual)-gateway
├── bin/
│   ├── run
│   ├── run.cmd
├── cache/
|   └── (cache files...)
├── certs/
|   └── (certificate files...)
├── conf.d/
|   └── (configuration files...)
├── core/
|   └── (core OSGi bundles...)
├── extensions/
|   └── (extension OSGi bundles...)
├── lib/
|   └── (common lib OSGi bundles...)
├── framework/
|   └── (main jars added to classpath...)
├── .uuid
└── (other/specific physical and virtual files/folders)
```

 - Main executables are found under `bin/` folder, these are `bin\run` for linux/MacOS and `bin\run.cmd` for Windows.
  - `conf.d/` folder contains configuration files, covered in the [next topic](getting-started.md#basic-configuration)
  - `certs/` folder contains certificate files for peer authentication, more information available in the [advanced documentation section](advanced-documentation.md#peer-authentication)
  - `core/`, `extensions/`, `lib/` and `framework/` contain the core and extension modules of the gateway. Will not be covered in the user guide but more information is available in the [developer guide](../developer-guide/extension-development.md)
  - `.uuid` is a file containinig a unique identifier for the gateway. It will be deprecated with the usage of peer authentication certificates.
  - Other files/folder such as device/rule definitions will be covered in the specific [physical]() and [virtual]() usage guide.

### Basic Configuration

 Either you are deploying the physical or virtual part of the Gateway, all configuration resides in the `conf.d/` directory. The configuration files
 are read sequentially following lexicographical order keeping the last value as of a configuration property, similar to udev or apache configuration.
 
 For that reason, it is recommended to follow these simple guidelines:

   - Configuration files are named with the following pattern: `<priority>.<component>.properties`. Being `<priority>` a number that forces the lexicographical order in a specific way and `<component>` a relevant name for the configuration file, normally a core module or an installed gateway extension.
   - Default configuration files start with priority `00` and are read first in order. Custom extensions should add default configuration files also starting with `00`.
   - User/Deployment specific configuration should start with `10`, `20`, etc. priority numbers in a single file.
 
 You can consult specific configuration for the physical gateway and virtual gateway in the advanced documentation [sections](advanced-documentation.md#physical-gateway-documentation).

## Running the Gateway

The following subsections are valid for both the physical and virtual gateway.

### Installing extensions
 
The physical and virtual part of the gateway are meant to be fully customizable for each deployment.
For that reason, they won't do anything if there are no extensions installed. 

There are 3 types of extensions: **physical gateway extensions**, **virtual gateway extensions** and **common extensions**:

 - **Physical gateway extensions** will be mainly device controllers and only work in the physical gateway environment.

 - **Common extensions** work in both the physical and virtual gateway and typically are utilities.

 - **Virtual gateway extensions** add functionality to the gateway that need more computing power and would be a burden if they are running in the physical gateway device. The functionalities range from middleware modules to connect to IoT platforms, server and REST API to manage and query the gateway, rules engine to create simple rules and scripting, etc.

To install extensions you have to download the zip file of the extension (you can try with some of our [supported extensions](supported-extensions.md)) and install them. Installing extensions is as simple as running the following command in the gateway distribution folder (either physical or virtual): `$ bin/run --install <path_to_extension_zip_file>`.

### Prerequisites

 - Java 1.8
 - Maven 3.3+
 - Docker (and docker-compose)
 - Configure correctly the PATH variable for all of them (windows)

### Compile
 - `mvn clean package` in the gateway root folder

### Run

#### Start MW (Orion for the moment)

 - Go to `<gateway root folder>/docker/mwplatforms/orion-for-gw`
 - Start it with `docker-compose up`
 - Stop it closing the command console (or ctrl+c several times) and then `docker-compose down` in the same folder
 - *\*This can be done once and keep it running always*

#### Virtual Part

 - The compiled version is in `<gateway root folder>/target/virtual` folder
 - Modify `configuration.properties` if needed (the default configuration is taken from `<gateway root folder>/src/physical/configuration.properties.example` and can be changed)
 - Start with `java -jar gateway.framework-0.0.1-SNAPSHOT` in a commandline console

#### Physical Part

 - The compiled version is in `<gateway root folder>/target/physical` folder
 - Modify `configuration.properties` if needed (the default configuration is taken from `<gateway root folder>/src/virtual/configuration.properties.example` and can be changed)
 - Configure devices in `<gateway root folder>/target/physical/devices>` folder ([example configuration](https://git.inter-iot.eu/Inter-IoT/gateway/wiki/Device+Configuration+File))
 - Start with `java -jar gateway.framework-0.0.1-SNAPSHOT` in a commandline console
 - In the gateway console there this commands available to test devices:
     - `device list` to show a list of devices connected
     - `device connect <device id>` to start the connection process
     - `device disconnect <device id>` to start the disconnection process
     - `device measurement <device id>` to get a measurement of that device
     - `device action <device id> <attributename>=<value> <attributename>=<value> ...` to push a set of values to the device actuators. The values have to end with i,f or b if they are integer, float or boolean. For example `device action dev123 light_intensity=58i`
