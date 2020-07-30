---
title: 軟體清查記錄彙總工具
description: 說明如何安裝和管理軟體清查記錄匯總工具
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 350187c0ad7490a0698e4a3b99ef710b632f6c6c
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408837"
---
# <a name="software-inventory-logging-aggregator"></a>軟體清查記錄彙總工具

> 適用于： Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>什麼是軟體清查記錄匯總工具？

軟體清查記錄彙總工具 (SILA) 可接收、彙總並產生資料中心內的 Windows Server 上已安裝的 Microsoft 企業軟體數目和類型的基本報告。

SILA 是可安裝在 Windows Server 上的軟體，但不包含在 Windows Server 安裝中。 若要安裝此軟體，請先從 Windows 下載中心免費下載： [適用於 Windows Server 的軟體清查記錄彙總工具 1.0](https://www.microsoft.com/download/details.aspx?id=49046)

軟體清查記錄架構是為了降低清查 IT 環境中許多伺服器上部署的 Microsoft 軟體時的作業成本。 這個架構是由 Windows Server 2012 R2 中引進的「軟體清查記錄」（SIL）中的兩個元件（SIL 匯總工具和 Windows Server 功能）所組成。 此軟體清查記錄彙總工具 1.0 安裝在一部伺服器上，並接收任何已設定為透過 SIL 轉送資料的 Windows Server 所送來的清查資料。 此設計可讓資料中心系統管理員在廣泛散佈於環境的主要 Windows Server 映像中啟用 SIL。  此軟體套件是目標點，主要是讓客戶安裝在內部，以輕鬆記錄一段時間的清查資料。 此軟體也可以定期在 Microsoft Excel 中建立基本的清查報告。 軟體清查記錄彙總工具 1.0 報告中包含 Windows Server、System Center 和 SQL Server 的安裝計數。

> [!IMPORTANT]
> 使用此軟體並不會將任何資料傳送給 Microsoft。

### <a name="data-sil-collects-over-time"></a>SIL 收集一段時間的資料

正確部署後，就可在 SIL 彙總工具中檢視下列資料：

-   唯一的 Windows Server 安裝在您的資料中心

-   FQDN

-   識別 GUID

-   實體處理器及核心數目

-   虛擬處理器數目 (如果有 VM)

-   實體處理器的型號和類型

-   實體處理器是否啟用超執行緒

-   底座序號

-   在一段時間內，每個主機上同時執行的 Windows Server VM (如果主機執行 Hypervisor) 的上限標記計數和身分識別

-   一段時間內，同時執行受管理的 System Center 代理程式的高水位標記計數和主機名稱會 \( 顯示每部 \) 主機上的 Windows Server vm

-   Vm 上所安裝的 System Center 代理程式名稱，並以受管理 \- 的上限標準計算

-   一段時間內 SQL Server 安裝的計數和位置 \( （僅限需要授權的 sku 和版本）\)

-   [新增/移除程式] 中安裝的軟體清單

### <a name="who-will-use-sil"></a>誰會使用 SIL？

-   **IT 專業人員或資料中心系統管理員**，尋找以低成本的方法自動收集一段時間內重要的軟體清查資料。

-   **Cio 和財務**機關，他們需要報告其組織 IT 部署中的 Microsoft 企業軟體使用方式。

## <a name="getting-started"></a>開始使用
**先決條件**

軟體清查記錄彙總工具 (SIL 彙總工具) 至少在一部伺服器上用於彙總和報告 (在 VM 中或實體硬體上)：

-   **Windows Server 2012 R2** (Standard 或 Datacenter Edition)

-   **IIS 伺服器角色**以及 .Net Framework 4.5、WCF 服務和 HTTP 啟用，都位於 **[新增角色及功能精靈]** 中相同的樹狀結構中。

-   **SQL Server** 2012 Sp2 Standard Edition 或 SQL Server 2014 Standard Edition

-   **64 位元 Microsoft Excel** 2013 (選擇性安裝，但在建立報告時需要)

-   選擇性： **VMware PowerCLI 5.5.0.5836** (VMware 環境所需)

>[!Note]
>使用 Windows Management Framework 時，只有 SIL 匯總工具上的 WMF 版本5.1 有已知的相容性問題。  在安裝 SIL 匯總工具的伺服器上，不需要超過 WMF 4.0 版。

軟體清查記錄 (SIL) 存在於已安裝下列更新的 Windows Server 版本中：

-   **Windows Server 2016**或更高版本

-   **Windows Server 2012 R2** (Standard 或 Datacenter Edition)

    -   Windows Server 2012 R2 更新 **KB3000850** (2014 年 11 月)

    -   Windows Server 2012 R2 更新 **KB3060681** (2015 年 6 月)(可能在 Windows Update 中顯示為可選用的更新)

### <a name="security-and-account-types"></a>安全性和帳戶類型
**憑證需求**

SIL 和 SIL 彙總工具在已驗證的通訊上依賴 SSL 憑證。 一般的實作是以一個憑證 (伺服器名稱和憑證名稱相符) 安裝 SIL 彙總工具，以裝載接收清查資料的 Web 服務。 然後，要使用 SIL 功能清查的 Windows Server 會使用不同的用戶端憑證，將資料推入 SIL 彙總工具。 PowerShell Cmdlet （Set-silaggregator，以下提供更多詳細資料）必須用來將憑證指紋新增至 SIL 匯總工具的已核准憑證清單中，而匯總工具將會從中接受相關聯的資料。 以憑證驗證每一個裝載的資料之後，SIL 彙總工具會繼續處理並插入至其資料庫。 如何相關運作方式的具體細節，請參閱 **SIL 彙總工具 Cmdlet 詳細資料**一節。

### <a name="polling-account-setup"></a>輪詢帳戶設定
將認證加入至 SIL 彙總工具來啟用輪詢作業時，您應該使用最低權限帳戶方法。 此外，基於安全性最佳作法，您不應該對資料中心或其他 IT 部署中的所有或多部主機使用相同的認證。

在您想要設定由 SIL 彙總工具輪詢的 Windows Server 主機上，以及為了避免使用系統管理員群組中的使用者，請遵循下列步驟，提供剛好足夠的存取權給使用者帳戶：

#### <a name="to-setup-a-polling-account"></a>設定輪詢帳戶

1.  在您想要從 SIL 彙總工具輪詢的 Windows Server Hyper-V 主機上，使用 Windows 中的 [電腦管理]**** 建立本機使用者帳戶 (務必取消核取強制在第一次登入時變更密碼的方塊)。

2.  將此使用者加入至 **Remote Management Users** 群組。

3.  將此使用者加入至 **Hyper-V Administrators** 群組。

4.  使用**WMIMgmt.msc** [**開始** -> **執行**] 開啟 wmimgmt.msc。

5.  按一下 [動作]**** 區段中的 [其他動作]****，選取 [內容]****。

6.  按一下 **[安全性]** 。

7.  在 [命名空間]**** 樹狀檢視中選取 [cimv2]**** 命名空間。

8.  按一下 [安全性]**** (按鈕)。

9. 以 **machinename\group name** 格式新增 **Remote Management Users** 群組

10. 按一下 [確定]。

11. 回到 [**root\cimv2** 的安全性] 視窗中，選取 [Remote Management Users]****。

12. 在底部的 [許可權] 區段中，確定已核取 [**遠端啟用**]。

13. 按一下 [套用]****，然後按一下 [確定]****。

14. 在 [**屬性**] 視窗中按一下 **[確定]** 。

### <a name="installing-sil-aggregator"></a>安裝 SIL 彙總工具
在 Windows Server 上安裝 SIL 彙總工具之前，您需要確定一些事情：

-   **您有一個有效的 SSL 憑證**，您想要用來裝載此軟體的 web 服務。

    -   憑證應該為 **.pfx** 格式

    -   Windows Server 名稱和憑證名稱應該符合。

-   **已安裝 SQL Server Standard Edition**，或已安裝在要搭配此軟體一起使用的遠端伺服器上。

    -   SIL 彙總工具適用於 SQL Server 2012 sp2 和 SQL Server 2014。 在 SQL Server 安裝期間進行選取時，沒有需要特別注意的事項。

    -   用於安裝 SIL 彙總工具的帳戶必須是 SQL 上的 sysadmin 角色，才能在安裝期間建立資料庫。

    -   安裝 SIL 彙總工具之前，應該將用來安裝 SIL 彙總工具的帳戶加入為 SQL Analysis Services 的系統管理員。

    -   安裝之後，應該將 SQL Server 代理程式設定成自動執行。

-   **已新增 IIS 伺服器角色**並選取 .Net Framework 4.5、WCF 服務和 HTTP 啟用，全都都在 [新增角色及功能精靈]**** 中選取的相同樹狀結構中完成。

-   您**使用具有伺服器系統管理許可權的帳戶登入伺服器**。

-   您已**使用 SQL Server 上具有 sysadmin 權限的帳戶登入伺服器** (如果想要使用 Windows 驗證)

    OR

    如果想要使用 SQL 驗證，**您必須知道 SQL 系統管理權限帳戶的密碼**。

#### <a name="to-install-software-inventory-logging-aggregator"></a>安裝軟體清查記錄彙總工具

1.  按兩下 **Setup.exe** 開始安裝。

2.  在歡迎使用視窗上按 [下一步]****。

3.  如果您接受 EULA，請核取接受合約的方塊，然後按 **[下一步]**。

4.  在 [選擇功能]**** 中，選取 [安裝軟體清查記錄彙總工具和報告模組]****，然後 [下一步]****。

    如需有關只安裝報告模組的詳細資訊，請參閱 **SIL 彙總工具 Cmdlet 詳細資料**一節下的 `Publish-SilReport`。

5.  驗證所有必要條件之後，按 [下一步]****。

6.  在 [選擇帳戶類型] **** 中，隨您的喜好選取 [本機使用者] **** 或 [gMSA] ****。

    選擇 [本機使用者] 帳戶選項會以自動產生的強式密碼建立本機使用者。 此帳戶將用於本機伺服器上的所有 SIL 彙總工具服務和工作作業。  如果彙總工具是 Active Directory 網域的一部分 (Windows Server 2012 及更高版本)，建議使用群組受管理的服務帳戶 (gMSA)。 如需 gMSA 的詳細資訊，請參閱：[群組受管理的服務帳戶概觀](https://technet.microsoft.com/library/hh831782.aspx)。

    -   如果您打算在 SIL 彙總工具以外的不同伺服器上執行 SQL Server 資料庫，則必須使用 gMSA 帳戶選項。

    -   將電腦帳戶新增至 Active Directory 中已啟用 gMSA 的安全性群組之後，別忘了重新開機伺服器。

7.  在 [選擇 SQL Server]**** 中，輸入已安裝 SQL 執行個體的 SQL Server 或 **localhost** (如果安裝在本機伺服器上)。

    每個 SQL 執行個體僅支援一個 SIL 彙總工具。

8.  選取驗證類型，然後按 [驗證 SQL]****。

9. 按 [下一步]****，然後在 [網際網路資訊服務伺服器的詳細資料]**** 中，選取連接埠號碼或保留預設值。

10. 瀏覽 **.pfx** 檔案位置，輸入 .pfx 檔案的密碼，然後按 [下一步]****。

11. 最後一個畫面會顯示安裝進度。 成功完成時，按一下 [完成]****。

### <a name="uninstalling-sil-aggregator"></a>解除安裝 SIL 彙總工具

#### <a name="to-uninstall-software-inventory-logging-aggregator"></a>解除安裝軟體清查記錄彙總工具

1.  以系統管理員身分開啟**PowerShell** ，然後輸入 `Stop-SilAggregator` 。 當提示返回時，表示 SIL 彙總工具已停止。

    根據設計，SIL 彙總工具會在 20 分鐘或收到 100 個檔案之後處理檔案。  在大規模環境中，永遠不會發生這種情況，但在小規模環境中，可能仍有某些檔案需要處理，因而無法停止彙總工具。 如果不需要保留這些檔案和資料，請使用 `–Force` 參數。

2.  移至 [控制台]****，依序按一下 [程式和功能]****、[解除安裝程式]****、[軟體清查記錄彙總工具]****，然後按一下 [解除安裝]****。

    軟體清查記錄彙總工具會開啟視窗，提示您選擇刪除資料庫中的所有資料，或將所有資料保留在資料庫中。 預設選項是保留 (如果想要重新安裝，您可以連接至現有的資料庫，回到彙總工具先前離開的地方取回資料)。

3.  選取 [保留]**** 或 [刪除]****，然後按 [下一步]****。

4.  進度列完成後，按一下 [完成]****。

### <a name="start-using-sil-and-the-sil-aggregator"></a>開始使用 SIL 和 SIL 彙總工具

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>SIL 彙總工具 PowerShell Cmdlet 簡介
身為系統管理員，可以從 Windows PowerShell 主控台執行下列命令。

|Windows PowerShell Cmdlet|函式|
|-----------------------------|------------|
|`Start-SilAggregator`|啟動所有軟體清查記錄彙總工具服務和工作。 需要如此，彙總工具才能從已啟動 SIL 記錄的伺服器透過 HTTPS 接收資料。|
|`Stop-SilAggregator`|停止所有軟體清查記錄彙總工具服務和工作。 如果工作或服務正在執行中，可能會延遲完成此命令。|
|`Set-SilAggregator`|可讓系統管理員變更軟體清查記錄彙總工具的組態。|
|`Add-SilVmHost`|用來新增要定期輪詢的特定主機名稱或主機名稱陣列， \( 預設為一小時的間隔 \) 。|
|`Remove-SilVmHost`|用於移除要定期輪詢的特定主機名稱或主機名稱陣列。|
|`Get-SilVMHost`|用於擷取供軟體清查記錄彙總工具已設定要輪詢進行中 VM 執行狀態資料的實體主機清單。|
|`Get-SILAggregatorData`|用於將資料庫中的資料擷取至 PowerShell 主控台。|
|`Publish-SilReport`|用於從軟體清查記錄資料的資料庫建立報告。 **注意：** 匯總工具上的 Cube 處理會一天執行一次。 因此，彙總工具所擷取的資料要等到隔天才會出現在報告中。|

#### <a name="suggested-order-to-start"></a>建議的啟動順序
在伺服器上安裝軟體清查記錄彙總工具之後，請以系統管理員身分開啟 PowerShell。

-   在 SIL 彙總工具上：

    -   `Start-SilAggregator`執行

        需要如此，彙總工具才能主動從您已經 (或將要) 設定要清查的伺服器，透過 HTTPS 接收轉送給它的資料。 請注意，即使您已經先啟用伺服器來轉送到此彙總工具，也沒有問題，因為它們會將資料內容快取在本機長達 30 天。 在匯總工具之後，其 "targeturi" 會啟動並執行，所有快取的資料會一次轉送到匯總工具，並處理所有資料。

    -   `Add-SilVMHost`執行

        範例： `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   在此範例中，**contoso1** 是您希望彙總工具輪詢的實體主機伺服器的網路名稱 (或 IP 位址)，為的是取得哪些 VM 正在執行的定期更新資訊，以便在一段時間內追蹤這項資料。 此後，Get-Credential 會提示使用者輸入用來輪詢此主機的帳戶。 在相同的主機上執行相同的命令，可讓您更新隨時要使用的帳戶。 請注意帳戶密碼經過一段時間會變更和過期。 如果認證變更或過期，在主機上輪詢會失敗。

        -   根據預設，從 `Start-SilAggregator` 執行之後的一小時開始，或從主機新增至輪詢清單之後的一小時開始，輪詢每小時執行一次。  您可以使用 `Set-SilAggregator cmdlet` 變更輪詢間隔。

        -   對於您要新增的主機，此 Cmdlet 會從預設的選項清單中 (請參閱 **SIL 彙總工具 Cmdlet 詳細資料**一節)，自動偵測正確的 HostType 和 HyperVisorType。 如果無法辨識這些資料或提供的認證不正確，將會顯示提示。 如果您輸入 **Y** 表示接受，則會新增主機而且列為 [未知]****，但不會輪詢它。

    -   執行「 `Set-SilAggregator –AddCertificateThumbprint` 您的用戶端憑證的指紋」

        需要如此，才能透過 HTTPS 從已啟用 SIL 記錄的 Windows Server 接收資料。 這個指紋會加入至供 SIL 彙總工具接受資料的指紋清單中。 SIL 彙總工具設計為接受有效的企業用戶端驗證憑證。 使用的憑證必須安裝在轉送資料的伺服器上的** \\ Localmachine\MY （本機電腦 > 個人**）存放區中。

-   在您要清查的 Windows Server 上，以系統管理員身分開啟 PowerShell 並執行下列命令：

    -   `Set-SilLogging –TargetUri "https://contososilaggregator" –CertificateThumbprint "your client certificate's thumbprint"`執行

        -   這會指告 Windows Server 中的 SIL 將清查資料傳送至何處，以及要用於驗證的憑證。

            > [!IMPORTANT]
            > 請確定 "HTTPs://' 是在 TargetUri 值中。

        -   具有此指紋的企業用戶端憑證必須安裝在 **\localmachine\MY**，或使用 **certmgr.msc** 將憑證安裝在 [本機電腦 -> 個人]**** 存放區。

            > [!IMPORTANT]
            > 如果這些值不正確，或憑證未安裝在正確的存放區 (或無效)，當 SIL 記錄啟動時，轉送至目標將會失敗。 資料會快取在本機長達 30 天。

    -   `Start-SilLogging`執行

        這會啟動 SIL 記錄。 SIL 會每小時依隨機間隔時間，將其清查資料轉送至 `–targeturi` 參數所指定的彙總工具。 第一次會轉送一組完整的資料。 後續每次轉送都會是「信號」更多，只會識別沒有任何變更的資料。 如果資料集有任何變更，將會轉送另一組完整的資料。

    -   `Publish-SilData`執行

        -   第一次啟用 SIL 來記錄時，這個步驟為選擇性。

        -   這是手動一次性轉送一組完整的資料。

        -   如果 SIL 記錄已啟動一段時間，然後以 `Set-SilLogging` 指定新的 SIL 彙總工具，則需要執行這個 Cmdlet (只需要執行一次)，將一組完整的資料傳送至新的彙總工具。

一旦您已遵循下列步驟來新增執行虛擬 Windows Server 電腦的實體主機，「而且」已在這些 Windows Server 內啟用軟體清查記錄 (或 SIL 記錄)，則您隨時可以在 SIL 彙總工具上執行 `Publish-SilReport –OpenReport` (需要 Excel 2013)。 但是請注意，SQL Server Analysis Services Cube 每天處理一次，因此資料不會出現在同一天的報告中。

## <a name="architectural-overview"></a>架構概觀
SIL 同時以推入和提取模式運作，由兩個平行運作的元件組成：Windows Server 中的軟體清查記錄 (SIL) 功能，以及軟體清查記錄彙總工具 (SILA) 可下載 MSI。 要清查的伺服器會利用 SIL 透過 HTTPS 將軟體清查資料推入 SIL 彙總工具 (依每小時內的隨機時間點)。 接著，彙總工具會每小時輪詢或查詢實體 Hypervisor 主機，以提取硬體清查資料。 推入和提取都必須適當設定，才能發揮 SIL 的完整功能。 這些可以依任何順序設定。 不過，彙總工具的 Cube 處理每天進行一次，因此，彙總工具上透過推入或提取所擷取的資料，必須等到隔天才會出現在報告中。

![軟體清查記錄匯總工具圖表](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> 使用此軟體並不會將任何資料傳送給 Microsoft。

## <a name="enable-sil-on-multiple-servers"></a>在多部伺服器上啟用 SIL
有幾種方式可以在分散式伺服器基礎結構中啟用 SIL，例如在虛擬機器的私人雲端中。  以下範例是將 Windows Server 映像設定為在網路上第一次在啟動時，自動將清查資料傳送至 SIL 彙總工具的一種方法。

在每個已安裝 Windows Server 的執行中 VM 或實體電腦/裝置上 (請參閱**必要條件**一節)，在 PowerShell 主控台中以系統管理員身分執行下列 Cmdlet：

您需要 .pfx 格式的有效用戶端 SSL 憑證。  必須使用 `Set-SILAggregator –AddCertificateThumbprint` Cmdlet 將此憑證的指紋加入您的 SIL 彙總工具。 此用戶端憑證不需要符合您 SIL 彙總工具的名稱。

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root`** < \\ server\path 以共用保存 pfx 憑證檔案>**`-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\`**<certificatename 的新磁片磁碟機目錄中的 .pfx 檔案> c：\<location of your choice>**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\`**<location \\ certificatename .pfx>**`cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi "https://`**<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE]
> 使用來自用戶端 pfx 檔案的憑證指紋，並使用**set-silaggregator '-AddCertificateThumbprint** Cmdlet 新增至您的 SIL 匯總工具。

-   `Start-sillogging`

每當無法存取 SIL 彙總工具時，SIL 清查資料就會在 Windows Server 本機快取長達 30 天。 一旦成功推入彙總工具，就會轉送所有快取的資料。

如果在 SIL 資料成功推入舊的彙總工具之後要再推入新的 SIL 彙總工具，請將 `Publish-SilData` 加入至上述清單 (這會傳送新的彙總工具所需要關於此電腦的一整組 SIL 資料)。

## <a name="software-inventory-logging-aggregator-reports"></a>軟體清查記錄彙總工具報告
![軟體清查記錄匯總工具報表的影像](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>Cube 處理
在軟體清查記錄彙總工具上，本機系統時間每天上午 3:00:00 會處理一次 SQL Server Analysis Services Cube。 報告會反映截至該時間為止的所有資料，但不會反映同一天該時間以後的任何資料。

### <a name="high-water-mark"></a>上限標記
軟體清查記錄匯總工具報告的一個基本層面，就是將一般稱為「高水位標記」的功能，同時執行 Windows 伺服器。 這適用於這些報告中的 Windows Server 和 System Center 計數。 就 Windows Server 而言，每一部實體主機在每個月的一個時間點 (不論主機的作業系統類型) 會有大多數 Windows Server VM 同時執行。 這就是每月上限標記。 此外，就 System Center 而言，每一部實體主機在每個月的一個時間點會有大多數受管理的 Windows Server 同時執行 (有一或多個 System Center 代理程式存在時，代表有受管理的伺服器)。 報告中只會顯示任何實體主機的最新上限標記。 不會顯示上限標記之後的任何資料。 可以假定在該點之後，Windows Server VM (WS 索引標籤) 或受管理 Windows Server VM (SC 索引標籤) 的數目低於上限標記。 這種追蹤及表示使用情況的方式主要是協助進行容量規劃，以及符合這些產品的授權模式。

在報表中的 SQL 相關索引標籤上，SQL Server 安裝會累積計算;not by hig-水線標記。 總數等於累計的 SQL Server 安裝。

> [!NOTE]
> 就算使用軟體清查記錄，仍然必須遵守適用的授權條款來準確報告 Microsoft 軟體使用情況。

### <a name="poll-date-time"></a>輪詢日期時間
使用軟體清查記錄彙總工具時，務必了解上限標記計數的彙總以輪詢為導向。 換句話說，只有輪詢基礎實體主機才能擷取上限標記。 因此，高水位標記計數會直接與對應的「輪詢日期時間」產生關聯。 雖然可調整輪詢間隔，但如果使用較高的間隔值，將會影響擷取到的上限標記的精確度。 間隔越長，資料就越無法代表實際的使用情況。

### <a name="reports-are-month-by-month"></a>報告以月份為單位
所有報告 (即使是年度報告) 都以月份報告表示。 每個行事曆月份開始時會重設上限標記、總計和電腦資料。

切換到新月份會影響的報告資料包括：

-   新的月份開始時會重設所有主機的所有上限標記。

-   如果彙總工具從 VM (透過 HTTPS) 收到至少一份完整的內容，但停止收到活動訊號，則在該月份內輪詢基礎主機時，都會將主機、VM 和 VM 資料之間的關聯假定為 VM 在整個月內持續執行或完全停止。 新的月份開始時會清除此關聯，直到從該 VM 收到完整內容或活動訊號為止。

### <a name="additional-notes-on-report-behavior"></a>報告行為的其他注意事項

-   摘要索引標籤可當做清查的快速參考清單。 主機及其 VM 列在相同的資料行。

-   忽略灰色或變暗的所有值。這些是從 SSAS cube 建立報表的構件。

-   如果 VM 是以「未知的 OS」列出，表示匯總工具尚未透過 HTTPS 透過 SIL 從該 VM 收到完整的資料裝載。

-   「不明主機」下所列的 Vm 是 Windows Server Vm 已成功將清查資料透過 HTTPS 轉送到匯總工具，但匯總工具不會主動或順利輪詢該 VM 的基礎主機。 因為基礎主機未知，這些項目的計數一律為零。 使用 `Add-SilVMHost` Cmdlet 並指定正確的認證，將主機 (或所有主機) 加入至 SIL 彙總工具來輪詢。 一旦成功輪詢，之後的報告就會將 VM 資料和主機資料相關聯。

-   所有日期和時間以 SIL 彙總工具本身的系統時間和地區設定為準。 這包括透過 HTTPS 從 SIL 啟用的系統接收的清查資料。 處理這些檔案時 (在收到後的 20 分鐘之內)，資料會以本機系統時間插入至資料庫。

-   「SIL 匯總工具」會在已安裝軟體清查記錄匯總工具的任何伺服器電腦上表示。

-   如果實體主機變更處理器數目或實體記憶體數量，則報告中除了舊的資料列，還會出現一個新的資料列。 舊的資料列上會停止輪詢更新，而在新的資料列上繼續，彷彿它是新加入的主機一樣。

-   在 [**摘要**] 和 [**詳細資料**] 索引標籤上，列在同時執行的 windows 伺服器或受管理的 windows 伺服器的 [欄數] 中，會指出下列所有主機的所有上限標記。 這些包括不是虛擬機器主機的 Windows 伺服器，而且沒有執行中的 Vm，以及可能執行 Vm 但狀態為「不明」的伺服器，因為從 VM 的 SIL 透過 HTTPS 不會收到任何資料。 為了方便起見，這些都已加總。

-   在 [儀表板]**** 索引標籤的 [SQL Server]**** 區段中，SQL Server 安裝總數是 [儀表板] 上所有版本總計的彙總。  在單一伺服器安裝多個 SQL 版本的情況下，這可能會導致 [SQL 詳細資料]**** 索引標籤上出現的總計不一致。  [儀表板] 會分別計算每個伺服器的這些數字，但 [詳細資料]**** 索引標籤並不會。  根據授權條款，一部 Windows Server 上安裝的多個 SQL 版本一律以 1 個版本計算。

-   在 [儀表板]**** 索引標籤的 [Windows Server]**** 區段中，[其他 Hypervisor 主機]**** 和 [Hypervisor 主機總計]**** 資料列包含可能或「沒有」執行 Hyper-V 的實體 Windows Server 主機。

### <a name="column-descriptions"></a>資料行描述
針對 SIL 彙總工具建立的 Excel 報告，以下是此報告的 [Windows Server 詳細資料]**** 索引標籤上每個資料行的描述。 其他資料索引標籤是相同或這些資料行的子集。 其中一個例外狀況是 [SQL Server] 索引標籤上的 [安裝計數] （請參閱**上限標記**一節）。

|資料行標頭|描述|
|-----------------|---------------|
|日曆月份|報告中的資料依月份分組，以最新月份為優先。 月份內的資料不依特定順序列出。|
|主機名稱|SIL 彙總工具成功輪詢的實體主機的網路名稱或 FQDN。<p>使用 Get-SilVMHost Cmdlet 尋找已加入但無法或不再成功輪詢的主機。 將會顯示最後一次成功的輪詢。|
|主機類型|實體主機的作業系統製造商。|
|Hypervisor 類型|實體主機的 Hypervisor 製造商。|
|處理器製造商|實體主機的處理器製造商。|
|處理器型號|實體主機的處理器型號。|
|已啟用超執行緒？|視實體主機的處理器是否啟用超執行緒而定，顯示為 True 或 False。|
|虛擬機器名稱|Windows Server 虛擬機器的網路名稱或 FQDN。 如果彙總工具透過 HTTPS 未收到這部電腦的資料，則會列出 Hypervisor 中的 VM 易記名稱。|
|主機同時執行的 Windows Server VM|主機上同時執行的 Windows Server VM 的計數。 該主機在月份中的最高數字是指在該時間點列出和擷取的上限標記計數。<p>請參閱本文件的**上限標記**一節。<p>已安裝 Windows Server 的實體主機，或已安裝 Windows Server 但未執行已知 Windows Server VM 的實體主機，一律以 1 個計算。 如果主機上至少執行一個已知的 Windows Server VM，而且主機本身也執行 Windows Server，則主機作業系統不納入計算。|
|實體處理器計數|安裝在實體主機上的實體處理器數目。|
|實體核心計數|安裝在實體主機上的實體處理器核心數目。|
|虛擬處理器計數|Windows 在 VM 內可辨識的虛擬處理器數目。 這個值只來自 Windows Server 中使用 SIL 透過 HTTPS 轉送的資料。|
|輪詢日期時間|該實體主機上同時執行的 Windows Server VM 的最新上限標記點的日期和時間。<p>請參閱本檔的**輪詢日期時間**一節。|
|前次看到 VM 的日期時間|彙總工具前次透過 HTTPS 從這個 Windows Server VM 收到資料清查的日期和時間。|
|前次看到主機的日期時間|彙總工具前次透過 HTTPS 從這個 Windows Server 實體伺服器收到資料清查的日期和時間。<p>它支援執行 Windows Server 和 HyperV 的實體主機啟用 SIL，並透過 HTTPS 將清查資料轉送至 SIL 彙總工具。|

## <a name="sil-aggregator-cmdlets-detail"></a>SIL 彙總工具 Cmdlet 詳細資料
以下是 SIL 彙總工具 Cmdlet 的詳細資料。 如需完整的 Cmdlet 文件，請參閱：[SIL 彙總工具 PowerShell Cmdlet](https://technet.microsoft.com/library/mt548455.aspx)

### <a name="publish-silreport"></a>Publish-SilReport

-   此 Cmdlet 的使用方式為，會建立軟體清查記錄報告，並將它放在登入使用者的 Documents 目錄中（執行此 Cmdlet 的電腦上需要 Excel 2013）。

-   此 Cmdlet 搭配 `–OpenReport` 參數一起使用，將建立報告並在 Excel 中開啟來檢視。

-   安裝 SIL 彙總工具時，您會發現有一個只安裝報告模組的選項。 您可以將報告模組安裝在 Windows 用戶端作業系統上，例如 Windows 8.1 或 Windows 10。 這可讓精簡型用戶端 (像是膝上型電腦或平板電腦) 連線到 SIL 彙總工具資料庫伺服器來直接發佈 SIL 報告。

    -   下列範例會提示您輸入要使用的認證、連接至名為 SILContoso 的 SIL 彙總工具資料庫伺服器，然後在本機電腦上建立並開啟 SIL 報告。

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   在大部分情況下，第一次連接之前，您必須 SIL 彙總工具資料庫伺服器的防火牆中開啟連接埠來允許連接。 IT 專業人員可以事先做好此設定，以允許財務總監或其他清查管理員有權建立自己的報告。 如需相關步驟，請參閱下面的連結。 SQL Server Analysis Services 的一般預設連接埠是 2383。

### <a name="add-silvmhost"></a>Add-SilVMHost
使用 `Add-SilVMHost` Cmdlet 時支援下列主機類型和 Hypervisor 版本。 請注意，不需要指定這些設定。 `Add-SilVMHost` Cmdlet 會自動偵測支援的組合。 如果無法偵測或提供的認證不正確，將會顯示提示。 如果使用者接受 "Y" 專案，將會加入主機，但不會進行輪詢。 它會新增為「不明」。

|Hypervisor 版本|SIL 彙總工具         HostType 值|SIL 彙總工具 HypervisorType 值|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server 2012 R2|Windows|HyperV|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu、OpenSuse 或 CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu、OpenSuse 或 CentOS|KVM|

這些 Hypervisor 平台和類型的其他版本也可能適用。  SIL 彙總工具隨附於下面的 sshnet 版本。  這用來與 Linux 型虛擬化平台進行通訊。

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
`Get-SilAggregator` 提供軟體清查記錄彙總工具應用程式的組態資訊。 下列範例輸出顯示：

-   應用程式正在執行

-   輪詢間隔為每個小時 (以小時為單位遞增)

-   輪詢開始時間

-   為了將資料轉送至此彙總工具，其他電腦應該設定的目標 URI

-   供此彙總工具接受 SIL 資料的來源憑證指紋

-   安裝時指定的帳戶類型

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
`Set-SilAggregator` Cmdlet 可讓您：

-   變更輪詢的每小時間隔。

-   變更開始日期和時間，依指定的間隔開始輪詢。

-   新增或移除供 SIL 彙總工具接受資料或產生關聯的憑證指紋。

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   此 Cmdlet 單獨使用時，將顯示 SIL 彙總工具 Excel 報告的 [Windows Server 詳細資料] 索引標籤的內容。

-   此 Cmdlet 搭配參數使用時，將直接從用於協助自訂使用 SIL 整體解決方案的資料庫中擷取資料。

-   請注意，`–StartTime` 和 `–Endtime` 參數會從顯示從開始日期的月份開頭到結束日期的月份結尾的報告資料。

![已完成 AggregatorData Cmdlet 的影像](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   此 Cmdlet 會輸出 SIL 彙總工具已設定要輪詢的實體主機清單、最近成功輪詢的日期和時間，以及 HostType (或作業系統製造商) 和 HypervisorType (Hypervisor 製造商)。 如需 HostType 和 HypervisorType 的詳細資訊，請參閱 Add-SilVMHost 詳細資料。

    如果主機沒有輪詢日期和時間，但有支援的 HostType 與 HypervisorType，這表示輪詢尚未開始或尚未成功。

-   此 Cmdlet 也會列出已透過來自 VM 本身的資料而加入的任何主機名稱的清單 (如果 VM 有提供的話)。 這些將會出現在清單中，但不會有任何 HostType 或 HypervisorType。 這項資料可協助匹配 VM 和可能未設定要輪詢的主機。

-   使用 `–StartTime` 和 `–EndTime` 參數可協助您了解主機第一次加入或前次輪詢的時間。

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   此 Cmdlet 將從要輪詢的主機清單中移除任何主機。 如果移除主機，主機上的 VM 可能會將主機重新加入至清單中，但將不會使用 `Add-SilVMHost` Cmdlet 所指定的正確認證來輪詢主機。

-   如果移除主機，它會從輪詢中移除，但不會從報告中移除。 由於將停止輪詢，主機將不會出現在下個月的報告上。

-   使用 `–StartTime` 和`–EndTime` 參數分別可協助您移除截止某日為止或從某日開始已成功輪詢的主機群組。

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>使用 SIL 和 SIL 彙總工具避免這些錯誤和問題 (疑難排解指南)

-   `SilLogging` 或 `Publish-Sildata` Cmdlet 失敗或錯誤時的檢查事項：

    -   請確定**targeturi**已**HTTPs://** 在專案中。

    -   請確定已安裝 Windows Server 的所有必要更新 (請參閱「SIL 的必要條件」)。  若要快速檢查，請使用下列 Cmdlet 來尋找這些方法：`Get-SilWindowsUpdate *3060*, *3000*`

    -   請確定用於向彙總工具驗證的憑證，已安裝在要以 SilLogging 清查的本機伺服器的正確存放區中 (請參閱「開始使用」一節)。

    -   在 SIL 彙總工具上，請確定用於向彙總工具驗證的憑證的憑證指紋，已透過 `Set-SilAggregator –AddCertificateThumbprint` Cmdlet 加入至清單 (請參閱「開始使用」一節)。

    -   如果使用企業憑證，請檢查 SIL 啟用的伺服器已加入建立憑證時所針對的網域，或可由根授權單位來驗證。 如果嘗試將資料轉送/推入彙總工具在本機電腦不信任憑證，這個動作會失敗並產生錯誤。

    -   如果已檢查上述所有情況，接著您可以檢查用來安裝 SIL 彙總工具的憑證狀況良好，且符合 SIL 彙總工具伺服器本身的名稱 (如果其他電腦順利轉送至相同的 SIL 彙總工具，則不需要此步驟)。

    -   您可以在嘗試轉送/推送 \Windows\System32 記錄檔 SIL 的伺服器上，檢查快取 SIL 檔案的下列位置 \\ \\ 。 如果 `SilLogging` 已啟動且執行超過一小時，或 `Publish-SilData` 才剛執行不久，因而此目錄中沒有任何檔案，則表示已成功登入彙總工具。

-   請確認登入的使用者具有 SQL 資料庫和 Analysis Services 存取權。

    -   安裝 SIL 彙總工具時需要如此。

    -   當您從遠端使用 PowerShell 來管理 SIL 匯總工具時，這是必要的。

-   從用戶端桌上型電腦作業系統發佈 SIL 彙總工具報告。

    -   使用只將報告模組安裝在 Windows 用戶端 (8.1/10) 的選項。

    -   當您嘗試從遠端使用 powershell 建立報告時，如果遇到問題，您可能需要在嘗試連接的 SIL 彙總工具上開啟防火牆連接埠 (請參閱「SIL 彙總工具 Cmdlet 詳細資料」一節中的 `Publish-SilReport` Cmdlet)。

-   使用 gMSA 選項時：

    -   在 Active Directory 中，將伺服器加入已啟用 gMSA 的電腦群組之後，別忘了將它重新開機。

    -   在安裝過程中，輸入 domain\user 時不要使用完整的網域 例如，使用**mydomain\gmsaaccount**。 請勿輸入**mydomain。 <i></i>com\gmsaaccount**。

-   在您的環境中使用 Windows Management Framework 時：

    -   請確定已安裝 SILA 的伺服器未安裝 WMF 5.1。  您可能會在事件記錄檔中遇到關於 DLL **' mpunits.dll '** 的錯誤。  這將導致無法正常運作。  SILA 只需要 WMF 4.0。

## <a name="managing-sil-over-time"></a>持續管理 SIL

### <a name="uninstallreinstall-sil-aggregator"></a>解除安裝/重新安裝 SIL 彙總工具
如果需要解除安裝再重新安裝 SIL 彙總工具，您也不會遺失現有和歷史清查資料。 只要解除安裝 (按照本文件提供的步驟)，並選取保留軟體清查記錄資料庫的選項即可。 然後，重新安裝 SIL 彙總工具 (按照本文件提供的步驟)，並選取連接至現有資料庫的選項。

執行此作業之後，在先前由 SIL 彙總工具輪詢的任何主機上，必須使用 `Add-SilVMHost` Cmdlet 更新認證 (假設需要繼續從這些主機收集資料)。 此外，為了避免相同的主機在報告上出現重複的項目，必須使用原先加入時的相同網路位址，重新加入主機來輪詢。 以下是使用 `Add-SilVMHost` Cmdlet 時支援用來加入主機的三種 vmhostname 類型：

-   IP 位址

-   完整網域名稱 (FQDN)

-   Netbios 名稱

### <a name="changing-sil-aggregators"></a>變更 SIL 彙總工具
當您想要使用不同的 SIL 彙總工具來開始清查您環境中的伺服器時，只要在這些伺服器上使用此 SIL Cmdlet 變更 targeturi 即可 (必要的話，也要變更憑證指紋)：`Set-SilLogging –TargetUri`。 請注意，這麼做之後，至少需要使用一次 `Publish-SilData` Cmdlet，將完整清查轉送至新指定的 SIL 彙總工具。

### <a name="changing-or-updating-certificates"></a>變更或更新憑證
**避免資料遺失的重要步驟：** 如果需要變更伺服器用來將資料轉送至 SIL 彙總工具的憑證，但目標彙總工具維持相同，請使用下列步驟，以避免資料在傳輸至彙總工具途中可能遺失：

-   在 SIL 彙總工具上，使用 `Set-SilAggregator –AddCertificateThumbprint` Cmdlet 將新的指紋加入至 SIL 彙總工具。

-   在「所有」轉送資料的伺服器上，使用您慣用的方法將想要使用的新憑證安裝在 **\LOCALMACHINE\MY** 中。

-   在「所有」轉送資料的伺服器上，使用 `Set-SilLogging –CertificateThumbprint` Cmdlet 將新憑證的指紋更新。

-   **重要：只有在所有轉送資料的伺服器都更新之後，** 才使用 `Set-SilAggregator –RemoveCertificateThumbprint` Cmdlet 從 SIL 彙總工具中移除舊的指紋。 如果轉送資料的伺服器繼續使用已從 SIL 彙總工具移除的舊憑證來轉送， **資料將會遺失** ，不會插入至彙總工具的資料庫中。 這只會影響伺服器先前已成功將資料轉送至 SIL 匯總工具的案例，然後從 SIL 匯總工具的指紋清單中移除憑證，以接受來自的資料。

## <a name="release-notes"></a>版本資訊

-   安裝 SQL Server Standard Edition 時，SIL 彙總工具不會處理並加以回報，這是已知問題。  若要更正此問題，步驟如下：

    1.  開啟您 SIL 彙總工具中的 SQL Server Management Studio。

    2.  連線至資料庫引擎。

    3.  在選取的樹目錄中依序展開 SoftwareInventoryLogging 資料庫及資料表。

    4.  以滑鼠右鍵按一下 [ **dbo]。Dbo.sqlserveredition]**，然後選取 [**編輯前 200**個數據列]。

    5.  將 [Standard Edition] 旁的 PropertyNumValue 變更為**2760240536** （從-1534726760）。

    6.  關閉查詢以儲存變更。

    7.  對於已將資料記錄到此彙總工具的所有執行 SIL 的伺服器，可能需要針對彙總工具執行一次 `Publish-SilData` Cmdlet，才能正確處理目前的 SQL Server Standard Edition。

-   在 SIL 產生的報告中，如果實體伺服器上已啟用超執行緒，則所有處理器核心計數會包含執行緒的計數。  若要得到啟用超執行緒的伺服器上的實際實體核心計數，必須將這些計數減少一半。

-   資料列（在 [**儀表板**] 索引標籤上）和標示為「**同時**執行 ...」的**資料**行總計，在兩個位置之間並不完全相符。 在 [**儀表板**] 索引標籤上，必須將「**Windows Server 裝置（沒有已知的 vm**）」值新增至「**同時**執行 ...」在 [**摘要] 和 [詳細資料**] 索引標籤上，等於此數目的值。

-   在本文件的**持續管理 SIL** 一節中變更或更新憑證時，請參閱**避免資料遺失的重要步驟**。

-   雖然可以將 Windows Server 2008 R2 和 Windows Server 2012 主機加入至輪詢主機清單，但此版本 (1.0) 的 SIL 彙總工具只支援在輪詢 Windows Server 2012 R2 時 (以 Windows/Hyper-V 為基礎的主機)，才能夠順利使用所有特性和功能。  特別的是，已知輪詢 Windows Server 2008 R2 主機時，虛擬機器和主機在 SIL 彙總工具報告中可能不匹配。

## <a name="see-also"></a>另請參閱
[適用於 Windows Server 的軟體清查記錄彙總工具 1.0](https://www.microsoft.com/download/details.aspx?id=49046)<br>
[SIL 彙總工具 PowerShell Cmdlet](https://technet.microsoft.com/library/mt548455.aspx)<br>
[SIL PowerShell Cmdlet](https://technet.microsoft.com/library/dn283390.aspx)<br>
[SIL 概觀](https://technet.microsoft.com/library/dn268301.aspx)<br>
[管理 SIL](https://technet.microsoft.com/library/dn383584.aspx)

