---
title: 部署 Nano Server
description: 說明建立及部署自訂映像、套件、驅動程式、網域、角色及功能
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8953627b7240bdb6bd0ea76b09c5af8317bde186
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830759"
---
# <a name="deploy-nano-server"></a>部署 Nano Server

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 版本 1709 開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

本主題涵蓋部署 Nano Server 映像所需的資訊，相較於＜Nano Server 快速入門＞主題中的簡單範例，這些映像更能夠依照您的需求加以自訂。 您可以找到下列相關資訊：使用您需要的所有功能來建立自訂 Nano Server 映像、從 VHD 或 WIM 安裝 Nano Server 映像、編輯檔案、使用網域、透過幾個方法來處理套件，以及使用伺服器角色。

## <a name="nano-server-image-builder"></a>Nano Server Image Builder

Nano Server Image Builder 工具可協助您在圖形化介面的輔助下，建立自訂 Nano Server 映像和可開機的 USB 媒體。 根據您提供的輸入，它會產生可重複使用的 PowerShell 指令碼，讓您輕鬆地自動並一致安裝執行 Windows Server 2016 Datacenter 或 Standard Edition 的 Nano Server。

從[下載中心](https://www.microsoft.com/en-us/download/details.aspx?id=54065)取得工具。 

此工具也需要 [Windows 評定及部署套件 (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit)。


Nano Server Image Builder 會建立 VHD、VHDX 或 ISO 格式的自訂 Nano Server 映像，以及可開機的 USB 媒體，來部署 Nano Server 或偵測伺服器的硬體設定。 它也可以執行下列作業：

- 接受授權條款 
- 建立 VHD、VHDX 或 ISO 格式
- 新增伺服器角色
- 新增裝置驅動程式
- 設定機器名稱、系統管理員密碼、記錄檔路徑和時區
- 使用現有的 Active Directory 帳戶或收集到的網域加入 Blob 來加入網域
- 啟用 WinRM 以在本機子網路外部進行通訊
- 啟用虛擬區域網路識別碼並設定靜態 IP 位址
- 快速插入新的服務套件
- 新增要在處理 unattend.xml 之後執行的 setupcomplete.cmd 或其他客戶指令碼
- 啟用緊急管理服務 (EMS) 以進行序列埠主控台存取
- 啟用開發服務，以啟用已用測試簽署的驅動程式和未簽署的應用程式、PowerShell 預設殼層
- 啟用透過序列、USB、TCP/IP 或 IEEE 1394 通訊協定進行偵錯
- 使用 WinPE 建立 USB 媒體，這將會分割伺服器，並安裝 Nano 映像
- 使用 WinPE 建立 USB 媒體，這將會偵測您現有的 Nano Server 硬體設定，並報告記錄檔中和螢幕上的所有詳細資料。 這包括網路介面卡、MAC 位址和韌體類型 (BIOS 或 UEFI)。 此偵測程序也會列出沒有 Server Core 驅動程式套件隨附驅動程式之系統和裝置上的所有磁碟區清單。

如果您不熟悉上述任何作業，請檢閱本主題的其餘部分及其他 Nano Server 主題，以準備好提供工具所需的資訊。

## <a name="BKMK_CreateImage"></a>建立自訂 Nano Server 映像  
在 Windows Server 2016 中，Nano Server 會透過實體媒體散發，您將在媒體中找到 **NanoServer** 資料夾，其中包含 .wim 映像及名為 **Packages** 的子資料夾。 這是您用來將伺服器角色和功能新增至 VHD 映像，並接著開機到此映像的套件檔案。  
  
您也可以尋找並安裝這些套件使用 PackageManagement (OneGet) PowerShell 模組的 NanoServerPackage 提供者。 請參閱本主題的＜線上安裝角色和功能＞一節。  
  
下表顯示此版 Nano Server 中可用的角色和功能，以及將安裝其套件的 Windows PowerShell 選項。 有些套件可透過自己的 Windows PowerShell 參數 (例如 -Compute) 直接安裝；其他套件則藉由將套件名稱傳遞至 -Package 參數進行安裝 (可在逗號分隔清單中結合)。 您可以使用 Get-NanoServerPackage Cmdlet 動態地列出可用的套件。  
  
|角色或功能|選項|
|-------------------|----------|
|Hyper-V 角色 (包括 NetQoS)|-Compute|
|容錯移轉叢集與其他元件，於此表後詳述|-Clustering|
|各種網路介面卡和存放控制器的基本驅動程式。 這是 Windows Server 2016 之 Server Core 安裝隨附的同一組驅動程式。|-OEMDrivers|
|檔案伺服器角色和其他存放元件，於此表後詳述|-Storage|
|Windows Defender，包括預設簽章檔案|-Defender|
|反轉轉寄站以與應用程式相容，例如 Ruby、Node.js 等通用應用程式架構。|現在預設會包含|
|DNS 伺服器角色|-Package Microsoft-NanoServer-DNS-Package|
|PowerShell 預期狀態設定 (DSC)|-Package Microsoft-NanoServer-DSC-Package<br />**注意：** 如需完整詳細資訊，請參閱[在 Nano Server 上使用 DSC](https://msdn.microsoft.com/powershell/dsc/nanoDsc)。|
|Internet Information Server (IIS)|-Package Microsoft-NanoServer-IIS-Package<br />**注意：** 請參閱[Nano Server 上的 IIS](IIS-on-Nano-Server.md)如需使用 IIS 的詳細資訊。|
|Windows 容器的主機支援|-Containers|
|System Center Virtual Machine Manager 代理程式|-Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**注意：** 只有當您要監視 HYPER-V，請使用 SCVMM 計算封裝。 對於 VMM 中的超融合部署，您應該也指定 -Storage 參數。 如需詳細資料，請參閱 [VMM 文件](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v)。| 
|System Center Operations Manager 代理程式| 個別安裝。 請參閱 System Center Operations Manager 文件，如需詳細資訊，在 https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server。|
|資料中心橋接 (包括 DCBQoS)|-Package Microsoft-NanoServer-DCB-Package|
|在虛擬機器上部署|-Package Microsoft-NanoServer-Guest-Package|
|在實體機器上部署|- Package Microsoft-NanoServer-Host-Package|
|BitLocker、信賴平台模組 (TPM)、磁碟區加密、平台識別、加密提供者，以及其他與安全啟動相關的功能|-Package Microsoft-NanoServer-SecureStartup-Package|
|Hyper-V 支援受防護的 VM|-Package Microsoft-NanoServer-ShieldedVM-Package<br />**注意：** 這個套件僅適用於 Nano Server Datacenter edition。|
|簡易網路管理通訊協定 (SNMP) 代理程式|-Package Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**注意：** 不包含在 Windows Server 2016 安裝媒體。 僅限線上存取。 如需詳細資料，請參閱[線上安裝角色和功能](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online)。|
|IPHelper 服務，使用 IPv6 轉換技術（6to4、ISATAP、Proxy 連接埠和 Teredo）和 IP-HTTPS 提供通道連接|-Package Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**注意：** 不包含在 Windows Server 2016 安裝媒體。 僅限線上存取。 如需詳細資料，請參閱[線上安裝角色和功能](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online)。|

> [!NOTE]  
> 當您使用這些選項安裝套件時，也會根據選取的伺服器媒體地區設定一併安裝對應的語言套件。 您可以在針對映像地區設定命名之子資料夾中的安裝媒體上，找到可用的語言套件及其地區設定縮寫。  
  
> [!NOTE]  
> 當您使用 -Storage 參數安裝檔案服務時，實際上不會啟用檔案服務。 請使用伺服器管理員，從遠端電腦啟用此功能。 

### <a name="failover-clustering-items-installed-by-the--clustering-parameter"></a>-Clustering 參數所安裝的容錯移轉叢集項目

- 容錯移轉叢集角色
- VM 容錯移轉叢集
- 儲存空間直接存取 (S2D)
- 存放裝置服務品質
- 磁碟區複寫叢集
- SMB 見證服務


### <a name="file-and-storage-items-installed-by-the--storage-parameter"></a>-Storage 參數所安裝的檔案與儲存項目

- 檔案伺服器角色
- 重複資料刪除
- 多重路徑 I/O，包括 Microsoft 裝置特定模組 (MSDSM) 的驅動程式
- ReFS (v1 與 v2)
- iSCSI 啟動器 (但非 iSCSI 目標)
- 儲存體複本
- 存放裝置管理服務與 SMI-S 支援
- SMB 見證服務
- 動態磁碟區
- 基本 Windows 儲存體提供者 (適用於 Windows 存放管理)
 
  
  
  
### <a name="installing-a-nano-server-vhd"></a>安裝 Nano Server VHD  
此範例會建立 GPT 型 VHDX 映像，該映像具有指定的電腦名稱，並包含透過網路共用上的 Nano Server 安裝媒體啟動的 Hyper-V 客體驅動程式。 在提升權限的 Windows PowerShell 命令提示字元中，從下列 Cmdlet 開始：  
  
`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  
  
此 Cmdlet 將會完成下列所有工作：  
  
1.  選取 Standard 作為基底版本  
  
2.  提示您輸入系統管理員密碼  
  
3.  將 \\\Path\To\Media\server_en-us 中的安裝媒體複製到 .\Base  
  
4.  將 WIM 映像轉換成 VHD (目標路徑引數的副檔名決定要建立第 1 代虛擬機器的 MBR 型 VHD，還是第 2 代虛擬機器的 GPT 型 VHDX)。  
  
5.  將產生的 VHD 複製到 .\FirstStepsNano.vhdx  
  
6.  依照指定為映像設定系統管理員密碼  
  
7.  將映像的電腦名稱設定為 FirstStepsNano  
  
8.  安裝 Hyper-V 客體驅動程式  
  
最後會產生 .\FirstStepsNano.vhdx 的映像。  
  
此 Cmdlet 會在執行時產生記錄檔，並在完成時告訴您此記錄檔的所在位置。 隨附的指令碼會完成 WIM 與 VHD 的轉換，並在 %TEMP%\Convert-WindowsImage\\\<GUID&gt; 中產生自己的記錄檔 (其中 \<GUID&gt; 是每個轉換工作階段的唯一識別碼)。  
  
只要您使用相同的基底路徑，就可以在每次執行此 Cmdlet 時省略媒體路徑參數，因為它會使用從基底路徑快取的檔案。 如果您未指定基底路徑，此 Cmdlet 會在 TEMP 資料夾中產生預設路徑。 不過，如果您想要使用不同的來源媒體但相同的基底路徑，則應該指定媒體路徑參數。  
  
>[!NOTE]  
>您現在可以選擇指定 Nano Server 版本，建置 Standard 或 Datacenter Edition。 請使用 -Edition 參數來指定 *Standard* 或 *Datacenter* Edition。  
  
一旦您擁有現成的映像，就可以視需要使用 *Edit-NanoServerImage* Cmdlet 加以修改。  
  
如果未指定電腦名稱，就會隨機產生名稱。  
  
### <a name="installing-a-nano-server-wim"></a>安裝 Nano Server WIM  
  
1.  將 *NanoServerImageGenerator* 資料夾從 Windows Server 2016 ISO 中的 \NanoServer 資料夾複製到您電腦上的本機資料夾。  
2. 以系統管理員身分啟動 Windows PowerShell，將目錄變更為您放置 NanoServerImageGenerator 資料夾的資料夾，然後使用 `Import-Module .\NanoServerImageGenerator -Verbose` 匯入模組。  
  
 >[!NOTE]  
>您可能需要調整 Windows PowerShell 執行原則。 `Set-ExecutionPolicy RemoteSigned` 應該正常運作。  
  
若要建立 Nano Server 映像作為 Hyper-V 主機，請執行下列命令：  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  
  
其中  
-   -MediaPath 是包含 Windows Server 2016 之 DVD 媒體或 ISO 映像的根目錄。  
-   -BasePath 會包含 Nano Server 二進位檔複本，讓您在未來執行時可以使用 New-NanoServerImage -BasePath，而不需要指定 -MediaPath。  
-   -TargetPath 會包含產生的 .wim 檔案，其中包含您選取的角色和功能。 請務必指定 .wim 副檔名。  
-   -Compute 會新增 Hyper-V 角色。  
-   -OemDrivers 會新增一些常見的驅動程式。  
  
系統將提示您輸入系統管理員密碼。  
  
如需詳細資訊，請執行 `Get-Help New-NanoServerImage -Full`。  
   
開機到 WinPE，並確保剛才建立的 .wim 檔案可從 WinPE 存取 (例如，您可以將 .wim 檔案複製到 USB 快閃磁碟機上可開機的 WinPE 映像)。  
  
啟動 WinPE 之後，使用 Diskpart.exe 來準備目標電腦的硬碟。 執行下列 Diskpart 命令 (如果未使用 UEFI 和 GPT，請相應地修改)：  
   
> [!WARNING]  
> 這些命令將會刪除硬碟上的所有資料。  
  
**Diskpart.exe 選取磁碟 0 全新轉換 GPT 建立分割區 efi 大小 = 100 格式快速 FS = FAT32 標籤 ="System"指派代號 ="s"Create partition msr size = 128 建立資料分割的主要格式快速 FS = NTFS 標籤 ="NanoServer"指派代號 ="n"List volume結束**  
   
套用 Nano Server 映像 (調整 .wim 檔案的路徑)：  
  
**Dism.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n: \Bcdboot.exe n:\Windows /s s:**  
   
取出 DVD 媒體或 USB 磁碟機，然後使用 **Wpeutil.exe Reboot** 重新啟動您的系統  
  
  
### <a name="editing-files-on-nano-server-locally-and-remotely"></a>編輯本機和遠端 Nano Server 上的檔案  
 在任一情況下，連線到 Nano Server，例如透過 Windows PowerShell 遠端執行功能。  
   
 連線到 Nano Server 之後，您可以藉由將檔案的相對或絕對路徑傳遞至 psEdit 命令，來編輯位於本機電腦上的檔案，例如：   
`psEdit C:\Windows\Logs\DISM\dism.log` 或 `psEdit .\myScript.ps1`  
  
藉由使用 `Enter-PSSession -ComputerName "192.168.0.100" -Credential ~\Administrator` 啟動遠端工作階段，再將檔案的相對或絕對路徑傳遞至 psEdit 命令，來編輯位於遠端 Nano Server 上的檔案，如下所示：   
`psEdit C:\Windows\Logs\DISM\dism.log`  
  
## <a name="BKMK_online"></a>線上安裝角色和功能  
> [!NOTE]
> 如果您是從媒體或線上存放庫安裝選用的 Nano Server 套件，它並不會包含最新的安全性問題修正。 若要避免選用套件和基本作業系統之間的版本不符，您應該在安裝選用套件之後和重新啟動伺服器**之前**，安裝[最新的累計更新](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server)。

### <a name="installing-roles-and-features-from-a-package-repository"></a>從套件存放庫安裝角色和功能  
您可以使用 PackageManagement PowerShell 模組的 NanoServerPackage 提供者，從線上套件存放庫尋找及安裝 Nano Server 套件。 若要安裝此提供者，請使用下列 Cmdlet：

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>如果您在執行 Install-PackageProvider 期間發生錯誤，請檢查您已安裝[最新的累積更新](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) ([KB3206632](https://support.microsoft.com/kb/3206632) 或更新版本)，或使用 Save-Module，如下所示： 

```powershell
Save-Module -Path "$Env:ProgramFiles\WindowsPowerShell\Modules\" -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

安裝且匯入此提供者後，您便可使用特地設計用來與 Nano Server 套件搭配使用的 Cmdlet 搜尋、下載並安裝 Nano Server 套件。

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  
  
您也可使用泛型 PackageManagement Cmdlet 並指定 NanoServerPackage 提供者：

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```
  
若要在 Nano Server 上搭配 Nano Server 套件使用上述任何 Cmdlet，請新增 `-ProviderName NanoServerPackage`。 如果您未新增 -ProviderName 參數，PackageManagement 將會逐一查看所有提供者。 如需這些 Cmdlet 的詳細資料，請執行 `Get-Help <cmdlet>`。 以下為部分常見的使用範例：
    
### <a name="searching-for-nano-server-packages"></a>搜尋 Nano Server 套件  
您可以使用 `Find-NanoServerPackage` 或 `Find-Package -ProviderName NanoServerPackage` 搜尋並傳回線上存放庫中可用的 Nano Server 套件清單。 例如，您可以取得所有最新套件的清單：

```powershell
Find-NanoServerPackage
```
  
執行 `Find-Package -ProviderName NanoServerPackage -DisplayCulture` 會顯示所有可用的文化特性。

如果您需要特定地區設定版本 (例如美國英文)，您可以使用 `Find-NanoServerPackage -Culture en-us`、  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` 或 `Find-Package -Culture en-us -DisplayCulture`。

若要依套件名稱尋找特定套件，請使用 -Name 參數。 此參數也接受萬用字元。 例如，若要尋找名稱中有 VMM 的所有套件，請使用 `Find-NanoServerPackage -Name *VMM*` 或 `Find-Package -ProviderName NanoServerPackage -Name *VMM*`。

您可以使用 -RequiredVersion、-MinimumVersion 或 -MaximumVersion 參數來尋找特定版本。 若要尋找所有可用的版本，請使用 -AllVersions。 否則，只會傳回最新版本。 例如：`Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`。 或者，若為所有版本： `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### <a name="installing-nano-server-packages"></a>安裝 Nano Server 套件  
您可以使用 `Install-NanoServerPackage` 或 `Install-Package -ProviderName NanoServerPackage`，將 Nano Server 套件 (包括其任何相依性套件) 安裝到本機 Nano Server 或離線映像。 兩者皆接受來自管道的輸入。

若要將最新版 Nano Server 套件安裝到線上 Nano Server，請使用 `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` 或 `Install-Package -Name Microsoft-NanoServer-Containers-Package`。 PackageManagement 將會使用 Nano Server 的文化特性。

您可以將 Nano Server 套件安裝到離線映像，同時指定特定版本和文化特性，如下所示：

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

或者：

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

以下是以管道方式將套件搜尋結果傳送至安裝 Cmdlet 的一些範例：  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` 會尋找名稱中的任何有"dcb"的套件，並安裝它們。

`Find-Package *nanoserver-compute-* | Install-Package` 會尋找名稱中有"nanoserver-compute-"的套件，並進行安裝。
  
`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` 會尋找名稱中有"compute"的套件，然後安裝到離線映像。

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` 執行相同動作的所有名稱中有"nanoserver-compute-"的套件。

### <a name="downloading-nano-server-packages"></a>下載 Nano Server 套件  

`Save-NanoServerPackage` 或`Save-Package`可讓您下載套件，並將其儲存而不加以安裝。 兩個 Cmdlet 皆接受來自管道的輸入。

例如，若要下載 Nano Server 套件並將其儲存至與萬用字元路徑相符的目錄，請使用 `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\`。在此範例中未指定 -Culture，因此會使用本機電腦的文化特性。 而且未指定任何版本，因此會儲存最新版本。

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` 儲存特定的版本和義大利文語言和地區設定。

您可以透過管道傳送搜尋結果，如下列範例所示：

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

或

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### <a name="inventory-installed-packages"></a>清查所安裝的套件
您可以使用 `Get-Package` 探索安裝了哪些 Nano Server 套件。 例如，使用 `Get-Package -ProviderName NanoserverPackage` 查看 Nano Server 上有哪些套件。

若要檢查在離線映像中所安裝的 Nano Server 套件，請執行 `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`。


### <a name="installing-roles-and-features-from-local-source"></a>從本機來源安裝角色和功能  
雖然建議離線安裝伺服器角色和其他套件，但在容器案例中，您可能需要在線上 (Nano Server 正在執行時) 安裝這些項目。 若要這樣做，請執行下列步驟：  
  
1.  將 Packages 資料夾從安裝媒體複製到執行中的本機 Nano Server (例如複製到 C:\packages)。  
  
2.  在另一部電腦上建立新的 Unattend.xml 檔案，然後將它複製到 Nano Server。 您可以將此 XML 內容複製並貼到您建立的 XML 檔案 (此範例示範如何安裝 IIS 套件)：  
  
   
     
```  
<?xml version="1.0" encoding="utf-8"?>
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  
  
  
  
  
3.  在您建立 (或複製) 的新 XML 檔案中，將 C:\packages 修改為您複製套件內容的目的地目錄。  
  
4.  切換至具有新建立之 XML 檔案的目錄，然後執行  
  
    **dism /online /apply-unattend:.\unattend.xml**  
  
     
  
5.  執行下列命令，確認已正確安裝該套件及其關聯的語言套件：  
  
    **dism /online /get-packages**  
  
    您應該會看到"Package Identity:Microsoft NanoServer-IIS 套件 ~ 31bf3856ad364e35 ~ amd64 ~ EN-US ~ 10.0.10586.0"列出兩次，一次針對 Release Type:語言套件，一次針對 Release Type:功能套件。  
  
## <a name="customizing-an-existing-nano-server-vhd"></a>自訂現有的 Nano Server VHD  
您可以使用 Edit-NanoServerImage Cmdlet 來變更現有 VHD 的詳細資料，如下列範例所示：  
  
`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  
  
此 Cmdlet 與 New-NanoServerImage 的功能相同，但會變更現有的映像，而不是將 WIM 轉換成 VHD。 它與 New-NanoServerImage 支援相同的參數，但 -MediaPath 和 -MaxSize 則為例外，因此必須使用這兩個參數建立初始 VHD，才能使用 Edit-NanoServerImage 進行變更。  

## <a name="additional-tasks-you-can-accomplish-with-new-nanoserverimage-and-edit-nanoserverimage"></a>您可以使用 New-NanoServerImage 和 Edit-NanoServerImage 完成其他工作  
  
### <a name="joining-domains"></a>加入網域  
New-NanoServerImage 提供兩種加入網域的方法，這兩個方法都需要離線網域佈建，但其中一個方法會收集 Blob 來完成加入。 在此範例中，此 Cmdlet 會從本機電腦為 Contoso 網域收集網域 Blob (這當然必須是 Contoso 網域的一部分)，然後使用此 Blob 執行映像的離線佈建：  
  
`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  
  
當此 Cmdlet 完成時，您應該會在 Active Directory 電腦清單中找到名為 "JoinDomHarvest" 的電腦。  
  
您也可以在未加入網域的電腦上使用此 Cmdlet。 若要這樣做，請從任何加入網域的電腦收集 Blob，然後將 Blob 自行提供給此 Cmdlet。 請注意，當您從其他電腦收集這類 Blob 時，Blob 已包含該電腦的名稱；因此，如果您嘗試新增 *-ComputerName* 參數，則會產生錯誤。  
  
您可以使用下列命令收集 Blob：  
  
**djoin**  
 **/ 佈建**  
 **/ 網域 Contoso**  
 **/Machine JoiningDomainsNoHarvest**  
 **/ SaveFile JoiningDomainsNoHarvest.djoin**  
  
使用收集到的 Blob執行 New-NanoServerImage：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  
  
如果您在網域中已有一個節點，其電腦名稱與未來的 Nano Server 同名，您可以新增 `-ReuseDomainNode` 參數來重複使用此電腦名稱。  

### <a name="adding-additional-drivers"></a>新增其他驅動程式
Nano Server 提供一個套件，其中包括適用於各種網路介面卡和存放控制器的一組基本驅動程式；您的網路介面卡適用的驅動程式可能未包含在內。 您可以使用下列步驟來尋找運作中系統的驅動程式、將驅動程式解壓縮，再新增至 Nano Server 映像。

1.  在您將執行 Nano Server 的實體電腦上安裝 Windows Server 2016。
2.  開啟裝置管理員，並找出下列類別的裝置：
* 網路介面卡
* 存放控制器
* 磁碟機
3.  針對這些類別的每個裝置，以滑鼠右鍵按一下裝置名稱，然後按一下 **/\[內容/\]**。 在開啟的對話方塊中，按一下 **/\[驅動程式/\]** 索引標籤，然後按一下 **/\[驅動程式詳細資料/\]**。
4.  記下出現之驅動程式檔案的檔名和路徑。 例如，假設驅動程式檔案為 e1i63x64.sys，且位於 C:\Windows\System32\Drivers 中。
5.  在命令提示字元中，搜尋驅動程式檔案並搜尋具有 dir e1i*.sys /s /b 的所有執行個體。 在此範例中，驅動程式檔案也會位於路徑 C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys 中。
6.  在提升權限的命令提示字元中，瀏覽至 Nano Server VHD 所在的目錄，然後執行下列命令︰**md mountdir**
     
     **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**
     
     **dism\dism /Add-Driver /image:.\mountdir /driver:C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**
     
     **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
7.  針對您需要的每個驅動程式檔案重複這些步驟。

> [!NOTE]  
> 在您保留驅動程式的資料夾中，必須有 SYS 檔案和對應的 INF 檔案。 此外，Nano Server 只支援已簽署的 64\- 位元驅動程式。 
  
### <a name="injecting-drivers"></a>插入驅動程式  
Nano Server 提供一個套件，其中包括適用於各種網路介面卡和存放控制器的一組基本驅動程式；您的網路介面卡適用的驅動程式可能未包含在內。 您可以使用此語法，讓 New-NanoServerImage 搜尋目錄中是否有可用的驅動程式，並將其插入 Nano Server 映像：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  
  
> [!NOTE]
> 在您保留驅動程式的資料夾中，必須有 SYS 檔案和對應的 INF 檔案。 此外，Nano Server 只支援已簽署的 64 位元驅動程式。

使用 -DriverPath 參數，您也可以將路徑陣列傳遞至驅動程式 .inf 檔案：

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### <a name="connecting-with-winrm"></a>使用 WinRM 進行連線  
若要使用 Windows 遠端管理 (WinRM) 從位於不同子網路的其他電腦連線到 Nano Server 電腦，請針對 Nano Server 映像上的連入 TCP 流量開啟連接埠 5985。 使用下列 Cmdlet：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  
  
### <a name="setting-static-ip-addresses"></a>設定靜態 IP 位址  
若要設定 Nano Server 映像使用靜態 IP 位址，請先使用 Get-NetAdapter、netsh 或 Nano Server 修復主控台，尋找您要修改之介面的名稱或索引。 請使用 -Ipv6Address、-Ipv6Dns、-Ipv4Address、-Ipv4SubnetMask、-Ipv4Gateway 和 -Ipv4Dns 參數來指定此設定，如下列範例所示：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  
  
### <a name="custom-image-size"></a>自訂映像大小  
您可以設定 Nano Server 映像，使用 -MaxSize 參數來動態擴充 VHD 或 VHDX，如下列範例所示：  
  
`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  
  
### <a name="embedding-custom-data"></a>嵌入自訂資料  
若要將您自己的指令碼或二進位檔嵌入 Nano Server 映像，請使用 -CopyPath 參數傳遞要複製的檔案和目錄陣列。 -CopyPath 參數也可接受雜湊表來指定檔案及路徑的目的地路徑。

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### <a name="running-custom-commands-after-the-first-boot"></a>在初次開機之後執行自訂命令
若要將自訂命令當做 setupcomplete.cmd 的一部分來執行，請使用 -SetupCompleteCommand 參數傳遞命令陣列：

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @("echo foo", "echo bar")`


### <a name="running-custom-powershell-scripts-as-part-of-image-creation"></a>將執行自訂 PowerShell 指令碼作為建立映像的一部份
若要將執行自訂 PowerShell 指令碼作為映像建立程序的一部份，請使用 -OfflineScriptPath 參數將路徑陣列傳遞至 .ps1 指令碼。 若這些指令碼接受引數，則使用 -OfflineScriptArgument 將其他引數的雜湊表傳遞至指令碼。

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1="Value1"; Param2="Value2"}`


### <a name="support-for-development-scenarios"></a>開發案例的支援
如果您想要在 Nano Server 上進行開發和測試，您可以使用 -Development 參數。 這會將 PowerShell 作為預設本機殼層啟用、啟用未簽署驅動程式的安裝、複製偵錯工具二進位檔、開啟偵錯用的連接埠、啟用測試簽署，以及啟用 AppX 套件的安裝，而不需要開發人員授權：

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### <a name="custom-unattend-file"></a>自訂自動安裝檔案  
如果您想要使用自己的自動安裝檔案，請使用 -UnattendPath 參數：  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  
  
在此自動安裝檔案中指定系統管理員密碼或電腦名稱，將會覆寫由 AdministratorPassword 和 -ComputerName 設定的值。 

> [!NOTE]
> Nano Server 不支援透過自動安裝檔案設定 TCP/IP 設定。 您可以使用 Setupcomplete.cmd 設定 TCP/IP 設定。

### <a name="collecting-log-files"></a>收集記錄檔
若您要在建立映像期間收集記錄檔，請使用 -LogPath 參數指定所有記錄檔的複製目標目錄。

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> New-NanoServerImage 及 Edit-NanoServerImage 上的部分參數僅供內部使用，且可放心忽略。 這包括 -SetupUI 及 -Internal 參數。


## <a name="installing-apps-and-drivers"></a>安裝應用程式和驅動程式
[comment]: # (從 Xumin 太陽 bug #68620。)  

### <a name="windows-server-app-installer"></a>Windows Server 應用程式安裝程式
Windows Server 應用程式 (WSA) 安裝程式為 Nano Server 提供可靠的安裝選項。 因為 Nano Server 不支援 Windows Installer (MSI)，所以 WSA 也是適用於非 Microsoft 產品的唯一安裝技術。 WSA 利用 Windows 應用程式封裝技術，其設計目的是為了能夠使用宣告式資訊清單，安全可靠地安裝及服務應用程式。 該技術擴充 Windows 應用程式封裝安裝程式，以便支援 Windows Server 特定的擴充功能，但具有 WSA 不支援安裝驅動程式的限制。

在 Nano Server 上建立及安裝 WSA 套件同時需要套件的發行者和取用者執行步驟。

套件發行者應該執行下列步驟：

1. 安裝[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)，其中包括建立 WSA 套件所需的工具：MakeAppx、 MakeCert、 Pvk2Pfx、 SignTool。
2. 宣告資訊清單：請遵循[WSA 資訊清單延伸結構描述](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx)建立資訊清單檔案 AppxManifest.xml。
3. 使用 **MakeAppx** 工具建立 WSA 套件。
4. 使用 **MakeCert** 和 **Pvk2Pfx** 工具建立憑證，然後使用 **Signtool** 簽署套件。

接下來，套件取用者應該執行下列步驟：

1. 執行 [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) PowerShell Cmdlet，將上述步驟 4 中的發行者憑證匯入 Nano Server，其 certStoreLocation 位於 "Cert:\LocalMachine\TrustedPeople"。 例如：`Import-Certificate -FilePath ".\xyz.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedPeople"`
2. 執行 [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) PowerShell Cmdlet 在 Nano Server 上安裝 WSA 套件，以在 Nano Server 上安裝應用程式。 例如：`Add-AppxPackage wsaSample.appx`

#### <a name="additional-resources-for-creating-apps"></a>用於建立應用程式的其他資源
WSA 是 Windows 應用程式封裝技術的伺服器擴充功能 (但未裝載於 Microsoft Store)。 如果您想要使用 WSA 發行應用程式，這些主題將協助您熟悉應用程式封裝管道：

- [如何建立基本封裝資訊清單](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [App Packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)
- [如何建立應用程式套件簽署憑證](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### <a name="installing-drivers-on-nano-server"></a>在 Nano Server 上安裝驅動程式
您可以使用 INF 驅動程式套件，在 Nano Server 上安裝非 Microsoft 驅動程式。 其中包括隨插即用 (PnP) 驅動程式套件和檔案系統篩選器驅動程式套件。 Nano Server 目前不支援網路篩選器驅動程式。

PnP 和檔案系統篩選器驅動程式套件必須遵循通用驅動程式需求和安裝程序，以及一般驅動程式套件方針 (例如簽署)。 相關說明請參閱下列位置：

- [驅動程式簽署](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing)
- [使用通用 INF 檔案](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file)

#### <a name="installing-driver-packages-offline"></a>離線安裝驅動程式套件

您可以透過 [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) 或 [DISM PowerShell](https://technet.microsoft.com/library/dn376497.aspx) Cmdlet，將支援的驅動程式套件安裝到離線 Nano Server。

#### <a name="installing-driver-packages-online"></a>線上安裝驅動程式套件
您可以使用 [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx)，將 PnP 驅動程式套件安裝到線上 Nano Server。 Nano Server 目前不支援非 PnP 驅動程式套件的線上驅動程式安裝。





 
--------------------------------------------------  
  
  
## <a name="BKMK_JoinDomain"></a>將 Nano Server 加入網域  
  
### <a name="to-add-nano-server-to-a-domain-online"></a>在線上將 Nano Server 加入網域  
  
1.  使用下列命令，從網域中已執行 Windows Threshold Server 的電腦收集資料 Blob：  
  
    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
    這會將資料 Blob 儲存在名為 "odjblob" 的檔案中。  
  
2.  使用下列命令，將 "odjblob" 檔案複製到 Nano Server 電腦：  
  
    **net use z: \\ \\ \<Nano 伺服器的 ip 位址 > \c$**  
  
    > [!NOTE]  
    > 如果 net use 命令失敗，您可能需要調整 Windows 防火牆規則。 若要這樣做，請先開啟提升權限的命令提示字元、啟動 Windows PowerShell，然後使用 Windows PowerShell 遠端執行功能並搭配下列命令連線到 Nano Server 電腦：  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
    >   
    > `$ip = "<ip address of Nano Server>"`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > 出現提示時，提供系統管理員密碼，然後執行下列命令來設定防火牆規則：  
    >   
    > **netsh advfirewall 防火牆設定的規則群組 = [檔案及印表機共用] 的新 enable = yes**  
    >   
    > 使用 `Exit-PSSession` 結束 Windows PowerShell，然後重試 net use 命令。 如果成功，請繼續將 "odjblob" 檔案內容複製到 Nano Server。  
  
    **md z:\Temp**  
  
    **copy odjblob z:\Temp**  
  
3.  檢查您要加入 Nano Server 的網域，並確定已設定 DNS。 此外，請確認網域或網域控制站的名稱解析如預期般運作正常。 若要這樣做，請開啟提升權限的命令提示字元、啟動 Windows PowerShell，然後使用 Windows PowerShell 遠端執行功能並搭配下列命令連線到 Nano Server 電腦：  
  
    `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
    `$ip = "<ip address of Nano Server>"`  
  
    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
  
    出現提示時，提供系統管理員密碼。 Nano Server 上無法使用 nslookup，因此您可以使用 Resolve-DNSName 確認名稱解析。

4. 如果名稱解析成功，則在相同的 Windows PowerShell 工作階段中，執行下列命令以加入網域：  
  
    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  
  
5.  重新啟動 Nano Server 電腦，然後結束 Windows PowerShell 工作階段：  
  
    `shutdown /r /t 5`  
  
    `Exit-PSSession`  
  
6.  將 Nano Server 加入網域之後，請將網域使用者帳戶新增至 Nano Server 上的 Administrators 群組。

7. 為了安全性，請從信任的主機清單中，使用此命令移除 Nano Server: `Set-Item WSMan:\localhost\client\TrustedHosts ""` 
  
**若要加入網域，以在一個步驟中的替代方法**  
  
首先，使用下列命令，從執行 Windows Threshold Server 並已在網域中的另一部電腦收集資料 Blob：  
  
`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  
  
開啟檔案 "odjblob" (或許使用 [記事本])、複製其內容，然後將內容貼到下列 Unattend.xml 檔案的 \<AccountData&gt; 區段中。  
  
將此 Unattend.xml 檔案放到 C:\NanoServer 資料夾中，然後使用下列命令來掛接 VHD 並套用 `offlineServicing` 區段中的設定：  
  
**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  
  
**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  
  
建立 "Panther" 資料夾 (供 Windows 系統用於存放安裝期間的檔案；如需詳細資訊，請參閱 [Windows 7, Windows Server 2008 R2, and Windows Vista setup log file locations](https://support.microsoft.com/en-us/kb/927521) (Windows 7、Windows Server 2008 R2 和 Windows Vista 安裝記錄檔位置))，將 Unattend.xml 檔案複製到其中，然後使用下列命令取消掛接 VHD：  
  
**md .\mountdir\windows\panther**  
  
**copy .\unattend.xml .\mountdir\windows\panther**  
  
**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  
  
第一次從此 VHD 啟動 Nano Server 時，將會套用其他設定。  
  
將 Nano Server 加入網域之後，請將網域使用者帳戶新增至 Nano Server 上的 Administrators 群組。  
 
## <a name="working-with-server-roles-on-nano-server"></a>在 Nano Server 上使用伺服器角色

###<a name="using-hyper-v-on-nano-server"></a>在 Nano Server 上使用 Hyper-V  
Hyper-V 在 Nano Server 上的運作方式與在 Server Core 模式的 Windows Server 上相同，但有兩個例外：  
  
-   您必須從遠端執行所有管理，而且管理電腦必須與 Nano Server 執行相同組建的 Windows Server。 舊版 Hyper-V 管理員或 Hyper-V Windows PowerShell Cmdlet 將無法運作。  
  
-   RemoteFX 不適用。  
  
在此版本中，Hyper-V 的這些功能已確認：  
  
-   啟用 Hyper-V  
  
-   建立第 1 代和第 2 代虛擬機器  
  
-   建立虛擬交換器  
  
-   啟動虛擬機器並執行 Windows 客體作業系統  
- Hyper-V 複本  
  
  
  
如果您想要執行虛擬機器的即時移轉、在 SMB 共用上建立虛擬機器，或將現有 SMB 共用上的資源連線到現有的虛擬機器，請務必正確設定驗證。 您有兩個選項可執行這項作業：  
  
**限制的委派**  
  
限制委派的運作方式與舊版完全相同。 如需詳細資訊，請參閱下列文章：  
  
-   [啟用 HYPER-V 遠端管理-設定的 SMB 和高可用性 SMB 限制的委派](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx)  
  
-   [啟用 HYPER-V 遠端管理-設定限制的委派的非叢集即時移轉](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx)  
  
**CredSSP**  
  
首先，參閱本主題的＜使用 Windows PowerShell 遠端執行功能＞一節來啟用及測試 CredSSP。 然後，您可以在管理電腦上使用 Hyper-V 管理員，然後選取 [連線為其他使用者] 選項。 Hyper-V 管理員將會使用 CredSSP。 即使您使用目前的帳戶，也應該執行這項操作。  
  
Hyper-V 的 Windows PowerShell Cmdlet 可以使用 CimSession 或 Credential 參數，這兩個參數都適用於 CredSSP。  
  
### <a name="BKMK_Failover"></a>使用 Nano Server 上的容錯移轉叢集  
容錯移轉叢集在 Nano Server 上的運作方式與在 Server Core 模式的 Windows Server 上相同，但請注意下列事項：  
  
-   必須使用容錯移轉叢集管理員或 Windows PowerShell 從遠端管理這些叢集。  
  
-   所有 Nano Server 叢集節點都必須都加入相同的網域，類似於 Windows Server 中的叢集節點。  
  
-   此網域帳戶必須具有所有 Nano Server 節點的系統管理員權限，就像是 Windows Server 中的叢集節點一樣。  
  
-   所有命令都必須在提升權限的命令提示字元中執行。  
  
> [!NOTE]  
> 此外，此版本不支援某些功能：  
>   
> -   您無法在本機 Nano Server 上透過 Windows PowerShell 執行容錯移轉叢集 Cmdlet。  
> -   將 Hyper-V 和檔案伺服器以外的角色叢集化。  
  
您會發現適用於管理容錯移轉叢集的下列 Windows PowerShell Cmdlet：  
  
您可以建立新的叢集 `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  
  
建立新的叢集之後，您應該在所有節點上執行 `Set-StorageSetting -NewDiskPolicy OfflineShared`。  
  
將其他節點新增至叢集： `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
從叢集移除節點  `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  
  
建立使用向外延展檔案伺服器 `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`  
  
您可以在 [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx) 中找到用於容錯移轉叢集的其他 Cmdlet。  
  
### <a name="BKMK_DNS"></a>Nano Server 上使用 DNS 伺服器  
若要提供 DNS 伺服器角色給 Nano Server，請將 Microsoft-NanoServer-DNS-Package 新增至映像 (請參閱本主題的＜建立自訂 Nano Server 映像＞一節。 一旦 Nano Server 在執行中，請連線到此伺服器，然後從提升權限的 Windows PowerShell 主控台執行下列命令來啟用功能：  
  
`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  
  
### <a name="BKMK_IIS"></a>Nano Server 上使用 IIS  
如需使用 Internet Information Services (IIS) 角色的步驟，請參閱 [Nano Server 上的 IIS](IIS-on-Nano-Server.md)。 

### <a name="using-mpio-on-nano-server"></a>在 Nano Server上使用多重路徑 I/O (MPIO)
如需使用多重路徑 I/O (MPIO) 的步驟，請參閱 [Nano Server 上的多重路徑 I/O (MPIO)](MPIO-on-Nano-Server.md) 

### <a name="BKMK_SSH"></a>Nano Server 上使用 SSH
如需如何在 Nano Server 上安裝及使用 SSH 的指示，請參閱 [Win32-OpenSSH Wiki](https://github.com/PowerShell/Win32-OpenSSH/wiki)。

## <a name="appendix-sample-unattendxml-file-that-joins-nano-server-to-a-domain"></a>附錄：Nano Server 加入網域的範例 Unattend.xml 檔案  
  
> [!NOTE]  
> 將 "odjblob" 的內容貼到 Unattend 檔案之後，請務必刪除內容中的尾端空格。  
  
```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
  <settings pass="offlineServicing">  
    <component name="Microsoft-Windows-UnattendedJoin" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  
  
  <settings pass="oobeSystem">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  
  
  <settings pass="specialize">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  
  
