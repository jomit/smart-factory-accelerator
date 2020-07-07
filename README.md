# smart-factory-accelerator
Smart Factory Solution Accelerator


Run Simulated OPC Server
- docker run -it -p 50000:50000 --name="plc1" --network="azure-iot-edge" --hostname="opcplc" mcr.microsoft.com/iotedge/opc-plc -aa


Build IoT Edge Solution Locally
- VS Code IoT Edge extension

- az acr login --name jomitacr

- Right click the deployment manifest file "Build and Push IoT Edge Solution"


Create IoT Central Device Template and Device
- Copy the scopeid, deviceid and symmetrickey for initializing the edge.


Install IoT Edge with Linux Containers on Windows
- https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-windows-with-linux#install-iot-edge-on-a-new-device


- .{Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -ContainerOs Linux

- .{Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Initialize-IoTEdge -Dps -ScopeId 0ne0013276F -RegistrationId device01 -SymmetricKey Ztt92UTHZyf/9e3OwI1bW2SVflx44Qoqm9HI+JPSbGo= -ContainerOs Linux

- Uninstall-IoTEdge -F



