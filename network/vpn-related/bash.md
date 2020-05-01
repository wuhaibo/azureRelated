## Prepare Azure and on-premises virtual networks by using Azure CLI commands

### Create the Azure-side resources
```bash
# create the Azure-VNet-1 virtual network and the Services subnet.
az network vnet create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name Azure-VNet-1 \
    --address-prefix 10.0.0.0/16 \
    --subnet-name Services \
    --subnet-prefix 10.0.0.0/24
```

```bash
#  create the LNG-HQ-Network local network gateway.
az network vnet subnet create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --vnet-name Azure-VNet-1 \
    --address-prefix 10.0.255.0/27 \
    --name GatewaySubnet

```

```bash
# create the LNG-HQ-Network local network gateway.
az network local-gateway create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --gateway-ip-address 94.0.252.160 \
    --name LNG-HQ-Network \
    --local-address-prefixes 172.16.0.0/16
```

### Create the simulated on-premises network and supporting resources
```bash
 # create the HQ-Network virtual network and the Applications subnet.
az network vnet create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name HQ-Network \
    --address-prefix 172.16.0.0/16 \
    --subnet-name Applications \
    --subnet-prefix 172.16.0.0/24
```

```bash
# to add GatewaySubnet to HQ-Network.
az network vnet subnet create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --address-prefix 172.16.255.0/27 \
    --name GatewaySubnet \
    --vnet-name HQ-Network
```

```bash
#  create the LNG-Azure-VNet-1 local network gateway.
az network local-gateway create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --gateway-ip-address 94.0.252.160 \
    --name LNG-Azure-VNet-1 \
    --local-address-prefixes 10.0.0.0/16

```

## Create a site-to-site VPN gateway by using Azure CLI commands

### Create the Azure-side VPN gateway
```bash
# create the PIP-VNG-Azure-VNet-1 public IP address
az network public-ip create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name PIP-VNG-Azure-VNet-1 \
    --allocation-method Dynamic

```

```bash

# to create the VNG-Azure-VNet-1 virtual network gateway

az network vnet-gateway create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name VNG-Azure-VNet-1 \
    --public-ip-address PIP-VNG-Azure-VNet-1 \
    --vnet Azure-VNet-1 \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait

```

### Create the on-premises VPN gateway

```bash
# to create the PIP-VNG-HQ-Network public IP address
az network public-ip create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name PIP-VNG-HQ-Network \
    --allocation-method Dynamic

```

```bash
# to create the VNG-HQ-Network virtual network gateway.
az network vnet-gateway create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name VNG-HQ-Network \
    --public-ip-address PIP-VNG-HQ-Network \
    --vnet HQ-Network \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait
```

```bash
# wait until provisioningState = succeeded
watch -d -n 5 az network vnet-gateway list \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --output table

```

### config local gateway ip
```bash
# presudo code, set via azure portal
LNG-Azure-VNet-1.ip = PIP-VNG-Azure-VNet-1.ip
LNG-HQ-Network.ip = PIP-VNG-HQ-Network.ip
```


### Create the connections
```bash

# set shared key
SHAREDKEY=williamwood

# set connection from azure to on-premise
az network vpn-connection create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name Azure-VNet-1-To-HQ-Network \
    --vnet-gateway1 VNG-Azure-VNet-1 \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-HQ-Network

# set connection from on premise to azure 
az network vpn-connection create \
    --resource-group learn-cadb6b5a-5811-498d-965b-33356062dde3 \
    --name HQ-Network-To-Azure-VNet-1  \
    --vnet-gateway1 VNG-HQ-Network \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-Azure-VNet-1
```


### test the connection
deploy vm to HQ-Network/Applications
and Azure-VNet-1/Services to see if the vms can connect to each other. 