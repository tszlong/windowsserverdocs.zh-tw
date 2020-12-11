---
title: 建立 Linux 受防護的 VM 範本磁片
description: 深入瞭解：建立 Linux 受防護的 VM 範本磁片
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: f17753cc423be342f1b732a66e2a1e7ffd40d782
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049816"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>建立 Linux 受防護的 VM 範本磁片

> 適用于： Windows Server 2019、Windows Server (半年通道) 、

本主題說明如何準備可用於將一或多個租使用者 Vm 具現化之 Linux 受防護 Vm 的範本磁片。

## <a name="prerequisites"></a>必要條件

若要準備及測試 Linux 受防護的 VM，您將需要下列可用資源：

- 具有執行 Windows Server 1709 版或更新版本之虛擬化 capababilities 的伺服器
- 第二部電腦 (Windows 10 或 Windows Server 2016) 能夠執行 Hyper-v 管理員以連接到執行中的 VM 主控台
- 其中一個支援的 Linux 受防護 VM 作業系統的 ISO 映像：
    - 具有4.4 核心的 Ubuntu 16.04 LTS
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- 下載 lsvmtools 套件和 OS 更新的網際網路存取

> [!IMPORTANT]
> 舊版 Linux Os 的較新版本可能包含已知的 TPM 驅動程式錯誤，這將會導致其無法成功布建為受防護的 Vm。
> 建議您將範本或受防護的 Vm 更新至較新的版本，直到有可用的修正。
> 當更新設為公開時，上述支援的作業系統清單將會更新。

## <a name="prepare-a-linux-vm"></a>準備 Linux VM

受防護的 Vm 是從安全範本磁片建立。
範本磁片包含 VM 的作業系統和中繼資料，包括/boot 和/root 磁碟分割的數位簽章，以確保在部署之前不會修改核心 OS 元件。

若要建立範本磁片，您必須先建立一般 (非遮罩) VM，您將準備做為未來受防護 Vm 的基礎映射。
您所安裝的軟體和您對此 VM 所做的設定變更，將會套用到從這個範本磁片建立的所有受防護 Vm。
這些步驟將逐步引導您完成可供 templatization Linux VM 的最低需求。

> [!NOTE]
> 磁片分割時，會設定 Linux 磁片加密。
> 這表示您必須建立使用 dm crypt 預先加密的新 VM，以建立 Linux 受防護的 VM 範本磁片。


1.  在虛擬化伺服器上，藉由在提高許可權的 PowerShell 主控台中執行下列命令，確定已安裝 Hyper-v 和主機守護者 Hyper-v 支援功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  從值得信任的來源下載 ISO 映像，並將它儲存在您的虛擬化伺服器上，或儲存在虛擬化伺服器可存取的檔案共用上。

3.  在執行 Windows Server 1709 版的管理電腦上，執行下列命令來安裝受防護的 VM 遠端伺服器管理工具：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  在管理電腦上開啟 [ **Hyper-v 管理員** ]，並連接到您的虛擬化伺服器。
    您可以按一下 [連線到伺服器 ...]在 [動作] 窗格中，或以滑鼠右鍵按一下 [Hyper-v 管理員]，然後選擇 [連線到伺服器 ...]提供 Hyper-v 伺服器的 DNS 名稱，如有必要，請提供連接到該伺服器所需的認證。

5.  使用 Hyper-v 管理員，在虛擬化伺服器上 [設定外部交換器](../../virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md) ，讓 Linux VM 可以存取網際網路以取得更新。

6.  接下來，建立新的虛擬機器，以在上安裝 Linux OS。
    在 [動作] 窗格中，按一下 [**新增**  >  **虛擬機器**] 以顯示嚮導。
    為您的 VM 提供易記的名稱，例如「預先範本化的 Linux」，然後按 **[下一步]**。

7.  在嚮導的第二個頁面上，選取第 **2 代** ，以確定 VM 已布建以 UEFI 為基礎的固件設定檔。

8.  根據您的喜好設定來完成嚮導的其餘部分。
    請勿針對此 VM 使用差異磁片;受防護的 VM 範本磁片無法使用差異磁片。
    最後，將您稍早下載的 ISO 映像連接到此 VM 的虛擬 DVD 光碟機，如此您就可以安裝 OS。

9.  在 [Hyper-v 管理員] 中，選取新建立的 VM，然後按一下 [執行] 窗格中的 [連線]，以 **連結** 至 VM 的虛擬主控台。
    在出現的視窗中，按一下 [ **開始** ] 以開啟虛擬機器。

