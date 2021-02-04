# packer-azure-github-actions

- Build d'une image W10 EVD avec installation des M365 Apps et paramétrage de FSLogix<br>
    ● Prérequis:<br>
        o Créer un SPN avec le rôle Contributeur sur le groupe de ressources dans lequel l'image sera déployée: az ad sp create-for-rbac -n "MyPackerSPN" --role Contributor --scopes="/subscriptions/SUBSCRIPTION_ID/resourceGroups/{ResourceGroup1}" --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"<br>
        o Créer des secrets GitHub Actions pour le SPN ID / PASSWORD / SUBSCRIPTION ID / TENANT ID<br><br>

    ● Trouver les images de machines virtuelles Windows Azure<br>
        o Packer a besoin des informations d'image du système d'exploitation pour construire la VM de référence:<br>
              o Publisher: L'organisation qui a crée l'image (exemple: MicrosoftWindowsServer, MicrosoftWindowsDesktop)<br>
              o Offer: Le nom d'un groupe d'image associées créées par un éditeur (exemple: WindowsServer, Windows-10)<br>
              o SKU: L'instance d'une offre comme la release majeure d'une distribution (exemple: 2019-Datacenter, 20h2-evd-o365pp)<br>
              o Version: Le numéro de version d'une SKU<br><br>

        o Pour lister les images:<br>
        #Se Connecter à Azure via le Cloud Shell (PowerShell) ou Azure PowerShell CLI<br>
            Connect-AzAccount<br>
            $location = "North Europe"<br><br>

        #Lister les publishers<br>
            Get-AzVMImagePublisher -Location $location | Select PublisherName | Where-Object { $_.PublisherName -like '*Windows*' }<br><br>
        
        #Lister les offres<br>
            $publisher = "MicrosoftWindowsDesktop"<br>
            Get-AzVMImageOffer -Location $location -PublisherName $publisher | Select Offer<br><br>

         #Lister les SKUs<br>
            $offer = "windows-10-20h2-vhd-client-office-prod-stage"<br>
            Get-AzVMImageSku -Location $location -PublisherName $publisher -Offer $offer | Select Skus<br>










