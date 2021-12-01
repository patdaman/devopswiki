|NAME                  |ADDRESS RANGE|IPV4 AVAILABLE ADDRESSES|DESCRIPTION                                             |EXAMPLES                                                       |
|----------------------|-------------|------------------------|--------------------------------------------------------|---------------------------------------------------------------|
|ActiveDirectory       |10.xx.5.0/24 |251                     |Active Directory Domain Controllers                     |Domain Controllers Only                                        |
|APIManager            |10.xx.40.0/23|507                     |Dedicated Subnet for Azure API Managers                 |Azure API Managers                                             |
|ApplicationGateway    |10.xx.30.0/23|507                     |Dedicated Subnet for Azure Application Gateways         |Azure App Gateways (dedicated subnet per MS)                   |
|ApplicationGatewayV2  |10.xx.34.0/24|251                     |Dedicated Subnet for Azure V2 Application Gateways      |Azure V2 App Gateways (dedicated subnet per MS)                |
|Data                  |10.xx.20.0/22|1019                    |Internal SQL Virtual/Physical Servers                   |SQL Server                                                     |
|ExternalWeb           |10.xx.14.0/24|251                     |External facing UI Virtual/Physical Web Servers         |IIS/Apache Web Servers                                         |
|GatewaySubnet         |10.xx.0.0/24 |251                     |Azure Gateway Subnet                                    |Azure Gateway Subnet (dedicated subnet per MS)                 |
|InternalApp           |10.xx.12.0/24|251                     |Internal facing API/Service Virtual/Physical Web Servers|IIS/Apache API/Services Web Servers                            |
|InternalWeb           |10.xx.13.0/24|251                     |Internal facing UI Virtual/Physical Web Servers         |IIS/Apache Internal websites (Intranet)                        |
|Jumpbox               |10.xx.80.0/22|1019                    |Virtual Machines used to access resources               |Developer virtual machines/VMs to add additional security layer|
|ManagedAuth           |10.xx.88.0/23|507                     |Azure Authentication Services                           |KeyVault, App Config, Certs                                    |
|ManagedData           |10.xx.24.0/22|1019                    |Azure SQL Databases                                     |Azure SQL Databases                                            |
|ManagedExternalWeb    |10.xx.16.0/23|507                     |Azure App Service Plans for External facing UI          |External Azure Web Server                                      |
|ManagedInternalApp    |10.xx.10.0/23|507                     |Azure App Service Plans for Internal API/Services       |Internal Azure Web Server for API/Services                     |
|ManagedInternalService|10.xx.18.0/23|507                     |Azure Cloud Services (Classic)                          |                                                               |
|ManagedStorage        |10.xx.52.0/23|507                     |Azure Storage Accounts                                  |                                                               |
|Storage               |10.xx.50.0/23|507                     |Virtual/Physical File Servers and Appliances            |Local File Shares                                              |
|Tools                 |10.xx.84.0/22|1019                    |Various utilities, updates and controllers              |WSUS, Antivirus servers, network controllers                   |