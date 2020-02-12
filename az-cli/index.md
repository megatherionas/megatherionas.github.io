---
layout: default
---

# You should learn to use Azure CLI! And use it with extensions 
The x-plat Azure CLI command line tools are an extremely powerful toolbox you can use when it comes to deployment, querying and management of Azure resources. With the CLI you have command line access to a majority (most) of the services Azure provide. 

I am still somewhat new to the Azure CLI and have veen a "hardcore fan" of PowerShell (the best command line tool available - that doesn't need grep to understand its command output) and Azure PowerShell. I've spent many hours with these cmdlets. First using ASM (Azure Service Manager) then with ARM and the AzureRM (Azure Resource Manager). I'm still a big fan of PowerShell. Tt's latest iteration, PowerShell Core, I think will be a strong contender to existing shells on any OS platform in the future.

Anyhow.... I digress, back on topic.

There is not much of a controversy saying that the winner of "best azure command line tool" is Azure CLI. 
On traditional windows platforms the previous PowerShell AzureRM module has been replaced by the Az module to make it more aligned with Azure Cli in features but still.. 
It's biggest issue to date is that it is confined to Powershell while Azure CLI can be used in most (if not all) Linux/Mac shell environments including PowerShell Core and Windows PowerShell. 

In the beginning, when I started learning the CLI syntax I found it confusing and not very intuitive at all. The traditional AzureRM (and Az)PowerShell command syntax felt much safer and easier to understand, I felt more human readable somehow. Over time I mastered the structure of CLI syntax and smile at how much more productive I have become using it.

Below is three examples on how you would use three simple commands, login to an account and list all resource groups in the default subscription and create a resource group in northeurope.


```powershell
#Azure CLI

PS C:\tmp>az login
PS C:\tmp>az group list
PS C:\tmp>az group create -n "MyRg" -l "northeurope"
```
For the parameters -n(ame) and -l(ocation) you can decide yourself if you want to use the long or short version of the params.

```powershell
# PowerShell Az

PS C:\tmp>Login-AzAccount
PS C:\tmp>Get-AzResourceGroup
PS C:\tmp>New-AzResourceGroup -Name "MyRg" -Location "northeurope"
```

You don't need to write a lot of code to see that there is a definitive productivity boost in using the CLI, given that you have invested time in learning the syntax. In my opinion the CLI syntax is just as verbose as PowerShell Az is, shorter but smarter.

The absolute biggest argument for the CLI is it's cross shell/platform support, unlike PowerShell Az that requires a version of PowerShell. PowerShell Core runs on windows,linux and mac, but it's not always possible to install a full blown shell on a machine, and if you are a lifetime ninja of bash you should be able to use that if that is your preference. It is really nice to know that I can do my job no matter what computer OS i happen to have available, the syntax is the sam.

Talking about Bash.... If you are planning to write and run automation scripts against the Azure management apis from a workstation or server I would definitely consider installing PS Core. It's capabilities to work with objects and not just text that need to be grep'ed. I'd dread to do this in Bash

```powershell
Get-AzResourceGroup | Select-Object -Expand $_ `
    | foreach-object -Process  { `
        Write-Host ("'{0}' has provisioning state '{1}' with unique id '{2}'" -f  `
                                                                        $_.ResourceGroupName, `
                                                                        $_.ProvisioningState, `
                                                                        $_.ResourceId )
}
```

PowerShell scripts can also work with Azure CLI, but it require a bit more work to to parse the command response and would look something like this.

```powershell
$json = $(az group list --output json)|ConvertFrom-Json | Select-Object -Expand $_
$json | Select Id, Name | foreach-object -Process {
    $rg = $_.Name
    Write-Host "Resource Group: " $rg 
}
```
but both of the above script samples will run in any PowerShell console.


Finally, with the rapid evolution cloud services, enhancements and new services there is no way that the dev team that maintain the Azure CLI will be able to keep up. A really nice feature in the CLI is it's ability to use extensions to add capabilities. For instance it's not possible to work with Azure Firewalls with the default install package for CLi, but if you  use 

```powershell
PS C:\tmp>az extension add --name azure-firewall
```
you will have the ability to create and manage firewalls through the console. To see a list of available extensions use the command 

```powershell
PS C:\tmp>az extension list-available --output json
```
A couple of my favorites are the "interactive" extension that gives you a much richer console window with intellisense. Still in preview but looks promising. the azure-devops extension lets you manage most parts of Azure DevOps pipelines, repos,projects and teams. 

All in all I think that investing time to learn and having this command line syntax skill will make you a lot more productive. As a bonus you won't have to wait for all that javascript and HTML to finally load and render on the portal just because you clicked a wrong link... You also get more sensible feedback if you append --debug or --verbose to your commands. 

happy command lining! :)

