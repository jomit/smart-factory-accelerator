## Development Guide 

- Install [VS Code](https://code.visualstudio.com/Download) and [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

- Create an [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal#create-a-container-registry) and [Log in to the registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal#log-in-to-registry)

- Update the `registryCredentials` in `FactoryEdge\deployment.template.json`

- `az acr login --name <registry-name>`

- Right click on `FactoryEdge\deployment.template.json` file and click on `Build and Push IoT Edge Solution`

- Right click on `FactoryEdge\deployment.template.json` file and click on `Generate IoT Edge Deployment Manifest`