10. 針對您選取的 Linux 發行版本進行設定程式。
    雖然每個 Linux 發行版本都使用不同的安裝程式，但將成為 Linux 受防護 VM 範本磁片的 Vm 必須符合下列需求：

    - 磁片必須使用 GUID 分割資料表 (GPT) 配置進行分割
    - 根磁碟分割必須使用 dm crypt 進行加密。 複雜密碼應該設定為所有小寫)  (複雜 **密碼** 。 當布建受防護的 VM 時，此複雜密碼將會是隨機的，而且會重新加密資料分割。
    - 開機磁碟分割必須使用 **ext2** 檔案系統

11. 當您的 Linux 作業系統完全啟動並登入之後，建議您安裝 linux 虛擬核心和相關聯的 Hyper-v integration services 套件。
    此外，您也會想要安裝 SSH 伺服器或其他遠端系統管理工具，以在 VM 受到防護後存取 VM。

    在 Ubuntu 上，執行下列命令以安裝這些元件：

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    在 RHEL 上，改為執行下列命令：

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    在 SLES 上，執行下列命令：

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. 視需要設定您的 Linux OS。
    您所安裝的任何軟體、新增的使用者帳戶，以及您所做的全系統設定變更，將會套用到所有未來從這個範本磁片建立的 Vm。
    您應該避免將任何秘密或不必要的套件儲存到磁片。

13. 如果您打算使用 System Center Virtual Machine Manager 部署 Vm，請安裝 VMM 來賓代理程式，讓 VMM 在 VM 布建期間將您的 OS 特殊化。
    特製化可讓每個 VM 安全地設定不同的使用者和 SSH 金鑰、網路設定和自訂設定步驟。
    瞭解如何在 VMM 檔中 [取得和安裝 vmm 來賓代理程式](/system-center/vmm/vm-linux#install-the-vmm-guest-agent) 。

14. 接下來， [將 Microsoft Linux 軟體存放庫新增至您的套件管理員](../../administration/linux-package-repository-for-microsoft-software.md)。

15. 使用您的套件管理員，安裝包含 Linux 受防護 VM 開機載入器填充碼、布建元件和磁片準備工具的 lsvmtools 套件。

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. 當您完成自訂 Linux 作業系統之後，請在您的系統上找出 lsvmprep 安裝程式並加以執行。

    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. 關閉您的 VM。

16. 如果您已取得 VM 的任何檢查點 (包括由 Hyper-v 建立的自動檢查點與 Windows 10 Fall Creators Update) ，請務必先將其刪除，再繼續進行。
    檢查點會 ( 範本磁片嚮導不支援的 .avhdx) 建立差異磁片。

    若要刪除檢查點，請開啟 [ **Hyper-v 管理員**]，選取您的 VM，在 [檢查點] 窗格中以滑鼠右鍵按一下最上層檢查點，然後按一下 [**刪除檢查點子樹**

    ![在 Hyper-v 管理員中刪除範本 VM 的所有檢查點](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>保護範本磁片

您在上一節中準備的 VM 幾乎已準備好用來做為 Linux 受防護的 VM 範本磁片。
最後一個步驟是透過範本 Disk Wizard 執行磁片，這將會雜湊並以數位方式簽署根目錄和開機磁碟分割的目前狀態。
布建受防護的 VM 時，會驗證雜湊和數位簽章，以確保在建立和部署範本之間，不會對兩個磁碟分割進行未經授權的變更。

### <a name="obtain-a-certificate-to-sign-the-disk"></a>取得憑證以簽署磁片

若要以數位方式簽署磁片測量，您必須在將執行範本磁片嚮導的電腦上取得憑證。
憑證必須符合下列要求：

Certificate 屬性 | 必要值
---------------------|---------------
金鑰演算法 | RSA
最小金鑰大小 | 2048 位元
簽章演算法 | SHA256 (建議的) 
金鑰使用方式 | 數位簽章

當租使用者建立防護資料檔案並授權其信任的磁片時，將會向租使用者顯示此憑證的詳細資料。
因此，請務必從您和您的租使用者所信任的憑證授權單位單位取得此憑證。
在您同時是主機服務提供者和租使用者的企業案例中，您可以考慮從企業憑證授權單位單位發行此憑證。
請小心保護此憑證，因為擁有此憑證的任何人都可以建立信任和您的真實磁片相同的新範本磁片。

在測試實驗室環境中，您可以使用下列 PowerShell 命令來建立自我簽署憑證：

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>使用範本 Disk Wizard Cmdlet 處理磁片

將您的範本磁片和憑證複製到執行 Windows Server 1709 版的電腦，然後執行下列命令以起始簽署程式。
您提供給參數的 VHDX `-Path` 將會以更新的範本磁片覆寫，因此請務必在執行命令之前建立複本。

> [!IMPORTANT]
> Windows Server 2016 或 Windows 10 上提供的遠端伺服器管理工具不能用來準備 Linux 受防護的 VM 範本磁片。
> 請只使用 windows server 2019 1709 版或遠端伺服器管理工具版上提供的 [TemplateDisk](/powershell/module/shieldedvmtemplate/protect-templatedisk) 指令，來準備 Linux 受防護的 VM 範本磁片。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

您的範本磁片現在已準備好用來布建 Linux 受防護的 Vm。
如果您使用 System Center Virtual Machine Manager 部署 VM，您現在可以將 VHDX 複製到 VMM 程式庫。

您也可能想要從 VHDX 解壓縮磁片區簽章目錄。
此檔案是用來將簽署憑證、磁片名稱和版本的相關資訊提供給想要使用範本的 VM 擁有者。
他們必須將此檔案匯入防護資料檔案嚮導，以授權您（擁有簽署憑證的範本作者）為其建立此和未來的範本磁片。

若要解壓縮磁片區簽章目錄，請在 PowerShell 中執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
