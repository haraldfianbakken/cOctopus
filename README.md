# cOctopus - An updated DSC resource for configuring Octopus tentacles 

The OctopusDSC resource today has a few issues ; it's not on PSGallery, requires the tentacle IP and is requires manual fixes to be able to used together with Azure Automation (Pull server). 

This resource uses the OctopusDSC as an inspiration, but adds these features and is pushed and published to the PSGallery so they can be used
seemleasly with the PS tooling support that comes with WMF5 and through the Azure portal. . 

# Usage

Installing the tentacle without configuring (using default resources)

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
