---
title: 建立 Linux 受防護 VM 的範本磁碟
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858669"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>建立 Linux 受防護 VM 的範本磁碟

> 適用於：Windows Server 2019，Windows Server （半年通道） 

本主題說明如何準備 Linux 受防護的 Vm，可以用來具現化一或多個租用戶 Vm 的範本磁碟。

## <a name="prerequisites"></a>先決條件

若要準備和測試 Linux 受防護的 VM，您將需要下列資源：

- 具有執行 Windows Server 1709 版或更新版本的虛擬化 capababilities 的伺服器
- 第二部電腦 （Windows 10 或 Windows Server 2016） 能夠執行 HYPER-V 管理員來連接到執行中 VM 的主控台
- ISO 映像的其中一個支援的 Linux 受防護 VM 的作業系統：
    - 4.4 的核心使用的 Ubuntu 16.04 LTS
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- 網際網路存取才可下載 lsvmtools 套件和作業系統更新

> [!IMPORTANT]
> 較新版本的先前的 Linux Os 可能包含已知的 TPM 驅動程式錯誤因此無法成功佈建為受防護的 Vm。
> 不建議您更新您的範本，或以較新版本中受防護 Vm，有可用的修正程式為止。
> 公用進行更新時，將更新的支援上述的作業系統清單。

## <a name="prepare-a-linux-vm"></a>準備 Linux VM

從安全的範本磁碟會建立受防護的 Vm。
範本磁碟包含作業系統的 VM 和中繼資料，包括數位簽章 /boot 和 /root 分割區，以確保在部署前將不會修改核心 OS 元件。

若要建立的範本磁碟，您必須先建立一般的 （受防護） VM 會在未來受防護的 Vm 為基礎的映像準備。
您所安裝的軟體和您對此 VM 的組態變更會套用到所有受防護的 Vm，建立從這個範本磁碟。
這些步驟將引導您準備 templatization Linux VM 的裸機最低需求。

> [!NOTE]
> 分割磁碟時，會設定 Linux 磁碟加密。
> 這表示，您必須建立新的 VM 已預先加密使用 dm-crypt 來建立 Linux 受防護 VM 的範本磁碟。


1.  在虛擬化伺服器上，確認您安裝 HYPER-V 和主機守護者 HYPER-V 支援功能，在提升權限的 PowerShell 主控台中執行下列命令：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  從信任的來源下載 ISO 映像，並將它儲存在您虛擬化伺服器上，或您的虛擬化伺服器必須能夠在檔案共用上。

3.  您管理在電腦上執行 Windows Server 1709 版，請執行下列命令以安裝受防護的 VM 遠端伺服器管理工具：

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  開啟**HYPER-V 管理員**管理電腦上並連接到您的虛擬化伺服器。
    您可以依序按一下 [連接到伺服器...]在 [動作] 窗格中，或以滑鼠右鍵按一下 HYPER-V 管理員，然後選擇 [連線至伺服器...]提供您的 HYPER-V 伺服器的 DNS 名稱，並以如有必要，所需的認證連接到它。

5.  使用 HYPER-V 管理員 中，[設定外部交換器](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines)讓 Linux VM 能夠存取網際網路，以取得更新您虛擬化伺服器上。

6.  接下來，建立新的虛擬機器，若要安裝到 Linux OS。
    在 [動作] 窗格中，按一下**的新** > **虛擬機器**即可啟動精靈。
    提供您的 VM，例如"Pre-templatized Linux"的易記名稱，然後按**下一步**。

7.  在精靈的第二個頁面上，選取**第 2 代**以確保使用 UEFI 式韌體的設定檔佈建 VM。

8.  完成 [根據您的喜好設定] 精靈的其餘部分。
    此 VM，請勿使用差異磁碟受防護的 VM 範本磁碟無法使用差異磁碟。
    最後，連接您稍早下載至虛擬 DVD 光碟機此 vm，您可以安裝作業系統的 ISO 映像。

9.  在 [HYPER-V 管理員] 中，選取您新建立的 VM，然後按一下**連接...** 附加至 VM 虛擬主控台的 [動作] 窗格中。
    在出現的視窗，按一下**啟動**開啟虛擬機器。

10. 繼續執行您所選的 Linux 散發套件的安裝程序。
    雖然每個 Linux 散發套件會使用不同的安裝程式精靈，就必須符合下列需求的 Vm 即將成為 Linux 受防護的 VM 範本磁碟：

    - 必須使用 GUID 分割表格 (GPT) 的配置分割磁碟
    - 必須使用 dm crypt 加密根磁碟分割。 複雜密碼應設為**複雜密碼**（全部小寫）。 此複雜密碼將會隨機決定，而且資料分割重新加密時佈建受防護的 VM。
    - 開機磁碟分割必須使用**ext2**檔案系統

