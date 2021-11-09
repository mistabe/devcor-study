# Infrastructure and Automation

## Heading 2

### 5.1 Explain considerations of model-driven telemetry (including data consumption and data storage)

Telemetry has the notions of;

- Subscriber
- Publisher
- Receiver (or collector)

IOS-XE has two types of subscritions, _dynamic_ and _configured_. Dynamic are created by third parties connecting to the device with the correct authorisation. Configured are exactly that - in the running configuration and will persist until removed from the configuration.

It is important to apply proper filters (as specific as possible) and review the frequency of periodic updates.

A receiver would be a combination of Telegraf, InfluxDB, and Grafana, and another would be Kafka + ELK stack.

### 5.2 Utilize RESTCONF to configure a network device, including interfaces, static routes, and VLANs (IOS XE only)

### 5.3 Construct a workflow to configure network parameters with

#### 5.3.A Ansible playbook

#### 5.3.B Puppet manifest

### 5.4 Identify a configuration management solution to achieve technical/business requirements

### 5.5 Describe how to host an application on a network device (including Catalyst 9000 and Cisco IOx–enabled devices)

Application hosting is generally supported on Catalyst 9000, ISR 4K, CSR1000v, and ASR1K platforms;

Application hosting is designed in a way to prevent performance and security impact on the main system:

- Memory and CPU usage for applications is bounded using control groups (cgroups).

- Process and file access is isolated and restricted (using user namespace).

- Disk usage is isolated using separate storage.

Two major requirements for application hostin on the Cat9K series;

- External storage. Two options are available, depending on the platform: USB 3.0 or SATA SSD drives.

- DNA-Advantage licensing.

Support is only for Docker containers.

Global config command to enable the IOx subsystem;

`cat9k(config)# iox`

Subsequently, the command to drive app management in enable mode is

`cat9k# app-hosting <command>`

Commands available are;

install
activate
start
stop
deactivate
uninstall
