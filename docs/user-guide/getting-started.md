# Getting Started

## Installation

## Basic Configuration

## Running the Gateway
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