11. 當完全啟動您的 Linux 作業系統，而且您已登入時，建議您安裝 linux 虛擬核心和相關聯的 HYPER-V integration services 封裝。
    此外，您要安裝 SSH 伺服器或其他遠端管理工具來存取後已防護的 VM。

    在 Ubuntu 上，執行下列命令以安裝這些元件：

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    在 RHEL，請改為執行下列命令：

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    與在 SLES，執行下列命令：

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. 視需要設定您的 Linux 作業系統。
    任何軟體安裝，您將加入，使用者帳戶和您所做的全系統組態變更會套用至所有未來的 Vm，建立從這個範本磁碟。
    您應該避免將任何機密資料或不必要的封裝儲存至磁碟。

13. 如果您打算使用 System Center Virtual Machine Manager 來部署您的 Vm，安裝 VMM 客體代理程式，以便讓 VMM 在 VM 佈建期間是專為您的作業系統。
    特製化可讓每個 VM 設定安全地使用不同的使用者和 SSH 金鑰、 網路功能的組態，以及自訂的設定步驟。
    了解如何[取得並安裝 VMM 客體代理程式](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent)VMM 文件中。

14. 下一步 [Microsoft Linux 軟體存放庫加入您的套件管理員](../../administration/linux-package-repository-for-microsoft-software.md)。

15. 使用您的套件管理員，則安裝 lsvmtools 套件包含 Linux 受防護 VM 的開機載入器填充碼，提供元件，以及磁碟準備工具。

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. 當您完成時自訂 Linux OS，您的系統上找出 lsvmprep 安裝程式並執行它。
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. 關閉您的 VM。

16. 如果您安裝了 VM （包括 HYPER-V 與 Windows 10 Fall Creators Update 所建立的自動檢查點） 的任何檢查點，請務必刪除，然後再繼續。
    檢查點會建立差異磁碟 (.avhdx) 不支援的範本磁碟精靈。
    
    若要刪除檢查點，請開啟**HYPER-V 管理員**，選取您的 VM，以滑鼠右鍵按一下最上層的檢查點，在檢查點 窗格中，然後按**刪除檢查點樹狀子目錄**。

    ![刪除您的範本 VM 在 HYPER-V manager 中的所有檢查點](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>保護的範本磁碟

您在上一節中備妥 VM 幾乎就能做為 Linux 受防護的 VM 範本磁碟。
最後一個步驟是執行透過此 「 範本磁碟精靈 」，並將雜湊和數位簽章的根和開機磁碟分割的目前狀態的磁碟。
受防護的 VM 佈建以確保未經授權的變更已對範本建立和部署之間的兩個資料分割時，會驗證雜湊和數位簽章。

### <a name="obtain-a-certificate-to-sign-the-disk"></a>取得憑證來簽署磁碟

若要數位簽署磁碟度量，您必須取得電腦上的憑證，您將在其中執行範本磁碟精靈。
憑證必須符合下列需求：

憑證屬性 | 必要的值
---------------------|---------------
金鑰演算法 | RSA
最小金鑰大小 | 2048 位元
簽章演算法 | SHA256 （建議選項）
金鑰使用方法 | 數位簽章

當他們建立其虛擬機器防護資料檔案，並在已授權他們信任的磁碟時，此憑證的詳細資訊將會顯示給租用戶。
因此，務必從相互信任您和您的租用戶之憑證授權單位取得此憑證。
在您所在的主機服務提供者和租用戶的企業案例，您可以考慮發出此憑證，從您的企業憑證授權單位。
謹慎保護此憑證，因為擁有此憑證的任何人都可以建立新的範本磁碟，受信任的真確的磁碟相同。

在測試實驗室環境中，您可以使用下列 PowerShell 命令來建立自我簽署的憑證：

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>處理序範本磁碟精靈 的 cmdlet 與磁碟

將您的範本磁碟和憑證複製到執行 Windows Server 1709 版的電腦，然後執行下列命令來起始簽署程序。
您提供給 VHDX`-Path`參數將會覆寫與更新後的範本磁碟，因此請務必執行命令之前製作的複本。

> [!IMPORTANT]
> 提供在 Windows Server 2016 或 Windows 10 上的遠端伺服器管理工具無法用來準備 Linux 受防護 VM 的範本磁碟。
> 只用[保護 TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet 可在 Windows Server，版本 1709年或提供於 Windows Server 2019 的遠端伺服器管理工具來準備 Linux 受防護的 VM 範本磁碟。

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

現在已備妥要佈建 Linux 受防護的 Vm 可用的範本磁碟。
如果您使用 System Center Virtual Machine Manager 來部署 VM，您現在可以將 VHDX 複製到 VMM 程式庫。

您也可以從 VHDX 擷取磁碟區簽章目錄。
這個檔案用來簽署憑證、 磁碟名稱和版本資訊提供 VM 擁有者想要使用您的範本。
他們必須將此檔案匯入授權您擁有的簽章憑證，建立此範本作者防護資料檔案精靈 和未來的範本磁碟它們。

若要擷取的磁碟區簽章目錄，請在 PowerShell 中執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
