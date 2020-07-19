# Connected Factory Solution

Table of contents
1. [Overview](#overview)
2. [Create OPCFlattener docker image](#create-opcflattener-docker-image)
3. [Create an IoT Central custom app](#create-an-iot-central-custom-app)
    * [Create a new application](#create-a-new-application)
    * [Access the application](#access-the-application)
    * [Create a device template](#create-a-device-template)
    * [Create custom interface](#create-custom-interface)
    * [Publish template](#publish-template)
    * [Create a device](#create-a-device)
    * [Get the device connection details](#get-the-device-connection-details)
4. [Create an IoT Edge device on Ubuntu VM](#create-an-iot-edge-device-on-ubuntu-vm)
    * [Create a IoT Enable Ubuntu VM](#create-a-iot-enable-ubuntu-vm)
    * [Locate the IoT Edge enabled VM](#locate-the-iot-edge-enabled-vm)
    * [SSH to the IoT Edge enabled VM](#ssh-to-the-iot-edge-enabled-vm)
    * [Restart the IoT Edge](#restart-the-iot-edge)
    * [Verify edge modules](#verify-edge-modules)
5. [Start the OPC-UA Server](#start-the-opc-ua-server)
6. [Check the logs](#check-the-logs)

# Overview 

![Overview](./images/overview.png)

# Create OPCFlattener docker image
Please refer to the following section for details <br>
[OPCFlattener](./README2.md)

# Create an IoT Central custom app

### Create a new application

Access the direct [link](https://apps.azureiotcentral.com/build/new/custom) to create a custom IOT central application. 
 
![](./images/create-iotcentral.png)

### Access the application

After creating the custom IoT Central application, it will show up in your subscription like below. 

![](./images/iotc-resource.png)

Select the custom application _smartfactorydemo_ to get access to the URL for the IoT Central application 

![](./images/iotc-app.png)

Access the URL from a browser or click on it will launch the application

![](./images/iotc-landing.png)

### Create a device template

> **NOTE:**: Edit the _edgedeploymentmanifest.json_ file and make sure the following section is updated

![](./images/update-edge-manifest.png)

Select the "Device template" blade and create a custom IoT edge template

![](./images/iotc-create-template.png)

![](./images/iotc-create-template-2.png)

The _edgedeploymentmanifest.json_ manifest file can be found at this [link](./edgedeploymentmanifest.json)

![](./images/iotc-device-templates.png)

After you review and create, you will see two modules 
* Module OPCFlattener
* Module OPCPublisher

![](./images/iotc-device-template-modules.png)

Delete Module OPCPublisher

![](./images/iotc-delete-opcpublisher-module.png)

### Create custom interface

Add interface
![](./images/iotc-add-interface.png)

Select custom interface
![](./images/iotc-create-custom-interface.png)

Add 5 telemetry capabilities like below
![](./images/iotc-telemetry.png)

### Publish template

![](./images/iotc-publish-template.png)

![](./images/iotc-publish.png)

### Add a device

![](./images/iotc-add-device.png)
![](./images/iotc-create-device.png)

### Get the device connection details
![](./images/iotc-device-list.png)

![](./images/iotc-device-connect.png)

Note the device connection details which is needed in the following step
![](./images/iotc-device-connection-details.png)

# Create an IoT Edge device on Ubuntu VM

### Create a IoT Enable Ubuntu VM

Launch the ARM template to create the IoT Edge device<br>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json" data-linktype="external"><img src="https://aka.ms/deploytoazurebutton" alt="Deploy to Azure Button for iotedge-vm-deploy" data-linktype="external"></a>

![](./images/create-iotedge.png)

### Locate the IoT Edge enabled VM 

![](./images/locate-iot-edge-vm.png)

Get the SSH connection details

![](./images/connect-iotedge.png)

Get the hostname of the VM

![](./images/vm.png)

### SSH to the IoT Edge enabled VM

SSH to the VM
![](./images/ssh-vm.png)

![](./images/ssh-vm-2.png)

Use the `nano` editor to open the IoT Edge config.yaml file
```
sudo nano /etc/iotedge/config.yaml
```
Scroll down until you see # Manual provisioning configuration. Comment out the next three lines as shown in the 
following snippet:
```
# Manual provisioning configuration
#provisioning:
#  source: "manual"
#  device_connection_string: "update-me-later"
```
Scroll down until you see # DPS symmetric key provisioning configuration. Uncomment the next eight lines as shown in 
the following snippet:

![](./images/iotedge-config-yaml-replace.png)

Fill in the details like below<br>
![](./images/iotedge-config-yaml-filled.png)

After editing both sections it should look like below<br>
![](./images/iotedge-config-yaml-updated.png)

### Copy config files
Create a folder `/opcconfigs` under root
```
cd /
sudo mkdir opcconfig
```

Using winscp copy the below files to `/opcconfigs` 
* mappings.json
* publishednodes.json

### Restart the IoT Edge

Execute the below command to restart the iot edge
```
sudo systemctl restart iotedge
```

### Verify edge modules

Notice all modules listed in the _edgeDeploymentManifest.json_ file downloaded and started
* OPCFlattener
* OPCPublisher
* edgeAgent
* edgeHub

![](./images/iotedge-restart.png)

# Start the OPC-UA Server

```
sudo docker run --rm -p 50000:50000 --name="opcplc" --network="azure-iot-edge" --hostname="opcplc" mcr.microsoft.com/iotedge/opc-plc --autoaccept &
```
![](./images/start-opc-server.png)

# Check the logs

Check the OPCPublisher logs
```
sudo docker logs -f OPCPublisher
```
![](./images/opcpublisher-logs.png)

Check the OPCFlattener logs
```
sudo docker logs -f OPCFlattener
```
![](./images/opcflattener-logs.png)


Check the OPCServer logs
```
sudo docker logs -f opcplc
```
![](./images/opcplc-logs.png)

















 



