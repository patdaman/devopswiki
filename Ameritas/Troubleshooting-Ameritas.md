## Development

Documentation on development environment found here:  
[Directories Setup](https://dev.azure.com/questanalytics/Payer%20Products/_wiki/wikis/PayerProducts.wiki/55/Ameritas)
___
## Unresponsive Web Apps  
The [Ameritas Web App](https://dev.azure.com/questanalytics/Payer%20Products/_git/Directories?path=%2FDirectories.Ameritas) is the [Ameritas Find A Provider](https://dentalnetwork.ameritas.com/) web site.  It may stall and fail to load in a timely manner.  This is most often due to high CPU usage on the Virtual Machine Scale Set.  

* [Production metrics](https://grafana.dev.questanalytics.com:3000/d/7x_ntRZGk/prod-directories-new?orgId=1) found in Grafana will show you a high CPU load.
* Grafana will send an alert to [OpsGenie]() when the __*Average per minute*__ load on the scale set is above a threshold (currently 85%).

![Directories Architecture](/images/Ameritas_CPU.jpg)
___
## Unable to load providers

![Ameritas_Valid_Indices Index Missing](/images/Ameritas_Index.jpg)
___
## Dental Cost Estimator fails to load

[Dental Cost Estimator](https://dentalnetwork.ameritas.com/dceoutofnetwork) will most often fail to load when the public load balancer is unable to talk to Ameritas's API.  

![Ameritas API](/images/Ameritas_API.jpg)

## Services
