# traffic manager

## Traffic Manager uses dns to route the requests to endpoint.

## Traffic manager endpoints 
- Azure endpoints. Services in azure
- External endpoints. Services hosted outside azure.
- Nested endpoints. Complex structure. 
  
![alt](img/nestedstructure.png)


## Traffic Manager routing methods

1. Weighted routing. Each endpoints has a spedified rate from 1 to 1000.
   
    ![alt](img/weightedRouting.png)

2. Performance routing. Endpoints is weighted by response time. 
    ![alt](img/performanceRouting.png)

3. Geographic routing. The endpoints is chosen by the geo-location.
4. Priority routing. One endpoint as main the other as failover. 
5. Subnet routing. The requestes from a range of ip would be routed to specific endpoint.