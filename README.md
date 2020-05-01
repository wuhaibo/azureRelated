"# azureRelated" 
```powershell
# create new resource group
New-AzResourceGroup -name "new-group-name" -location "west europe"

```

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template> -TemplateParameterFile <path-to-parameterfile>
```
```powershell
remove-azResourceGroup -name <resourceGroupName>
get-azResourceGroup | ft 

```

```powershell
#get image of os
$managedImage = Get-AzImage `
   -ImageName "hbVmTest-image-win16DC-iis-20200409114811" `
   -ResourceGroupName myResources


#create vm 

New-AzVm `
    -ResourceGroupName "testBackend" `
    -Name "frontend" `
	  -ImageName $managedImage `
    -Location "west europe" `
    -VirtualNetworkName "hbVnet" `
    -SubnetName "frontendSubnet" `
    -OpenPorts 3389

```

```bash
az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>  --parameters <path-to-parameterfile>
```

```bash
# validate template
az deployment group validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json

```

