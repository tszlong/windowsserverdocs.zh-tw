---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 受防護的租使用者 Vm-在內部部署建立新的受防護 VM，並將其移至受保護的網狀架構
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c6753ae5d767f0c71b86fc47c1d8bf9971a2a5cc
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475525"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>受防護的租使用者 Vm-在內部部署建立新的受防護 VM，並將其移至受保護的網狀架構

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

本主題說明使用 Hyper-v 建立受防護 VM 的步驟;也就是說，沒有 Virtual Machine Manager、範本磁片或防護資料檔案。 這不是大多數公用雲端裝載環境的罕見案例，但在測試受防護網狀架構或在企業案例中，如果 VM 是從部門結構移至共用的 IT 基礎結構，而且必須在進行遷移之前進行加密，則可能會很有用。

若要瞭解本主題如何適用于部署受防護 Vm 的整體程式，請參閱[主機服務提供者設定步驟中的受防護主機和受防護的 vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)。

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>在租使用者 Hyper-v 伺服器上匯入守護者設定

1.  開始此程式之前，請確定您是在執行 Windows Server 2016 的 Hyper-v 主機上，且已安裝下列角色和功能：

    - 角色

        - Hyper-V

    - 特性

        - 遠端伺服器管理工具 \\ 功能管理工具 \\ 受防護的 VM 工具

    > [!NOTE]
    > 此處使用的主機*不*應該是受防護網狀架構中的主機。 這是個別的主機，Vm 會在移至受防護網狀架構之前備妥。

2.  在這部電腦上執行受防護的 VM 之前，您必須確保虛擬化型安全性已啟用且正在執行。 在提高許可權的 Windows PowerShell 視窗中執行下列命令，以安裝主機守護者 Hyper-v 支援功能。 這會在您的電腦上設定所有必要的設定，使其能夠執行受防護的 Vm。

        Install-WindowsFeature HostGuardian

3.  您將需要取得 VM 將在其中執行的受防護網狀架構的守護者中繼資料。 此中繼資料是用來授權該網狀架構執行受防護的 VM。 針對每個主機服務提供者或企業，您取得這項資訊的方式會有所不同。 主機服務提供者（如果您有權存取受保護網狀架構網路，則可以透過執行下列 Windows PowerShell 命令來取得此資訊）：

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    在上述範例中，"hgs" 是 HGS 叢集的分散式網路名稱，而 "防禦" 是 HGS 網域的名稱。

4.  若要匯入在稍後的程式中需要的監護人金鑰，請執行下列命令。

    針對 [ &lt; 路徑 &gt; &lt; 檔案名] &gt; ，以您在上一個步驟中儲存之 XML 檔案的路徑和檔案名取代，例如： **C： \\ temp \\GuardianKey.xml**

    針對 &lt; GuardianName &gt; ，指定主機服務提供者或企業資料中心的名稱，例如**HostingProvider1**。 記錄下一個程式的名稱。

    只有在 HGS 伺服器已設定自我簽署憑證時，才包含 **-AllowUntrustedRoot** 。 （這些憑證是 HGS 中的金鑰保護服務的一部分）。

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>在主機上建立新的受防護虛擬機器

在此程式中，您將會在 Hyper-v 主機上建立虛擬機器，並準備將它匯出至您的主機服務提供者或資料中心系統管理員，以便在受防護的主機上執行。

在此過程中，您將建立包含兩個重要元素的金鑰保護裝置：

-   **擁有**者：在金鑰保護裝置中，您所使用的群組（或更有可能）共用安全性元素（例如憑證）會被識別為 VM 的「擁有者」。 身為擁有者的身分識別是由憑證所表示，如果您執行如所示的命令，會產生為自我簽署憑證。 （選擇性）您可以改為使用 PKI 基礎結構支援的憑證，並在命令中省略 **-AllowUntrustedRoot**參數。

-   **監護人**：在金鑰保護裝置中，您的主機服務提供者或企業資料中心（執行 HGS 和受防護主機）會被識別為「守護者」。 系統會以您在上一個程式中匯入的守護者金鑰來表示守護者，並在[租使用者 hyper-v 伺服器上匯入守護者](#import-the-guardian-configuration-on-the-tenant-hyper-v-server)設定。

如需顯示金鑰保護裝置（也就是防護資料檔案中的元素）的圖例，請參閱[什麼是防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

1. 在租使用者 Hyper-v 主機上，若要建立新的第2代虛擬機器，請執行下列命令。

   針對 &lt; ShieldedVMname &gt; ，指定 VM 的名稱，例如： **ShieldVM1**

   針對 &lt; VHDPath &gt; ，指定要儲存 VM VHDX 的位置，例如： **C： \\ vm \\ ShieldVM1 \\ ShieldVM1 .vhdx。**

   針對 &lt; [nnGB] &gt; ，指定 VHDX 的大小，例如： **60GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. 在 VM 上安裝支援的作業系統（Windows Server 2012 或更新版本、Windows 8 用戶端或更新版本），並啟用 [遠端桌面連線] 和對應的防火牆規則。 記錄 VM 的 IP 位址和（或） DNS 名稱;您將需要它以遠端方式連接到它。

3. 使用 RDP 從遠端連線至 VM，並確認已正確設定 RDP 和防火牆。 在防護程式中，透過 Hyper-v 對虛擬機器的主控台存取將會停用，因此請務必確保您能夠從網路遠端管理系統。

4. 若要建立新的金鑰保護裝置（如本節開頭所述），請執行下列命令。

   針對 &lt; GuardianName &gt; ，請使用您在上一個程式中指定的名稱，例如： **HostingProvider1**

   包含 **-AllowUntrustedRoot**可允許自我簽署憑證。

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   如果您想要讓一個以上的資料中心能夠執行受防護的 VM （例如，嚴重損壞修復網站和公用雲端提供者），您可以將一份監護人清單提供給 **-監護人**參數。 如需詳細資訊，請參閱 [新增-HgsKeyProtector] （ https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps 。

5. 若要使用金鑰保護裝置來啟用 vTPM，請執行下列命令。 針對 &lt; ShieldedVMname &gt; ，請使用先前步驟中使用的相同 VM 名稱。

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. 若要啟動 VM 以確認金鑰保護裝置正在使用本機擁有者憑證，請執行下列命令。

       Start-VM -Name $VMName

7. 確認 VM 已在 Hyper-v 主控台中啟動。

8. 使用 RDP 從遠端連線到 VM，並在所有連接到受防護 VM 的 Vhdx 上，啟用所有分割區上的 BitLocker。

   > [!IMPORTANT]
   > 繼續進行下一個步驟之前，請等候 BitLocker 加密在您啟用它的所有分割區上完成。

9. 當您準備好要將 VM 移至受防護的網狀架構時，請將它關閉。

10. 在租使用者 Hyper-v 伺服器上，使用您選擇的工具（Windows PowerShell 或 Hyper-v 管理員）匯出 VM。 然後排列要複製到裝載提供者或企業資料中心維護之受防護主機的檔案。

11. **針對主控提供者或企業資料中心**：

    使用 Hyper-v 管理員或 Windows PowerShell 匯入受防護的 VM。 您必須從 VM 擁有者匯入 VM 設定檔，才能啟動 VM。 這是因為金鑰保護裝置和 VM 的虛擬 TPM 會儲存在設定檔中。 如果 VM 設定為在受防護網狀架構上執行，則應該能夠順利啟動。

## <a name="additional-references"></a>其他參考

- [適用於受防護主機和受防護 VM 的託管服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
