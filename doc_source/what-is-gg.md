# What Is AWS IoT Greengrass?<a name="what-is-gg"></a>

AWS IoT Greengrass is software that extends cloud capabilities to local devices\. This enables devices to collect and analyze data closer to the source of information, react autonomously to local events, and communicate securely with each other on local networks\. Local devices can also communicate securely with AWS IoT Core and export IoT data to the AWS Cloud\. AWS IoT Greengrass developers can use AWS Lambda functions and prebuilt [connectors](connectors.md) to create serverless applications that are deployed to devices for local execution\.

The following diagram shows the basic architecture of AWS IoT Greengrass\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/greengrass.png)

AWS IoT Greengrass makes it possible for customers to build IoT devices and application logic\. Specifically, AWS IoT Greengrass provides cloud\-based management of application logic that runs on devices\. Locally deployed Lambda functions and connectors are triggered by local events, messages from the cloud, or other sources\.

In AWS IoT Greengrass, devices securely communicate on a local network and exchange messages with each other without having to connect to the cloud\. AWS IoT Greengrass provides a local pub/sub message manager that can intelligently buffer messages if connectivity is lost so that inbound and outbound messages to the cloud are preserved\.

AWS IoT Greengrass protects user data:
+ Through the secure authentication and authorization of devices\.
+ Through secure connectivity in the local network\.
+ Between local devices and the cloud\.

Device security credentials function in a group until they are revoked, even if connectivity to the cloud is disrupted, so that the devices can continue to securely communicate locally\.

AWS IoT Greengrass provides secure, over\-the\-air updates of Lambda functions\.

AWS IoT Greengrass consists of:
+ Software distributions
  + AWS IoT Greengrass Core software
  + AWS IoT Greengrass Core SDK
+ Cloud service
  + AWS IoT Greengrass API
+ Features
  + Lambda runtime
  + Shadows implementation
  + Message manager
  + Group management
  + Discovery service
  + Over\-the\-air update agent
  + Stream manager
  + Local resource access
  + Local machine learning inference
  + Local secrets manager
  + Connectors with built\-in integration with services, protocols, and software

## AWS IoT Greengrass Core Software<a name="gg-core-software"></a>

The AWS IoT Greengrass Core software provides the following functionality:<a name="ggc-software-features"></a>
+ Deployment and local execution of connectors and Lambda functions\.
+ Process data streams locally with automatic exports to the AWS Cloud\.
+ MQTT messaging over the local network between devices, connectors, and Lambda functions using managed subscriptions\.
+ MQTT messaging between AWS IoT and devices, connectors, and Lambda functions using managed subscriptions\.
+ Secure connections between devices and the cloud using device authentication and authorization\.
+ Local shadow synchronization of devices\. Shadows can be configured to sync with the cloud\.
+ Controlled access to local device and volume resources\.
+ Deployment of cloud\-trained machine learning models for running local inference\.
+ Automatic IP address detection that enables devices to discover the Greengrass core device\.
+ Central deployment of new or updated group configuration\. After the configuration data is downloaded, the core device is restarted automatically\.
+ Secure, over\-the\-air \(OTA\) software updates of user\-defined Lambda functions\.
+ Secure, encrypted storage of local secrets and controlled access by connectors and Lambda functions\.

AWS IoT Greengrass core instances are configured through AWS IoT Greengrass APIs that create and update AWS IoT Greengrass group definitions stored in the cloud\.

### AWS IoT Greengrass Core Versions<a name="ggc-versions"></a>

AWS IoT Greengrass provides several options for installing the AWS IoT Greengrass Core software, including tar\.gz download files, a quick start script, and `apt` installations on supported Debian platforms\. For more information, see [Install the AWS IoT Greengrass Core Software](install-ggc.md)\.

The following tabs describe what's new and changed in AWS IoT Greengrass Core software versions\.

------
#### [ GGC v1\.10 ]

