

## Create the virtual networks

```bash
#1. create the virtual network and subnet for the Sales systems
az network vnet create \
    --resource-group vnet-related \
    --name SalesVNet \
    --address-prefix 10.1.0.0/16 \
    --subnet-name Apps \
    --subnet-prefix 10.1.1.0/24 \
    --location northeurope
```

```bash
#2. create the virtual network and subnet for the Marketing systems.
az network vnet create \
    --resource-group vnet-related \
    --name MarketingVNet \
    --address-prefix 10.2.0.0/16 \
    --subnet-name Apps \
    --subnet-prefix 10.2.1.0/24 \
    --location northeurope
```


```bash
#3. create the virtual network and subnet for the Research systems.
az network vnet create \
    --resource-group vnet-related \
    --name ResearchVNet \
    --address-prefix 10.3.0.0/16 \
    --subnet-name Data \
    --subnet-prefix 10.3.1.0/24 \
    --location westeurope

```

## Confirm the virtual network configuration

```bash
# view the virtual networks.
az network vnet list --output table

```

## Create virtual machines in each virtual network

```bash
#1 create an Ubuntu VM in the Apps subnet of SalesVNet
az vm create \
    --resource-group vnet-related \
    --no-wait \
    --name SalesVM \
    --location northeurope \
    --vnet-name SalesVNet \
    --subnet Apps \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password vTd5X[~m5z-n
```

```bash
 #2 create another Ubuntu VM in the Apps subnet of MarketingVNet
 az vm create \
    --resource-group vnet-related \
    --no-wait \
    --name MarketingVM \
    --location northeurope \
    --vnet-name MarketingVNet \
    --subnet Apps \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password vTd5X[~m5z-n

```

```bash
#3 create an Ubuntu VM in the Data subnet of ResearchVNet. 
az vm create \
    --resource-group vnet-related \
    --no-wait \
    --name ResearchVM \
    --location westeurope \
    --vnet-name ResearchVNet \
    --subnet Data \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password vTd5X[~m5z-n

```

```bash
# 4 To confirm that the VMs are running
watch -d -n 5 "az vm list \
    --resource-group vnet-related \
    --show-details \
    --query '[*].{Name:name, ProvisioningState:provisioningState, PowerState:powerState}' \
    --output table"

```


## Create virtual network peering connections
```bash
# 1 create the peering connection between the SalesVNet and MarketingVNet virtual networks
az network vnet peering create \
    --name SalesVNet-To-MarketingVNet \
    --remote-vnet MarketingVNet \
    --resource-group vnet-related \
    --vnet-name SalesVNet \
    --allow-vnet-access
```

```bash
# 2 create a reciprocal connection from MarketingVNet to SalesVNet
az network vnet peering create \
    --name MarketingVNet-To-SalesVNet \
    --remote-vnet SalesVNet \
    --resource-group vnet-related \
    --vnet-name MarketingVNet \
    --allow-vnet-access
```

## Check the virtual network peering connections
```bash
# check the connection between SalesVNet and MarketingVNet.
az network vnet peering list \
    --resource-group vnet-related \
    --vnet-name SalesVNet \
    --output table

```

## Check effective routes

```bash
# look at the routes that apply to the SalesVM network interface.
az network nic show-effective-route-table \
    --resource-group vnet-related \
    --name SalesVMVMNic \
    --output table
```
