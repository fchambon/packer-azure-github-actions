# packer-azure-github-actions

- Build d'une image W10 EVD avec installation des M365 Apps et paramétrage de FSLogix
    ● Prérequis:
        o Créer un SPN avec le rôle Contributeur sur le groupe de ressources dans lequel l'image sera déployée: az ad sp create-for-rbac -n "MyPackerSPN" --role Contributor --scopes="/subscriptions/SUBSCRIPTION_ID/resourceGroups/{ResourceGroup1}" --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
        o Créer des secrets GitHub Actions pour le SPN ID / PASSWORD / SUBSCRIPTION ID / TENANT ID

    ● Trouver les images de machines virtuelles Windows Azure
        o Packer a besoin des informations d'image du système d'exploitation pour construire la VM de référence:
              o Publisher: L'organisation qui a crée l'image (exemple: MicrosoftWindowsServer, MicrosoftWindowsDesktop)
              o Offer: Le nom d'un groupe d'image associées créées par un éditeur (exemple: WindowsServer, Windows-10)
              o SKU: L'instance d'une offre comme la release majeure d'une distribution (exemple: 2019-Datacenter, 20h2-evd-o365pp)
              o Version: Le numéro de version d'une SKU

        o Pour lister les images:
        #Se Connecter à Azure via le Cloud Shell (PowerShell) ou Azure PowerShell CLI
            Connect-AzAccount
            $location = "North Europe"

        #Lister les publishers
            Get-AzVMImagePublisher -Location $location | Select PublisherName | Where-Object { $_.PublisherName -like '*Windows*' }
        
        #Lister les offres
            $publisher = "MicrosoftWindowsDesktop"
            Get-AzVMImageOffer -Location $location -PublisherName $publisher | Select Offer

         #Lister les SKUs
            $offer = "windows-10-20h2-vhd-client-office-prod-stage"
            Get-AzVMImageSku -Location $location -PublisherName $publisher -Offer $offer | Select Skus