1\.10\.0 \- Current version  
New features:  <a name="what-new-v1100"></a>
+ A stream manager that processes data streams locally and exports them to the AWS Cloud automatically\. This feature requires Java 8 on the Greengrass core device\. For more information, see [Manage Data Streams on the AWS IoT Greengrass Core](stream-manager.md)\.
+ A new Greengrass Docker application deployment connector that runs a Docker application on a core device\. For more information, see [Docker Application Deployment Connector](docker-app-connector.md)\.
+ A new IoT SiteWise connector that sends industrial device data from OPC\-UA servers to asset properties in AWS IoT SiteWise\. For more information, see [IoT SiteWise Connector](iot-sitewise-connector.md)\.
+ Lambda functions that run without containerization can access machine learning resources in the Greengrass group\. For more information, see [Access Machine Learning Resources from Lambda Functions](access-ml-resources.md)\.
+ Support for MQTT persistent sessions with AWS IoT\. For more information, see [MQTT Persistent Sessions with AWS IoT](gg-core.md#mqtt-persistent-sessions)\.
+ Local MQTT traffic can travel over a port other than the default port 8883\. For more information, see [Configure the MQTT Port for Local Messaging](gg-core.md#config-local-mqtt-port)\.
+ New `queueFullPolicy` options in the [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core) for reliable message publishing from Lambda functions\.
+ Support for running Node\.js 12\.x Lambda functions on the core\.
Bug fixes and improvements:  <a name="bug-fix-v1100"></a>
+ Over\-the\-air \(OTA\) updates with hardware security integration can be configured with OpenSSL 1\.1\.
+ General performance improvements and bug fixes\.

------
#### [ GGC v1\.9 ]

1\.9\.4  
Bug fixes and improvements:  
+ General performance improvements and bug fixes\.

1\.9\.3  
New features:  
+ <a name="what-new-v193-armv6l"></a>Support for Armv6l\. AWS IoT Greengrass Core software v1\.9\.3 or later can be installed on Raspbian distributions on Armv6l architectures \(for example, on Raspberry Pi Zero devices\)\.
+ <a name="what-new-v193-ota-alpn"></a>OTA updates on port 443 with ALPN\. Greengrass cores that use port 443 for MQTT traffic now support over\-the\-air \(OTA\) software updates\. AWS IoT Greengrass uses the Application Layer Protocol Network \(ALPN\) TLS extension to enable these connections\. For more information, see [OTA Updates of AWS IoT Greengrass Core Software](core-ota-update.md) and [Connect on Port 443 or Through a Network Proxy](gg-core.md#alpn-network-proxy)\.
Bug fixes and improvements:  
+ Fixes a bug introduced in v1\.9\.0 that prevented Python 2\.7 Lambda functions from sending binary payloads to other Lambda functions\.
+ General performance improvements and bug fixes\.

1\.9\.2  
New features:  
+ <a name="what-new-v192-openwrt"></a>Support for [OpenWrt](https://openwrt.org/)\. AWS IoT Greengrass Core software v1\.9\.2 or later can be installed on OpenWrt distributions with Armv8 \(AArch64\) and Armv7l architectures\. Currently, OpenWrt does not support ML inference\.

1\.9\.1  
Bug fixes and improvements:  
+ Fixes a bug introduced in v1\.9\.0 that drops messages from the `cloud` that contain wildcard characters in the topic\.

1\.9\.0  
New features:  
+ <a name="what-new-v190-runtimes"></a>Support for Python 3\.7 and Node\.js 8\.10 Lambda runtimes\. Lambda functions that use Python 3\.7 and Node\.js 8\.10 runtimes can now run on an AWS IoT Greengrass core\. \(AWS IoT Greengrass continues to support the Python 2\.7 and Node\.js 6\.10 runtimes\.\)
+ <a name="what-new-v190-mqtt-opt"></a>Optimized MQTT connections\. The Greengrass core establishes fewer connections with the AWS IoT Core\. This change can reduce operational costs for charges that are based on the number of connections\.
+ <a name="what-new-v190-ec-key"></a>Elliptic Curve \(EC\) key for the local MQTT server\. The local MQTT server supports EC keys in addition to RSA keys\. \(The MQTT server certificate has an SHA\-256 RSA signature, regardless of the key type\.\) For more information, see [AWS IoT Greengrass Core Security Principals](gg-sec.md#gg-principals)\.
Bug fixes and improvements:  
+ General performance improvements and bug fixes\.

------
#### [ GGC v1\.8 ]

1\.8\.4  
Fixed an issue with shadow synchronization and device certificate manager reconnection\.  
General performance improvements and bug fixes\.

1\.8\.3  
General performance improvements and bug fixes\.

1\.8\.2  
General performance improvements and bug fixes\.

1\.8\.1  
General performance improvements and bug fixes\.

1\.8\.0  
New features:  
+ Configurable default access identity for Lambda functions in the group\. This group\-level setting determines the default permissions that are used to run Lambda functions\. You can set the user ID, group ID, or both\. Individual Lambda functions can override the default access identity of their group\. For more information, see [Setting the Default Access Identity for Lambda Functions in a Group](lambda-group-config.md#lambda-access-identity-groupsettings)\.
+ HTTPS traffic over port 443\. HTTPS communication can be configured to travel over port 443 instead of the default port 8443\. This complements AWS IoT Greengrass support for the Application Layer Protocol Network \(ALPN\) TLS extension and allows all Greengrass messaging traffic—both MQTT and HTTPS—to use port 443\. For more information, see [Connect on Port 443 or Through a Network Proxy](gg-core.md#alpn-network-proxy)\.
+ Predictably named client IDs for AWS IoT connections\. This change enables support for AWS IoT Device Defender and [AWS IoT Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html), so you can receive notifications for connect, disconnect, subscribe, and unsubscribe events\. Predictable naming also makes it easier to create logic around connection IDs \(for example, to create [subscribe policy](https://docs.aws.amazon.com/iot/latest/developerguide/pub-sub-policy.html#pub-sub-policy-cert) templates based on certificate attributes\)\. For more information, see [Client IDs for MQTT Connections with AWS IoT](gg-core.md#connection-client-id)\.
Bug fixes and improvements:  
+ Fixed an issue with shadow synchronization and device certificate manager reconnection\.
+ General performance improvements and bug fixes\.

------
#### [ Deprecated versions ]

1\.7\.1  
New features:  
+ Greengrass connectors provide built\-in integration with local infrastructure, device protocols, AWS, and other cloud services\. For more information, see [Integrate with Services and Protocols Using Greengrass Connectors](connectors.md)\.
+ AWS IoT Greengrass extends AWS Secrets Manager to core devices, which makes your passwords, tokens, and other secrets available to connectors and Lambda functions\. Secrets are encrypted in transit and at rest\. For more information, see [Deploy Secrets to the AWS IoT Greengrass Core](secrets.md)\.
+ Support for a hardware root of trust security option\. For more information, see [Hardware Security Integration](hardware-security.md)\.
+ Isolation and permission settings that allow Lambda functions to run without Greengrass containers and to use the permissions of a specified user and group\. For more information, see [Controlling Execution of Greengrass Lambda Functions by Using Group\-Specific Configuration](lambda-group-config.md)\.
+ You can run AWS IoT Greengrass in a Docker container \(on Windows, macOS, or Linux\) by configuring your Greengrass group to run with no containerization\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.
+ MQTT messaging on port 443 with Application Layer Protocol Negotiation \(ALPN\) or connection through a network proxy\. For more information, see [Connect on Port 443 or Through a Network Proxy](gg-core.md#alpn-network-proxy)\.
+ The Amazon SageMaker Neo deep learning runtime, which supports machine learning models that have been optimized by the Amazon SageMaker Neo deep learning compiler\. For information about the Neo deep learning runtime, see [Runtimes and Precompiled Framework Libraries for ML Inference](ml-inference.md#precompiled-ml-libraries)\.
+ Support for Raspbian Stretch \(2018\-06\-27\) on Raspberry Pi core devices\.
Bug fixes and improvements:  
+ General performance improvements and bug fixes\.
In addition, the following features are available with this release:  
+ The AWS IoT Device Tester for AWS IoT Greengrass, which you can use to verify that your CPU architecture, kernel configuration, and drivers work with AWS IoT Greengrass\. For more information, see [Using AWS IoT Device Tester for AWS IoT Greengrass](device-tester-for-greengrass-ug.md)\.
+ The AWS IoT Greengrass Core software, AWS IoT Greengrass Core SDK, and AWS IoT Greengrass Machine Learning SDK packages are available for download through Amazon CloudFront\. For more information, see [AWS IoT Greengrass Downloads](#gg-downloads)\.

1\.6\.1  
New features:  
+ Lambda executables that run binary code on the Greengrass core\. Use the new AWS IoT Greengrass Core SDK for C to write Lambda executables in C and C\+\+\. For more information, see [Lambda Executables](lambda-functions.md#lambda-executables)\.
+ Optional local storage message cache that can persist across restarts\. You can configure the storage settings for MQTT messages that are queued for processing\. For more information, see [MQTT Message Queue for Cloud Targets](gg-core.md#mqtt-message-queue)\.
+ Configurable maximum reconnect retry interval for when the core device is disconnected\. For more information, see the `mqttMaxConnectionRetryInterval` property in [AWS IoT Greengrass Core Configuration File](gg-core.md#config-json)\.
+ Local resource access to the host /proc directory\. For more information, see [Access Local Resources with Lambda Functions and Connectors](access-local-resources.md)\.
+ Configurable write directory\. The AWS IoT Greengrass Core software can be deployed to read\-only and read\-write locations\. For more information, see [Configure a Write Directory for AWS IoT Greengrass](gg-core.md#write-directory)\.
Bug fixes and improvements:  
+ Performance improvement for publishing messages in the Greengrass core and between devices and the core\.
+ Reduced the compute resources required to process logs generated by user\-defined Lambda functions\.

1\.5\.0  
New features:  
+ AWS IoT Greengrass Machine Learning \(ML\) Inference is generally available\. You can perform ML inference locally on AWS IoT Greengrass devices using models that are built and trained in the cloud\. For more information, see [Perform Machine Learning Inference](ml-inference.md)\.
+ Greengrass Lambda functions now support binary data as input payload, in addition to JSON\. To use this feature, you must upgrade to AWS IoT Greengrass Core SDK version 1\.1\.0, which you can download from the [AWS IoT Greengrass Core SDK](#gg-core-sdk-download) downloads page\. 
Bug fixes and improvements:  
+ Reduced the overall memory footprint\.
+ Performance improvements for sending messages to the cloud\.
+ Performance and stability improvements for the download agent, Device Certificate Manager, and OTA update agent\.
+ Minor bug fixes\.

1\.3\.0  
New features:  
+ Over\-the\-air \(OTA\) update agent capable of handling cloud\-deployed, Greengrass update jobs\. The agent is found under the new `/greengrass/ota` directory\. For more information, see [OTA Updates of AWS IoT Greengrass Core Software](core-ota-update.md)\.
+ Local resource access feature allows Greengrass Lambda functions to access local resources, such as peripheral devices and volumes\. For more information, see [Access Local Resources with Lambda Functions and Connectors](access-local-resources.md)\.

1\.1\.0  
New features:  
+ Deployed AWS IoT Greengrass groups can be reset by deleting Lambda functions, subscriptions, and configurations\. For more information, see [Reset Deployments](reset-deployments-scenario.md)\.
+ Support for Node\.js 6\.10 and Java 8 Lambda runtimes, in addition to Python 2\.7\.
To migrate from the previous version of the AWS IoT Greengrass core:  
+ Copy certificates from the `/greengrass/configuration/certs` folder to `/greengrass/certs`\.
+ Copy `/greengrass/configuration/config.json` to `/greengrass/config/config.json`\.
+ Run `/greengrass/ggc/core/greengrassd` instead of `/greengrass/greengrassd`\.
+ Deploy the group to the new core\.

1\.0\.0  
Initial version

------

## AWS IoT Greengrass Groups<a name="gg-group"></a>

An AWS IoT Greengrass group is a collection of settings and components, such as an AWS IoT Greengrass core, devices, and subscriptions\. Groups are used to define a scope of interaction\. For example, a group might represent one floor of a building, one truck, or an entire mining site\. The following diagram shows the components that can make up an Greengrass group\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/gg-group.png)

In the preceding diagram:

A: AWS IoT Greengrass group definition  
A collection of information about the AWS IoT Greengrass group\.

B: AWS IoT Greengrass group settings  
These include:  
+ AWS IoT Greengrass group role\.
+ Certificate authority and local connection configuration\.
+ AWS IoT Greengrass core connectivity information\.
+ Default Lambda runtime environment\. For more information, see [Setting Default Containerization for Lambda Functions in a Group](lambda-group-config.md#lambda-containerization-groupsettings)\.
+ CloudWatch and local logs configuration\. For more information, see [Monitoring with AWS IoT Greengrass Logs](greengrass-logs-overview.md)\.

C: AWS IoT Greengrass core  
The AWS IoT Core thing that represents the AWS IoT Greengrass core\. For more information, see [Configure the AWS IoT Greengrass Core](gg-core.md)\.

D: Lambda function definition  
A list of Lambda functions that run locally on the core, with associated configuration data\. For more information, see [Run Lambda Functions on the AWS IoT Greengrass Core](lambda-functions.md)\.

E: Subscription definition  
A list of subscriptions that enable communication using MQTT messages\. A subscription defines:  
+ A message source and message target\. These can be devices, Lambda functions, connectors, AWS IoT Core, and the local shadow service\.
+ A topic \(or subject\) that's used to filter messages\.
For more information, see [Managed Subscriptions in the MQTT Messaging Workflow](gg-sec.md#gg-msg-workflow)\.

F: Connector definition  
A list of connectors that run locally on the core, with associated configuration data\. For more information, see [Integrate with Services and Protocols Using Greengrass Connectors](connectors.md)\.

G: Device definition  
A list of AWS IoT Core things \(devices\) that are members of the AWS IoT Greengrass group, with associated configuration data\. For more information, see [Devices in AWS IoT Greengrass](#devices)\.

H: Resource definition  
A list of local resources, machine learning resources, and secret resources on the AWS IoT Greengrass core, with associated configuration data\. For more information, see [Access Local Resources with Lambda Functions and Connectors](access-local-resources.md), [Perform Machine Learning Inference](ml-inference.md), and [Deploy Secrets to the AWS IoT Greengrass Core](secrets.md)\.

When deployed, the AWS IoT Greengrass group definition, Lambda functions, connectors, resources, and subscription table are copied to an AWS IoT Greengrass core device\. For more information, see [Deploy AWS IoT Greengrass Groups to an AWS IoT Greengrass Core](deployments.md)\.

## Devices in AWS IoT Greengrass<a name="devices"></a>

An AWS IoT Greengrass group can contain two types of device:

Greengrass core  
A Greengrass core is a device that runs the AWS IoT Greengrass Core software, which allows it to communicate directly with AWS IoT Core and the AWS IoT Greengrass service\. A core has its own device certificate used for authenticating with AWS IoT Core\. It has a device shadow and an entry in the AWS IoT Core registry\. Greengrass cores run a local Lambda runtime, deployment agent, and IP address tracker that sends IP address information to the AWS IoT Greengrass service to allow Greengrass devices to automatically discover their group and core connection information\. For more information, see [Configure the AWS IoT Greengrass Core](gg-core.md)\.  
A Greengrass group must contain exactly one core\.

Device connected to a Greengrass core  <a name="greengrass-devices"></a>
Connected devices \(also called *Greengrass devices*\) also have their own device certificate for AWS IoT Core authentication, a device shadow, and an entry in the AWS IoT Core registry\. <a name="gg-device-discovery"></a>Greengrass devices can run [FreeRTOS](https://docs.aws.amazon.com/freertos/latest/userguide/freertos-lib-gg-connectivity.html) or use the [AWS IoT Device SDK](#iot-device-sdk) or [ AWS IoT Greengrass Discovery API](gg-discover-api.md) to get discovery information used to connect and authenticate with the core in the same Greengrass group\. To learn how to use the AWS IoT console to create and configure a device for AWS IoT Greengrass, see [Module 4: Interacting with Devices in an AWS IoT Greengrass Group](module4.md)\. Or, for examples that show you how to use the AWS CLI to create and configure a device for AWS IoT Greengrass, see [create\-device\-definition](https://docs.aws.amazon.com/cli/latest/reference/greengrass/create-device-definition.html) in the *AWS CLI Command Reference*\.  
In a Greengrass group, you can create subscriptions that allow devices to communicate over MQTT with Lambda functions, connectors, and other devices in the group, and with AWS IoT Core or the local shadow service\. MQTT messages are routed through the core\. If the core device loses connectivity to the cloud, devices can continue to communicate over the local network\. Devices can vary in size, from smaller microcontroller\-based devices to large appliances\. Currently, a Greengrass group can contain up to 200 devices\. A device can be a member of up to 10 groups\.  
<a name="sitewise-connector-opcua-support"></a>OPC\-UA is an information exchange standard for industrial communication\. To implement support for OPC\-UA on the Greengrass core, you can use the [IoT SiteWise connector](iot-sitewise-connector.md)\. The connector sends industrial device data from OPC\-UA servers to asset properties in AWS IoT SiteWise\.

The following table shows how these device types are related\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/devices.png)

The AWS IoT Greengrass core device stores certificates in two locations:<a name="ggc-certificate-locations"></a>
+ Core device certificate in `/greengrass-root/certs`\. Typically, the core device certificate is named `hash.cert.pem` \(for example, `86c84488a5.cert.pem`\)\. This certificate is used by the AWS IoT client for mutual authentication when the core connects to the AWS IoT Core and AWS IoT Greengrass services\.
+ MQTT server certificate in `/greengrass-root/ggc/var/state/server`\. The MQTT server certificate is named `server.crt`\. This certificate is used for mutual authentication between the local MQTT server \(on the Greengrass core\) and Greengrass devices\.
**Note**  
*greengrass\-root* represents the path where the AWS IoT Greengrass Core software is installed on your device\. Typically, this is the `/greengrass` directory\.

## SDKs<a name="gg-sdks"></a>

The following AWS\-provided SDKs are used to work with AWS IoT Greengrass:

AWS SDK  
Use the AWS SDK to build applications that interact with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK in deployed Lambda functions to make direct calls to any AWS service\. For more information, see [AWS SDKs](lambda-functions.md#lambda-sdks-aws)\.  
The Greengrass\-specific operations available in the AWS SDKs are also available in the [AWS IoT Greengrass API](https://docs.aws.amazon.com/greengrass/latest/apireference/) and [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/greengrass)\.

AWS IoT Device SDK  <a name="iot-device-sdk"></a>
The AWS IoT Device SDK helps devices connect to AWS IoT Core or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDK platforms to connect to a Greengrass core only the C\+\+ and Python SDKs provide AWS IoT Greengrass specific functionality, such as access to the AWS IoT Greengrass Discovery Service and group CA certificate downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot-core/resources/#AWS_IoT_Device_SDKs)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the Greengrass core, publish messages to AWS IoT, interact with the local shadow service, invoke other deployed Lambda functions, and access secret resources\. This SDK is used by Lambda functions that run on an AWS IoT Greengrass core\. For more information, see [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core)\.

AWS IoT Greengrass Machine Learning SDK  
The AWS IoT Greengrass Machine Learning SDK enables Lambda functions to consume machine learning models that are deployed to the Greengrass core as machine learning resources\. This SDK is used by Lambda functions that run on an AWS IoT Greengrass core and interact with a local inference service\. For more information, see [AWS IoT Greengrass Machine Learning SDK](lambda-functions.md#lambda-sdks-ml)\.

## Supported Platforms and Requirements<a name="gg-platforms"></a>

The following tabs list supported platforms and requirements for the AWS IoT Greengrass Core software\.

**Note**  
You can download the AWS IoT Greengrass Core software from the [AWS IoT Greengrass Core Software](#gg-core-download-tab) downloads\.

------
#### [ GGC v1\.10 ]

Supported platforms:
+ <a name="arch_armv7l_193"></a>Architecture: Armv7l
  + OS: Linux; Distribution: [Raspbian Buster, 2019\-07\-10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/)\. AWS IoT Greengrass might work with other distributions for a Raspberry Pi, but we recommend Raspbian because it's the officially supported distribution\.
  + OS: Linux; Distribution: [OpenWrt](https://openwrt.org/)
+ <a name="arch_armv8-aarch64_190"></a>Architecture: Armv8 \(AArch64\)
  + OS: Linux; Distribution: [Arch Linux](https://www.archlinux.org/)
  + OS: Linux; Distribution: [OpenWrt](https://openwrt.org/)
+ <a name="arch_armv6l_193"></a>Architecture: Armv6l
  + OS: Linux; Distribution: [Raspbian Buster, 2019\-07\-10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/)
+ <a name="arch_x86-64_amazonlinux_190"></a>Architecture: x86\_64
  + OS: Linux; Distribution: Amazon Linux \(amzn2\-ami\-hvm\-2\.0\.20190313\-x86\_64\-gp2\), Ubuntu 18\.04
+ <a name="arch_docker_180"></a>Windows, macOS, and Linux platforms can run AWS IoT Greengrass in a Docker container\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.

Requirements:
+ <a name="mem_128_disk_space_180"></a>Minimum 128 MB disk space available for the AWS IoT Greengrass Core software\. If you use the [OTA update agent](core-ota-update.md), the minimum is 400 MB\.
+ <a name="mem_128_ram_1100"></a>Minimum 128 MB RAM allocated to the AWS IoT Greengrass Core software\. With [stream manager](stream-manager.md) enabled, the minimum is 198 MB RAM\.
**Note**  
Stream manager is enabled by default if you use the **Default Group creation** option on the AWS IoT console to create your Greengrass group\.
+ Linux kernel version:
  + <a name="kernel_4.4_180"></a>Linux kernel version 4\.4 or later is required to support running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-considerations)\.
  + <a name="kernel_3.17_180"></a>Linux kernel version 3\.17 or later is required to support running AWS IoT Greengrass without containers\. In this configuration, the default Lambda function containerization for the Greengrass group must be set to **No container**\. For instructions, see [Setting Default Containerization for Lambda Functions in a Group](lambda-group-config.md#lambda-containerization-groupsettings)\.
+ <a name="glibc_190"></a>[GNU C Library](https://www.gnu.org/software/libc/) \(glibc\) version 2\.14 or later\. OpenWrt distributions require [musl C Library](https://www.musl-libc.org/download.html) version 1\.1\.16 or later\.
+ <a name="var_run_180"></a>The `/var/run` directory must be present on the device\.
+ <a name="dev_dir_180"></a>The `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` files must be available\.
+ <a name="hardlink_softlink_180"></a>Hardlink and softlink protection must be enabled on the device\. Otherwise, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
+ <a name="kernel_config_180"></a>The following Linux kernel configurations must be enabled on the device: 
  + <a name="kernel_namespace_180"></a>Namespace:
    + CONFIG\_IPC\_NS
    + CONFIG\_UTS\_NS
    + CONFIG\_USER\_NS
    + CONFIG\_PID\_NS
  + <a name="kernel_cgroups_180"></a>Cgroups:
    + CONFIG\_CGROUP\_DEVICE
    + CONFIG\_CGROUPS
    + CONFIG\_MEMCG

    The kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\. The following requirements apply when running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-groupsettings):
    + The *memory* cgroup must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
    + The *devices* cgroup must be enabled and mounted if Lambda functions with [local resource access](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + <a name="kernel_others_180"></a>Others:
    + CONFIG\_POSIX\_MQUEUE
    + CONFIG\_OVERLAY\_FS
    + CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER
    + CONFIG\_SECCOMP\_FILTER
    + CONFIG\_KEYS
    + CONFIG\_SECCOMP
    + CONFIG\_SHMEM
+ <a name="s3_iot_root_cert_180"></a>The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ <a name="stream-manager-requirement"></a>[Stream manager](stream-manager.md) requires the Java 8 runtime and a minimum of 70 MB RAM in addition to the base AWS IoT Greengrass Core software memory requirement\. Stream manager is enabled by default when you use the **Default Group creation** option on the AWS IoT console\. Stream manager is not supported on OpenWrt distributions\.
+ Libraries that support the [AWS Lambda runtime](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) required by the Lambda functions you want to run locally\. Required libraries must be installed on the core and added to the `PATH` environment variable\. Multiple libraries can be installed on the same core\.
  + <a name="runtime_python_3.7"></a>[Python](https://www.python.org/) version 3\.7 for functions that use the Python 3\.7 runtime\.
  + <a name="runtime_python_2.7"></a>[Python](https://www.python.org/) version 2\.7 for functions that use the Python 2\.7 runtime\.
  + <a name="runtime_nodejs_12.x"></a>[Node\.js](https://www.nodejs.org/) version 12\.x for functions that use the Node\.js 12\.x runtime\.
  + <a name="runtime_java_8_190"></a>[Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or later for functions that use the Java 8 runtime\.
**Note**  
Running Java on an OpenWrt distribution isn't officially supported\. However, if your OpenWrt build has Java support, you might be able to run Lambda functions authored in Java on your OpenWrt devices\.

    For more information about AWS IoT Greengrass support for Lambda runtimes, see [Run Lambda Functions on the AWS IoT Greengrass Core](lambda-functions.md)\.
+ <a name="ota_agent_180"></a>The following shell commands \(not the BusyBox variants\) are required by the [over\-the\-air \(OTA\) update agent](core-ota-update.md#ota-agent):
  + `wget`
  + `realpath`
  + `tar`
  + `readlink`
  + `basename`
  + `dirname`
  + `pidof`
  + `df`
  + `grep`
  + `umount`
  + `mv`
  + `gzip`
  + `mkdir`
  + `rm`
  + `ln`
  + `cut`
  + `cat`

------
#### [ GGC v1\.9 ]

Supported platforms:
+ <a name="arch_armv7l_193"></a>Architecture: Armv7l
  + OS: Linux; Distribution: [Raspbian Buster, 2019\-07\-10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/)\. AWS IoT Greengrass might work with other distributions for a Raspberry Pi, but we recommend Raspbian because it's the officially supported distribution\.
  + OS: Linux; Distribution: [OpenWrt](https://openwrt.org/)
+ <a name="arch_armv8-aarch64_190"></a>Architecture: Armv8 \(AArch64\)
  + OS: Linux; Distribution: [Arch Linux](https://www.archlinux.org/)
  + OS: Linux; Distribution: [OpenWrt](https://openwrt.org/)
+ <a name="arch_armv6l_193"></a>Architecture: Armv6l
  + OS: Linux; Distribution: [Raspbian Buster, 2019\-07\-10](https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/)
+ <a name="arch_x86-64_amazonlinux_190"></a>Architecture: x86\_64
  + OS: Linux; Distribution: Amazon Linux \(amzn2\-ami\-hvm\-2\.0\.20190313\-x86\_64\-gp2\), Ubuntu 18\.04
+ <a name="arch_docker_180"></a>Windows, macOS, and Linux platforms can run AWS IoT Greengrass in a Docker container\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.

Requirements:
+ <a name="mem_128_disk_space_180"></a>Minimum 128 MB disk space available for the AWS IoT Greengrass Core software\. If you use the [OTA update agent](core-ota-update.md), the minimum is 400 MB\.
+ <a name="mem_128_ram_180"></a>Minimum 128 MB RAM allocated to the AWS IoT Greengrass Core software\.
+ Linux kernel version:
  + <a name="kernel_4.4_180"></a>Linux kernel version 4\.4 or later is required to support running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-considerations)\.
  + <a name="kernel_3.17_180"></a>Linux kernel version 3\.17 or later is required to support running AWS IoT Greengrass without containers\. In this configuration, the default Lambda function containerization for the Greengrass group must be set to **No container**\. For instructions, see [Setting Default Containerization for Lambda Functions in a Group](lambda-group-config.md#lambda-containerization-groupsettings)\.
+ <a name="glibc_190"></a>[GNU C Library](https://www.gnu.org/software/libc/) \(glibc\) version 2\.14 or later\. OpenWrt distributions require [musl C Library](https://www.musl-libc.org/download.html) version 1\.1\.16 or later\.
+ <a name="var_run_180"></a>The `/var/run` directory must be present on the device\.
+ <a name="dev_dir_180"></a>The `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` files must be available\.
+ <a name="hardlink_softlink_180"></a>Hardlink and softlink protection must be enabled on the device\. Otherwise, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
+ <a name="kernel_config_180"></a>The following Linux kernel configurations must be enabled on the device: 
  + <a name="kernel_namespace_180"></a>Namespace:
    + CONFIG\_IPC\_NS
    + CONFIG\_UTS\_NS
    + CONFIG\_USER\_NS
    + CONFIG\_PID\_NS
  + <a name="kernel_cgroups_180"></a>Cgroups:
    + CONFIG\_CGROUP\_DEVICE
    + CONFIG\_CGROUPS
    + CONFIG\_MEMCG

    The kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\. The following requirements apply when running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-groupsettings):
    + The *memory* cgroup must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
    + The *devices* cgroup must be enabled and mounted if Lambda functions with [local resource access](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + <a name="kernel_others_180"></a>Others:
    + CONFIG\_POSIX\_MQUEUE
    + CONFIG\_OVERLAY\_FS
    + CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER
    + CONFIG\_SECCOMP\_FILTER
    + CONFIG\_KEYS
    + CONFIG\_SECCOMP
    + CONFIG\_SHMEM
+ <a name="s3_iot_root_cert_180"></a>The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ Libraries that support the [AWS Lambda runtime](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) required by the Lambda functions you want to run locally\. Required libraries must be installed on the core and added to the `PATH` environment variable\. Multiple libraries can be installed on the same core\.
  + <a name="runtime_python_2.7"></a>[Python](https://www.python.org/) version 2\.7 for functions that use the Python 2\.7 runtime\.
  + <a name="runtime_python_3.7"></a>[Python](https://www.python.org/) version 3\.7 for functions that use the Python 3\.7 runtime\.
  + <a name="runtime_nodejs_6.10"></a>[Node\.js](https://www.nodejs.org/) version 6\.10 or later for functions that use the Node\.js 6\.10 runtime\.
  + <a name="runtime_nodejs_8.10"></a>[Node\.js](https://www.nodejs.org/) version 8\.10 or later for functions that use the Node\.js 8\.10 runtime\.
  + <a name="runtime_java_8_190"></a>[Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or later for functions that use the Java 8 runtime\.
**Note**  
Running Java on an OpenWrt distribution isn't officially supported\. However, if your OpenWrt build has Java support, you might be able to run Lambda functions authored in Java on your OpenWrt devices\.

    For more information about AWS IoT Greengrass support for Lambda runtimes, see [Run Lambda Functions on the AWS IoT Greengrass Core](lambda-functions.md)\.
+ <a name="ota_agent_180"></a>The following shell commands \(not the BusyBox variants\) are required by the [over\-the\-air \(OTA\) update agent](core-ota-update.md#ota-agent):
  + `wget`
  + `realpath`
  + `tar`
  + `readlink`
  + `basename`
  + `dirname`
  + `pidof`
  + `df`
  + `grep`
  + `umount`
  + `mv`
  + `gzip`
  + `mkdir`
  + `rm`
  + `ln`
  + `cut`
  + `cat`

------
#### [ GGC v1\.8 ]
+ Supported platforms:
  + <a name="arch_armv7l_rpi_180"></a>Architecture: Armv7l; OS: Linux; Distribution: [Raspbian Stretch, 2018\-06\-29](https://downloads.raspberrypi.org/raspbian/images/raspbian-2018-06-29/)\. Other versions might work with AWS IoT Greengrass, but we recommend this because it is the officially supported distribution\.
  + <a name="arch_x86-64_amazonlinux_180"></a>Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\), Ubuntu 14\.04 – 16\.04
  + <a name="arch_armv8-aarch64_archlinux_180"></a>Architecture: Armv8 \(AArch64\); OS: Linux; Distribution: Arch Linux
  + <a name="arch_docker_180"></a>Windows, macOS, and Linux platforms can run AWS IoT Greengrass in a Docker container\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.
  + <a name="arch_snap_180"></a>Linux platforms can run a version of AWS IoT Greengrass with limited functionality using the Greengrass snap, which is available through [Snapcraft](https://snapcraft.io/aws-iot-greengrass)\. For more information, see [AWS IoT Greengrass Snap Software](#gg-snapstore-download)\.
+ The following items are required:
  + <a name="mem_128_disk_space_180"></a>Minimum 128 MB disk space available for the AWS IoT Greengrass Core software\. If you use the [OTA update agent](core-ota-update.md), the minimum is 400 MB\.
  + <a name="mem_128_ram_180"></a>Minimum 128 MB RAM allocated to the AWS IoT Greengrass Core software\.
  + Linux kernel version:
    + <a name="kernel_4.4_180"></a>Linux kernel version 4\.4 or later is required to support running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-considerations)\.
    + <a name="kernel_3.17_180"></a>Linux kernel version 3\.17 or later is required to support running AWS IoT Greengrass without containers\. In this configuration, the default Lambda function containerization for the Greengrass group must be set to **No container**\. For instructions, see [Setting Default Containerization for Lambda Functions in a Group](lambda-group-config.md#lambda-containerization-groupsettings)\.
  + <a name="glibc_180"></a>[GNU C Library](https://www.gnu.org/software/libc/) \(glibc\) version 2\.14 or later\.
  + <a name="var_run_180"></a>The `/var/run` directory must be present on the device\.
  + <a name="dev_dir_180"></a>The `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` files must be available\.
  + <a name="hardlink_softlink_180"></a>Hardlink and softlink protection must be enabled on the device\. Otherwise, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + <a name="kernel_config_180"></a>The following Linux kernel configurations must be enabled on the device: 
    + <a name="kernel_namespace_180"></a>Namespace:
      + CONFIG\_IPC\_NS
      + CONFIG\_UTS\_NS
      + CONFIG\_USER\_NS
      + CONFIG\_PID\_NS
    + <a name="kernel_cgroups_180"></a>Cgroups:
      + CONFIG\_CGROUP\_DEVICE
      + CONFIG\_CGROUPS
      + CONFIG\_MEMCG

      The kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\. The following requirements apply when running AWS IoT Greengrass with [containers](lambda-group-config.md#lambda-containerization-groupsettings):
      + The *memory* cgroup must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
      + The *devices* cgroup must be enabled and mounted if Lambda functions with [local resource access](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
    + <a name="kernel_others_180"></a>Others:
      + CONFIG\_POSIX\_MQUEUE
      + CONFIG\_OVERLAY\_FS
      + CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER
      + CONFIG\_SECCOMP\_FILTER
      + CONFIG\_KEYS
      + CONFIG\_SECCOMP
      + CONFIG\_SHMEM
  + <a name="s3_iot_root_cert_180"></a>The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items are conditionally required:
  + Libraries that support the [AWS Lambda runtime](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) required by the Lambda functions you want to run locally\. Required libraries must be installed on the core and added to the `PATH` environment variable\. Multiple libraries can be installed on the same core\.
    + <a name="runtime_python_2.7"></a>[Python](https://www.python.org/) version 2\.7 for functions that use the Python 2\.7 runtime\.
    + <a name="runtime_nodejs_6.10"></a>[Node\.js](https://www.nodejs.org/) version 6\.10 or later for functions that use the Node\.js 6\.10 runtime\.
    + <a name="runtime_java_8"></a>[Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or later for functions that use the Java 8 runtime\.
  + <a name="ota_agent_180"></a>The following shell commands \(not the BusyBox variants\) are required by the [over\-the\-air \(OTA\) update agent](core-ota-update.md#ota-agent):
    + `wget`
    + `realpath`
    + `tar`
    + `readlink`
    + `basename`
    + `dirname`
    + `pidof`
    + `df`
    + `grep`
    + `umount`
    + `mv`
    + `gzip`
    + `mkdir`
    + `rm`
    + `ln`
    + `cut`
    + `cat`

------

<a name="gg-limits-genref"></a>For information about AWS IoT Greengrass quotas \(limits\), see [Service Quotas](https://docs.aws.amazon.com/general/latest/gr/greengrass.html#limits_greengrass) in the *Amazon Web Services General Reference*\.

## AWS IoT Greengrass Downloads<a name="gg-downloads"></a>

 You can use the following information to find and download software for use with AWS IoT Greengrass\. 

### AWS IoT Greengrass Core Software<a name="gg-core-download-tab"></a>

<a name="ggc-software-descripton"></a> The AWS IoT Greengrass Core software extends AWS functionality onto an AWS IoT Greengrass core device, making it possible for local devices to act locally on the data they generate\.

------
#### [ v1\.10\.0 ]

New features in v1\.10:<a name="what-new-v1100"></a>
+ A stream manager that processes data streams locally and exports them to the AWS Cloud automatically\. This feature requires Java 8 on the Greengrass core device\. For more information, see [Manage Data Streams on the AWS IoT Greengrass Core](stream-manager.md)\.
+ A new Greengrass Docker application deployment connector that runs a Docker application on a core device\. For more information, see [Docker Application Deployment Connector](docker-app-connector.md)\.
+ A new IoT SiteWise connector that sends industrial device data from OPC\-UA servers to asset properties in AWS IoT SiteWise\. For more information, see [IoT SiteWise Connector](iot-sitewise-connector.md)\.
+ Lambda functions that run without containerization can access machine learning resources in the Greengrass group\. For more information, see [Access Machine Learning Resources from Lambda Functions](access-ml-resources.md)\.
+ Support for MQTT persistent sessions with AWS IoT\. For more information, see [MQTT Persistent Sessions with AWS IoT](gg-core.md#mqtt-persistent-sessions)\.
+ Local MQTT traffic can travel over a port other than the default port 8883\. For more information, see [Configure the MQTT Port for Local Messaging](gg-core.md#config-local-mqtt-port)\.
+ New `queueFullPolicy` options in the [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core) for reliable message publishing from Lambda functions\.
+ Support for running Node\.js 12\.x Lambda functions on the core\.

Bug fixes and improvements:<a name="bug-fix-v1100"></a>
+ Over\-the\-air \(OTA\) updates with hardware security integration can be configured with OpenSSL 1\.1\.
+ General performance improvements and bug fixes\.

To install the AWS IoT Greengrass Core software on your core device, download the package for your architecture, distribution, and operating system \(OS\), and then follow the steps in the [Getting Started Guide](gg-gs.md)\.

**Tip**  
<a name="ggc-install-options"></a>AWS IoT Greengrass also provides other options for installing the AWS IoT Greengrass Core software\. For example, you can use [Greengrass device setup](quick-start.md) to configure your environment and install the latest version of the AWS IoT Greengrass Core software\. Or, on supported Debian platforms, you can use the [APT package manager](install-ggc.md#ggc-package-manager) to install or upgrade the AWS IoT Greengrass Core software\. For more information, see [Install the AWS IoT Greengrass Core Software](install-ggc.md)\.


| Architecture | Distribution | OS | Link | 
| --- | --- | --- | --- | 
| Armv8 \(AArch64\) | Arch Linux | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-linux-aarch64-1.10.0.tar.gz) | 
| Armv8 \(AArch64\) | OpenWrt | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-openwrt-aarch64-1.10.0.tar.gz) | 
| Armv7l | Raspbian | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-linux-armv7l-1.10.0.tar.gz) | 
| Armv7l | OpenWrt | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-openwrt-armv7l-1.10.0.tar.gz) | 
| Armv6l | Raspbian | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-linux-armv6l-1.10.0.tar.gz) | 
| x86\_64 | Linux | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/greengrass-linux-x86-64-1.10.0.tar.gz) | 

------
#### [  v1\.9\.4  ]

New features in v1\.9:
+ <a name="what-new-v190-runtimes"></a>Support for Python 3\.7 and Node\.js 8\.10 Lambda runtimes\. Lambda functions that use Python 3\.7 and Node\.js 8\.10 runtimes can now run on an AWS IoT Greengrass core\. \(AWS IoT Greengrass continues to support the Python 2\.7 and Node\.js 6\.10 runtimes\.\)
+ <a name="what-new-v190-mqtt-opt"></a>Optimized MQTT connections\. The Greengrass core establishes fewer connections with the AWS IoT Core\. This change can reduce operational costs for charges that are based on the number of connections\.
+ <a name="what-new-v190-ec-key"></a>Elliptic Curve \(EC\) key for the local MQTT server\. The local MQTT server supports EC keys in addition to RSA keys\. \(The MQTT server certificate has an SHA\-256 RSA signature, regardless of the key type\.\) For more information, see [AWS IoT Greengrass Core Security Principals](gg-sec.md#gg-principals)\.
+ <a name="what-new-v192-openwrt"></a>Support for [OpenWrt](https://openwrt.org/)\. AWS IoT Greengrass Core software v1\.9\.2 or later can be installed on OpenWrt distributions with Armv8 \(AArch64\) and Armv7l architectures\. Currently, OpenWrt does not support ML inference\.
+ <a name="what-new-v193-armv6l"></a>Support for Armv6l\. AWS IoT Greengrass Core software v1\.9\.3 or later can be installed on Raspbian distributions on Armv6l architectures \(for example, on Raspberry Pi Zero devices\)\.
+ <a name="what-new-v193-ota-alpn"></a>OTA updates on port 443 with ALPN\. Greengrass cores that use port 443 for MQTT traffic now support over\-the\-air \(OTA\) software updates\. AWS IoT Greengrass uses the Application Layer Protocol Network \(ALPN\) TLS extension to enable these connections\. For more information, see [OTA Updates of AWS IoT Greengrass Core Software](core-ota-update.md) and [Connect on Port 443 or Through a Network Proxy](gg-core.md#alpn-network-proxy)\.

To install the AWS IoT Greengrass Core software on your core device, download the package for your architecture, distribution, and operating system \(OS\), and then follow the steps in the [Getting Started Guide](gg-gs.md)\.


| Architecture | Distribution | OS | Link | 
| --- | --- | --- | --- | 
| Armv8 \(AArch64\) | Arch Linux | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-linux-aarch64-1.9.4.tar.gz) | 
| Armv8 \(AArch64\) | OpenWrt | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-openwrt-aarch64-1.9.4.tar.gz) | 
| Armv7l | Raspbian | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-linux-armv7l-1.9.4.tar.gz) | 
| Armv7l | OpenWrt | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-openwrt-armv7l-1.9.4.tar.gz) | 
| Armv6l | Raspbian | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-linux-armv6l-1.9.4.tar.gz) | 
| x86\_64 | Linux | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/greengrass-linux-x86-64-1.9.4.tar.gz) | 

------
#### [  v1\.8\.4  ]
+ <a name="what-new-v180"></a>New features:
  + Configurable default access identity for Lambda functions in the group\. This group\-level setting determines the default permissions that are used to run Lambda functions\. You can set the user ID, group ID, or both\. Individual Lambda functions can override the default access identity of their group\. For more information, see [Setting the Default Access Identity for Lambda Functions in a Group](lambda-group-config.md#lambda-access-identity-groupsettings)\.
  + HTTPS traffic over port 443\. HTTPS communication can be configured to travel over port 443 instead of the default port 8443\. This complements AWS IoT Greengrass support for the Application Layer Protocol Network \(ALPN\) TLS extension and allows all Greengrass messaging traffic—both MQTT and HTTPS—to use port 443\. For more information, see [Connect on Port 443 or Through a Network Proxy](gg-core.md#alpn-network-proxy)\.
  + Predictably named client IDs for AWS IoT connections\. This change enables support for AWS IoT Device Defender and [AWS IoT Lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html), so you can receive notifications for connect, disconnect, subscribe, and unsubscribe events\. Predictable naming also makes it easier to create logic around connection IDs \(for example, to create [subscribe policy](https://docs.aws.amazon.com/iot/latest/developerguide/pub-sub-policy.html#pub-sub-policy-cert) templates based on certificate attributes\)\. For more information, see [Client IDs for MQTT Connections with AWS IoT](gg-core.md#connection-client-id)\.

  Bug fixes and improvements:
  + Fixed an issue with shadow synchronization and device certificate manager reconnection\.
  + General performance improvements and bug fixes\.

To install the AWS IoT Greengrass Core software on your core device, download the package for your architecture, distribution, and operating system \(OS\), and then follow the steps in the [Getting Started Guide](gg-gs.md)\.


| Architecture | Distribution | OS | Link | 
| --- | --- | --- | --- | 
| Armv8 \(AArch64\) | Ubuntu 14\.04 \- 16\.04 | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.8.4/greengrass-linux-aarch64-1.8.4.tar.gz) | 
| Armv7l | Raspbian | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.8.4/greengrass-linux-armv7l-1.8.4.tar.gz) | 
| x86\_64 | Linux | Linux | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.8.4/greengrass-linux-x86-64-1.8.4.tar.gz) | 

------

 By downloading this software, you agree to the [ Greengrass Core Software License Agreement](https://greengrass-release-license.s3.us-west-2.amazonaws.com/greengrass-license-v1.pdf)\. 

For information about other options for installing the AWS IoT Greengrass Core software on your device, see [Install the AWS IoT Greengrass Core Software](install-ggc.md)\.

 

### AWS IoT Greengrass Snap Software<a name="gg-snapstore-download"></a>

<a name="snap-ggc-v1.8-only"></a>Currently, AWS IoT Greengrass snap software is available for AWS IoT Greengrass core version 1\.8 only\.

<a name="snap-ggc-desc"></a> The AWS IoT Greengrass snap software download makes it possible for you to run a version of AWS IoT Greengrass with limited functionality on Linux cloud, desktop, and IoT environments through convenient containerized software packages\. These packages, or snaps, contain the AWS IoT Greengrass Core software and its dependencies\. You can download and use these packages on your Linux environments as\-is\.

<a name="snap-ggc-limitations"></a> The AWS IoT Greengrass snap allows you to run a version of AWS IoT Greengrass with limited functionality on your Linux environments\. Currently, Java, Node\.js, and native Lambda functions are not supported\. Machine learning inference, connectors, and noncontainerized Lambda functions are also not supported\.

For more information, see [Getting Started with AWS IoT Greengrass Snap](install-ggc.md#gg-snap-setup)\.

 

### AWS IoT Greengrass Docker Software<a name="gg-docker-download"></a>

AWS provides a Dockerfile and Docker image that make it easier for you to run AWS IoT Greengrass in a Docker container\.

Dockerfile  
Source code for building custom AWS IoT Greengrass container images\. The image can be modified to run on different platform architectures or to reduce the image size\. For instructions, see the README file\.  
Choose the AWS IoT Greengrass Core software version\.  
+  [ Docker v1\.10\.0](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.10.0/aws-greengrass-docker-1.10.0.tar.gz)\. 
+  [ Docker v1\.9\.4](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.9.4/aws-greengrass-docker-1.9.4.tar.gz)\. 
+  [ Docker v1\.8\.1](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.8.1/aws-greengrass-docker-1.8.1.tar.gz)\. 
 

Docker image  
Docker image with the AWS IoT Greengrass Core software and dependencies installed\. Prebuilt images can help you get started quickly and experiment with AWS IoT Greengrass\.  
+ Docker image from [ Docker Hub](https://hub.docker.com/r/amazon/aws-iot-greengrass)\.
+ Docker image from Amazon Elastic Container Registry \(Amazon ECR\)\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.

By downloading this software, you agree to the [ Greengrass Core Software License Agreement](https://greengrass-release-license.s3.us-west-2.amazonaws.com/greengrass-license-v1.pdf)\.

 

### AWS IoT Greengrass Core SDK Software<a name="gg-core-sdk-download"></a>

Lambda functions use the AWS IoT Greengrass Core SDK to interact with the AWS IoT Greengrass core locally\. This allows deployed Lambda functions to:<a name="gg-core-sdk-functionality"></a>
+ Exchange MQTT messages with AWS IoT Core\.
+ Exchange MQTT messages with connectors, devices, and other Lambda functions in the Greengrass group\.
+ Interact with the local shadow service\.
+ Invoke other local Lambda functions\.
+ Access [secret resources](secrets.md)\.
+ Interact with [stream manager](stream-manager.md)\.

Download the AWS IoT Greengrass Core SDK for your language or platform from GitHub\.<a name="gg-core-sdk-download-list"></a>
+ [ AWS IoT Greengrass Core SDK for Java](https://github.com/aws/aws-greengrass-core-sdk-java/)
+ [ AWS IoT Greengrass Core SDK for Node\.js](https://github.com/aws/aws-greengrass-core-sdk-js/)
+ [ AWS IoT Greengrass Core SDK for Python](https://github.com/aws/aws-greengrass-core-sdk-python/)
+ [ AWS IoT Greengrass Core SDK for C](https://github.com/aws/aws-greengrass-core-sdk-c/)

For more information, see [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core)\.

 

### AWS IoT Greengrass Machine Learning Runtimes and Precompiled Libraries<a name="gg-ml-runtimes-pc-libs"></a>

Machine learning runtimes and libraries are required for your ML models to perform inference on Greengrass devices\.

Download the model type for your platform\.

------
#### [  Raspberry Pi  ]

Choose the download link for your model type\.

By downloading this software you agree to the associated license\.


| Model type | Version | License | Link | 
| --- | --- | --- | --- | 
| MXNet | 1\.2\.1 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/mxnet/ggc-mxnet-v1.2.1-python-raspi.tar.gz) | 
| TensorFlow | 1\.4\.0 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/tf/greengrass_ML@Edge_TF_v1_4_0_installer_cp27_raspi3_armv7.tar.gz) | 
| Deep Learning Runtime | 1\.0\.0 | [Greengrass License](https://greengrass-release-license.s3.us-west-2.amazonaws.com/greengrass-license-v1.pdf) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/dlr/dlr-1.0-py2-armv7l.tar.gz) | 

------
#### [  Nvidia Jetson TX2  ]

Choose the download link for your model type\.

By downloading this software you agree to the associated license\.


| Model type | Version | License | Link | 
| --- | --- | --- | --- | 
| MXNet | 1\.2\.1 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/mxnet/ggc-mxnet-v1.2.1-cu90-python-nvidia-tx2.tar.gz) | 
| TensorFlow | 1\.10\.0 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/tf/greengrass_ML@Edge_TF_v1_10_1_installer_cp27_cu90_aarch64_nvidia_jetson_tx2.tar.gz) | 
| Deep Learning Runtime | 1\.0\.0 | [Greengrass License](https://greengrass-release-license.s3.us-west-2.amazonaws.com/greengrass-license-v1.pdf) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/dlr/dlr-1.0-py2-cuda90-aarch64.tar.gz) | 

------
#### [  Intel Atom  ]

Choose the download link for your model type\.

By downloading this software you agree to the associated license\.


| Model type | Version | License | Link | 
| --- | --- | --- | --- | 
| MXNet | 1\.2\.1 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/mxnet/ggc-mxnet-v1.2.1-python-intel.tar.gz) | 
| TensorFlow | 1\.4\.0 | [Apache License 2\.0](https://www.apache.org/licenses/LICENSE-2.0) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/tf/greengrass_ML@Edge_TF_v1_4_0_installer_cp27_intel_mkl_x86_64.tar.gz) | 
| Deep Learning Runtime | 1\.0\.0 | [Greengrass License](https://greengrass-release-license.s3.us-west-2.amazonaws.com/greengrass-license-v1.pdf) | [Download](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-installers/dlr/dlr-1.0-py2-opencl-x86_64.tar.gz) | 

------

 

### AWS IoT Greengrass ML SDK Software<a name="gg-ml-sdk-download"></a>

The [AWS IoT Greengrass Machine Learning SDK](lambda-functions.md#lambda-sdks-ml) enables the Lambda functions you author to consume a local machine learning model and send data to the [ML Feedback](ml-feedback-connector.md) connector for uploading and publishing\.

------
#### [  v1\.1\.0  ]
+  [ Python 3\.7 or 2\.7](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-sdk/downloads/python/3.7/greengrass-machine-learning-python-sdk-1.1.0.tar.gz) \- Current version\. 

------
#### [  v1\.0\.0  ]
+  [ Python 2\.7](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-sdk/downloads/python/2.7/greengrass-machine-learning-python-sdk-1.0.0.tar.gz)\. 

------

## We Want to Hear from You<a name="contact-us"></a>

We welcome your feedback\. To contact us, visit the [AWS IoT Greengrass Forum](https://forums.aws.amazon.com/forum.jspa?forumID=254)\.