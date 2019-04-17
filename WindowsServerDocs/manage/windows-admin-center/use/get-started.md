---
title: 開始使用 Windows Admin Center
description: 開始使用 Windows Admin Center
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/15/2019
ms.openlocfilehash: f4fd9f69e75ed80bbdb345b4041c2337c65ec2e6
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296650"
---
# 開始使用 Windows Admin Center

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## Windows Admin Center 安裝在 Windows 10

> [!IMPORTANT]
> 您必須是在 Windows 10 上使用 Windows Admin Center 的本機系統管理員群組的成員

### 選取用戶端憑證

第一次開啟 Windows Admin Center 在 Windows 10，請務必選取*Windows Admin Center 用戶端*憑證 （否則您會收到 HTTP 403 錯誤訊息指出 「 無法取得此頁面 」）。

在 Microsoft Edge 中，當系統提示您使用這個對話方塊時：
 
1. 按一下 [**更多的選項**

    ![](../media/launch-cert-1.png)

2. 選取標示為**Windows Admin Center 用戶端**憑證，然後按一下 **[確定]**

    ![](../media/launch-cert-2.png)

3. 請確定已選取 [**一律允許存取**，並按一下 [**允許**]

    ![](../media/launch-cert-3.png)

## 連線到受管理的節點和叢集

您已完成的 Windows Admin Center 安裝之後，您可以新增伺服器或叢集管理從主要的概觀頁面。

 **新增在單一伺服器或叢集，做為受管理的節點**

 1. 按一下 [ **+ 新增**下**所有的連線**。

    ![](../media/launch/addserver0.png)

 2. 選擇新增伺服器、 容錯移轉叢集或超融合式叢集連線：
    
    ![](../media/launch/addserver1.png)

 3. 輸入伺服器或叢集，來管理並按一下 [**提交**的名稱。 伺服器或叢集將會新增到您在 [概觀] 頁面上的連線清單。

    ![](../media/launch/addserver2.png)

   **-- 或 --**

**大量匯入多部伺服器**

 1. 在**新增伺服器連線**頁面上，選擇 [**匯入的伺服器**] 索引標籤。

    ![](../media/launch/import-servers.png)

 2. 按一下 [**瀏覽**並選取的文字檔案，包含逗號或新行分隔，針對您想要新增的伺服器的 Fqdn 清單。

    **-- 或 --**

**搜尋 Active Directory 以新增伺服器**

 1. 在**新增伺服器連線**頁面上，選擇 [**搜尋 Active Directory** ] 索引標籤。

    ![](../media/launch/search-ad.png)

 2. 輸入您的搜尋條件，然後按一下 [**搜尋]**。 支援萬用字元 （*）。

 3. 搜尋完成之後-選取一或多個結果、 選擇性地新增標記，並按一下 [**新增**]。

## 使用受管理的節點進行驗證 ##

Windows Admin Center 支援許多機制，用於驗證的受管理的節點。 單一登入是預設值。

**單一登入**

您可以使用您目前的 Windows 認證向受管理的節點。 這是預設值，並新增伺服器時，Windows Admin Center 嘗試登入。 

**單一登入時即服務 Windows Server 上部署**

如果您已在 Windows Server 上安裝 Windows Admin Center，額外的設定是必要項目單一登入。  [設定您的環境，委派](..\configure\user-access-control.md)

**-- 或 --**

**使用*管理身分*來指定認證**

在下**所有的連線**，從清單中選取伺服器並選擇 [**管理身分**來指定您要用來向受管理的節點進行驗證的認證：

![](../media/launch-use-6.png)

如果 Windows Admin Center 在 Windows Server 上執行服務模式，但您不需要設定的 Kerberos 委派，您必須重新輸入您的 Windows 認證：

![](../media/launch-use-7.png)

您可能會將憑證套用到所有的連線，會將它們快取該特定瀏覽器工作階段。 如果您重新載入您的瀏覽器，您必須重新輸入您**管理身分**」 認證。

**本機系統管理員密碼解決方案 （圈）**

如果您的環境使用[圈](https://technet.microsoft.com/mt227395.aspx)，您可以使用圈認證向受管理的節點。 **如果您使用此案例中，請**[提供意見反應](http://aka.ms/WACFeedback)。

## 使用標記來組織您的連線

您可以使用標記來找出並在您的連線清單篩選相關的伺服器。  這可讓您查看您的伺服器連線清單中的子集。  這是特別有用，如果您有許多的連線。

### 編輯標籤

* 所有的連線清單中選取的伺服器或多部伺服器
* 在**所有的連線**，按一下 [**編輯標記**

![](../media/launch/tags-5.png)

**編輯連線標記**窗格可讓您修改、 新增或移除標記從您所選取的連線：

* 將新的標記新增到您所選取的連線，選取 [**新增標籤**並輸入您想要使用的標記名稱。

* 若要標記與現有的標記名稱的所選的連線時，請檢查您想要套用的標記名稱旁邊的方塊。

* 若要移除所有選取的連線的標籤，取消選取您想要移除的標記旁邊的方塊。

* 如果標記套用至所選連線的子集，核取方塊會顯示處於中繼狀態。 您可以按一下此方塊，檢查它與標記會套用至所有選取的連線，或再次按一下 [取消選取它，並移除所有的所選連線的標籤。

![](../media/launch/tags-6.png)

### 依標記篩選連線

一旦標記已新增至一或多個伺服器連線，您可以檢視的標記的連線清單上，並依標記篩選連線清單。

* 若要篩選的標記，請選取 [搜尋] 方塊旁邊的篩選圖示。
![](../media/launch/tags-7.png)
* 您可以選取 」 或 「 「 及 」 或 「 不 」 來修改選取標記的篩選器行為。
![](../media/launch/tags-8.png)

## 使用 PowerShell 來匯入或匯出您的連線 （使用標籤）

> 適用於： Windows Admin Center 預覽版

Windows Admin Center 預覽版包含 PowerShell 模組匯入或匯出您的連線清單。

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### 匯入連線的 CSV 檔案格式

CSV 檔案格式的開頭的四個標題```"name","type","tags","groupId"```，後面接著在新的一行每個連線。

**名稱**是連線的 FQDN

**類型**是連線類型。 針對包含使用 Windows Admin Center 預設連線，您將使用下列其中一項：

| 連線類型 | 連接字串 |
|------|-------------------------------|
| Windows Server | msft.sme.connection type.server |
| Windows 10 電腦 | msft.sme.connection type.windows-用戶端 |
| 容錯移轉叢集 | msft.sme.connection type.cluster |
| 超融合式叢集 | msft.sme.connection type.hyper-融合-叢集 |

**標記**是管道分隔。

**groupId**用於共用的連線。 使用值```global```為了讓這個連線共用此欄中。

### 範例 CSV 檔案以供匯入連線

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## 匯入 RDCman 連線

您可以使用下方的指令碼來儲存的連線中[RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/)匯出至檔案。 您可以接著檔案匯入 「 Windows Admin Center，維護您使用標記的 RDCMan 群組階層。 試試看 ！

1. 複製並貼到您的 PowerShell 工作階段的以下程式碼：

   ```powershell
   #Helper function for RdgToWacCsv
   function AddServers {
    param (
    [Parameter(Mandatory = $true)]
    [Xml.XmlLinkedNode]
    $node,
    [Parameter()]
    [String[]]
    $tags,
    [Parameter(Mandatory = $true)]
    [String]
    $csvPath
    )
    if ($node.LocalName -eq 'server') {
        $serverName = $node.properties.name
        $tagString = $tags -join "|"
        Add-Content -Path $csvPath -Value ('"'+ $serverName + '","msft.sme.connection-type.server","'+ $tagString +'"')
    } 
    elseif ($node.LocalName -eq 'group' -or $node.LocalName -eq 'file') {
        $groupName = $node.properties.name
        $tags+=$groupName
        $currNode = $node.properties.NextSibling
        while ($currNode) {
            AddServers -node $currNode -tags $tags -csvPath $csvPath
            $currNode = $currNode.NextSibling
        }
    } 
    else {
        # Node type isn't relevant to tagging or adding connections in WAC
    }
    return
   }

   <#
   .SYNOPSIS
   Convert an .rdg file from Remote Desktop Connection Manager into a .csv that can be imported into Windows Admin Center, maintaining groups via server tags. This will not modify the existing .rdg file and will create a new .csv file

    .DESCRIPTION
    This converts an .rdg file into a .csv that can be imported into Windows Admin Center.

    .PARAMETER RDGfilepath
    The path of the .rdg file to be converted. This file will not be modified, only read.

    .PARAMETER CSVdirectory
    Optional. The directory you wish to export the new .csv file. If not provided, the new file is created in the same directory as the .rdg file.

    .EXAMPLE
    C:\PS> RdgToWacCsv -RDGfilepath "rdcmangroup.rdg"
    #>
   function RdgToWacCsv {
    param(
        [Parameter(Mandatory = $true)]
        [String]
        $RDGfilepath,
        [Parameter(Mandatory = $false)]
        [String]
        $CSVdirectory
    )
    [xml]$RDGfile = Get-Content -Path $RDGfilepath
    $node = $RDGfile.RDCMan.file
    if (!$CSVdirectory){
        $csvPath = [System.IO.Path]::GetDirectoryName($RDGfilepath) + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    } else {
        $csvPath = $CSVdirectory + [System.IO.Path]::GetFileNameWithoutExtension($RDGfilepath) + "_WAC.csv"
    }
    New-item -Path $csvPath
    Add-Content -Path $csvPath -Value '"name","type","tags"'
    AddServers -node $node -csvPath $csvPath
    Write-Host "Converted $RDGfilepath `nOutput: $csvPath"
   }
   ```

2. 若要建立。CSV 檔案中，執行下列命令：

   ```powershell
   RdgToWacCsv -RDGfilepath "path\to\myRDCManfile.rdg"
   ```

3. 匯入所產生的。將會由 [連線] 清單中的標籤表示 CSV 檔案中的對 Windows Admin Center，而您所有的 RDCMan 群組階層。 如需詳細資訊，請參閱[使用 PowerShell 匯入或匯出您的連線 （使用標籤）](#use-powershell-to-import-or-export-your-connections-with-tags)。

## 檢視使用 Windows Admin Center 中的 PowerShell 指令碼

一旦您已連線到伺服器、 叢集或電腦，您可以看看 PowerShell 指令碼，電源可用的 UI 動作 Windows Admin Center 中。 從在工具中，按一下 PowerShell 中的圖示的最上層應用程式列。 從瀏覽到對應的 PowerShell 指令碼的下拉式清單中選取感興趣的命令。

![](../media/launch/showscript.png)