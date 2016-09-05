# cOctopus - An updated DSC resource for configuring Octopus tentacles 

The OctopusDSC resource today has a few issues: it's not on PSGallery, not being maintained, requires the tentacle IP and requires manual fixes to be able to used together with Azure Automation (Pull server). 

The intention of this community extension is to make a resource using  the OctopusDSC as an inspiration; and add the features needed and pushing it to PSGallery so they can be used seemleasly with the PS tooling support that comes with WMF5 and through the Azure portal. 

Also providing some better examples / documentation on how to automate and configure the installation of the tenacle prior to configuring it. 

# Installing tentacle - MSI - default DSC resources 
To install the node (required for being able to configure and use this module) and assuming you've imported Import-DSCResource xPSDesiredStateConfiguration, you can easily add the following for installing the MSI package deliver from Octopus. 

    File OctopusTentacle 
    {
      DestinationPath = "C:\Octopus\Tentacle.msi"
      SourcePath = "\\Server\InstallationFiles\Tentacle.msi"
      Ensure = "Present"
      Type = "File"
      Credential = $myCredential
      Checksum = "modifiedDate"
      Force = $true
      MatchSource = $true
    }
      
    Package OctopusDeployTentacle
    { 
      Name = 'Octopus Deploy Tentacle' 
      DependsOn = '[File]OctopusTentacle'
      Ensure = 'Present' 
      Path = 'C:\Octopus\Tentacle.msi' 
      ProductId = "6E3B06FB-FC97-4C5C-AC27-91C5DA3F73E4" # 2.6 - Find the right product ID for your version
    }

# Configuring tentacle - using this module
