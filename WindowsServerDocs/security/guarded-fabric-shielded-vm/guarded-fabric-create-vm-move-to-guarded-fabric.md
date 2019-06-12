---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 受防護的 Vm，租用戶-建立新的受防護的 VM 在內部，並將它移至受防護網狀架構
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: dd9b89f34a3b4af8bb98d2399a524790aa65de0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447481"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>受防護的 Vm，租用戶-建立新的受防護的 VM 在內部，並將它移至受防護網狀架構

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


<!-- NOTE THAT THIS FILE HAS A "redirect_url" LINE IN THE METADATA. EVENTUALLY WE WILL PROBABLY STRIP OUT THE DETAILED METADATA AND THE CONTENT BELOW, SO IT'S PURELY A REDIRECTED TOPIC. However, as of mid-November 2016, we're still deciding. -->



本主題描述如何建立受防護的 VM，使用只有 HYPER-V;也就是不含 Virtual Machine Manager、 範本或虛擬機器防護資料檔案。 這是大多數公用雲端裝載環境中，常見案例，但測試受防護網狀架構時可能會很有用，或在企業中，VM 會被移部門來光纖中的情況下共用 IT 基礎結構，而且必須移轉之前，先加密。

若要了解本主題如何放入部署受防護的 Vm 的整體程序，請參閱[裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>匯入租用戶的 HYPER-V 伺服器上的守護者組態

1.  開始之前的程序，請確定您是執行 Windows Server 2016 和下列角色和功能安裝在 HYPER-V 主機上：

    - [角色]

        - Hyper-V

    - 功能

        - 遠端伺服器管理工具\\功能系統管理工具\\受防護的 VM 工具

    > [!NOTE]
    > 此處所使用的主機應該*不*是受防護網狀架構中的主機。 這是個別的主控件，才能移至受防護網狀架構中已經到 Vm。

2.  您可以在這部電腦上執行受防護的 VM 之前，您必須確保虛擬化型安全性已啟用且正在執行。 執行下列命令，在提升權限的 Windows PowerShell 視窗安裝主機守護者 HYPER-V 支援功能。 這會在您的電腦能夠執行受防護的 Vm 上設定所有必要的設定。

        Install-WindowsFeature HostGuardian

3.  您必須為您的 VM 執行所在的受防護網狀架構中取得守護者中繼資料。 此中繼資料用來授權網狀架構執行受防護的 VM。 您要如何取得這項資訊會針對每個主機服務提供者或企業的不同。 主機服務提供者 （或您，如果您有存取權受防護網狀架構網路） 可以藉由執行下列 Windows PowerShell 命令來取得這項資訊：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    在上述範例中，「 hgs"HGS 叢集中的分散式的網路名稱而 「 bastion.local"HGS 網域的名稱。

4.  若要匯入守護者金鑰，您將需要在稍後的程序中，執行下列命令。

    針對&lt;路徑&gt;&lt;檔案名稱&gt;、 替代路徑和檔案名稱的 xml 檔案儲存在先前步驟中，例如：**C:\\temp\\GuardianKey.xml**

    針對&lt;GuardianName&gt;，指定的名稱，為您的主機服務提供者或企業資料中心，例如**HostingProvider1**。 資料錄下一個程序的名稱。

    包含 **-AllowUntrustedRoot**只有當 HGS 伺服器所設定的自我簽署憑證。 （這些憑證是在 HGS 金鑰保護服務的一部分）。

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>在主機上建立新的受防護的虛擬機器

在此程序中，您會在 HYPER-V 主機上，建立虛擬機器，並準備進行匯出至您的主機服務提供者或資料中心系統管理員，可以在受守護的主機執行。

程序的一部分，您將建立包含兩個重要的項目金鑰保護裝置：

-   **擁有者**:在金鑰保護裝置中，您或甚至您處理共用安全性項目，例如憑證的群組會識別為 「 擁有者 」 的 VM。 您身為擁有者的身分識別被以產生的憑證，如果您執行命令所示，是做為自我簽署的憑證。 （選擇性） 您可以改為使用憑證受到 PKI 基礎結構，並略過 **-AllowUntrustedRoot**命令中的參數。

-   **Guardians**:也在金鑰保護裝置中，您的主機服務提供者或企業資料中心 （它是 HGS 和受防護的主機） 識別為 「 守護者 」。 守護者由您在上一個程序中，匯入的守護者金鑰[匯入租用戶的 HYPER-V 伺服器上的守護者組態](#import-the-guardian-configuration-on-the-tenant-hyper-v-server)。

如圖例中顯示的金鑰保護裝置，也就是防護資料檔案中的項目，請參閱 <<c0> [ 什麼防護資料，以及它為何如此必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

1. 在租用戶的 HYPER-V 主機上的 若要建立新第 2 代虛擬機器，執行下列命令。

   針對&lt;ShieldedVMname&gt;，指定 VM 的名稱，例如：**ShieldVM1**
    
   針對&lt;VHDPath&gt;，指定位置來儲存 VM 的 VHDX，例如：**C:\\Vm\\ShieldVM1\\ShieldVM1.vhdx**
    
   針對&lt;nnGB&gt;，例如 VHDX，指定的大小：**60GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. 安裝支援的作業系統 (Windows Server 2012 或更新版本，Windows 8 的用戶端或更高版本) 上的 VM 並啟用遠端桌面連線與對應的防火牆規則。 記錄 VM 的 IP 位址和/或 DNS 名稱;您必須從遠端連接到它。

3. 使用 RDP 從遠端連線至 VM，並確認 RDP 和防火牆已正確設定。 防護的程序的一部分，透過 HYPER-V 虛擬機器的主控台存取將會停用，因此務必要確保您能夠從遠端透過網路管理系統。

4. 若要建立新的金鑰保護裝置 （位於本節開頭所述），執行下列命令。

   針對&lt;GuardianName&gt;，使用在先前的程序，例如指定的名稱：**HostingProvider1**

   包含 **-AllowUntrustedRoot**以便進行自我簽署憑證。

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   如果您想針對多個資料中心能夠執行您在受防護的 VM （例如災害復原站台和公用雲端提供者） 時，您可以提供一份以 guardians **-守護者**參數。 如需詳細資訊，請參閱 [新增-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps。

5. 若要啟用 vTPM，使用金鑰保護裝置，請執行下列命令。 針對&lt;ShieldedVMname&gt;，使用在先前步驟中的相同 VM 名稱。

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. 若要啟動的 VM，以確認金鑰保護裝置使用本機的擁有者憑證，請執行下列命令。

       Start-VM -Name $VMName

7. 請確認 VM 已在 HYPER-V 主控台啟動。

8. 使用 RDP 從遠端連線至 VM，並在所有 Vhdx 附加至受防護的 VM 上的所有資料分割上啟用 BitLocker。

   > [!IMPORTANT]
   > 下一個步驟之前，等候完成啟用的所有資料分割上的 BitLocker 加密。

9. 當您準備好要將它移至受防護網狀架構，請關閉 VM。

10. 在租用戶的 HYPER-V 伺服器上，將匯出的 VM，使用您的選擇 （Windows PowerShell 或 HYPER-V 管理員） 的工具。 然後排列的檔案複製到您的主機服務提供者或企業資料中心所維護的受防護主機。

11. **主控提供者或企業資料中心**:

    匯入受防護的 VM 使用 HYPER-V 管理員或 Windows PowerShell。 若要啟動 VM，您必須從 VM 擁有者匯 VM 設定檔。 這是因為金鑰保護裝置和虛擬機器的虛擬 TPM 會儲存在組態檔。 如果 VM 已在受防護網狀架構上執行，它應該能夠順利啟動。

## <a name="see-also"></a>另請參閱

- [裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
