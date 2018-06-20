# webappVnetIntegration
webapp to vnet integration

This is a fix to this script at https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3

fixed the dynamic subscription dropdown. 
```

#$subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))"; should be $subscriptionChoices += "$($subscription.Name) ($($subscription.SubscriptionId))";

```

Replaced the Login-AzureRmAccount with logic to see if you are already logged in
```

if ([string]::IsNullOrEmpty($(Get-AzureRmContext).Account)) {
    Write-Host "Please Login"
    Login-AzureRmAccount
}

```

You would be prompted to type in the resource group name for adding the certificate to the gateway. the cmdlet was not using the -resourcegroup parameter, so I added it in

```
            Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name -ResourceGroupName $gateway.ResourceGroupName

```
