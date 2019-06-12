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
ms.openlocfilehash: bd35e439ee3c76af1306bbbd712d754dd79f555f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446095"
---
# <a name="get-started-with-windows-admin-center"></a>開始使用 Windows Admin Center

>適用於：Windows Admin Center，Windows Admin Center 預覽

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows 10 上安裝的 Windows Admin Center

> [!IMPORTANT]
> 您必須使用 Windows Admin Center，在 Windows 10 上的本機系統管理員群組的成員

### <a name="selecting-a-client-certificate"></a>選取用戶端憑證

請確定選取的第一次開啟 Windows 10 上的 Windows Admin Center *Windows Admin Center 用戶端*（否則您將獲得 HTTP 403 錯誤，指出 「 無法取得此頁面 」） 的憑證。

在 Microsoft Edge 中，當系統提示您使用此對話方塊：
 
1. 按一下 **更多選擇**

    ![](../media/launch-cert-1.png)

2. 選取標示的憑證**Windows Admin Center 用戶端**，按一下  **確定**

    ![](../media/launch-cert-2.png)

3. 請確定**允許存取**已選取，然後按一下 **允許**

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>連接到受管理的節點和叢集

您已完成安裝的 Windows Admin Center 之後，您可以新增伺服器或叢集，以從主要概觀頁面上管理。

 **將單一伺服器或叢集新增為受管理的節點**

1. 按一下  **+ 加入**下方**所有連線**。

   ![](../media/launch/addserver0.png)

2. 選擇 新增伺服器、 容錯移轉叢集或 Hyper-Converged 叢集連線：
    
   ![](../media/launch/addserver1.png)

3. 輸入名稱的伺服器或叢集管理，並按一下**送出**。 伺服器或叢集會加入您在 [概觀] 頁面上的連接清單。

   ![](../media/launch/addserver2.png)

   **-- OR --**

**大量匯入多部伺服器**

 1. 在 [**加入伺服器連接**頁面上，選擇**匯入伺服器**] 索引標籤。

    ![](../media/launch/import-servers.png)

 2. 按一下 **瀏覽**和選取文字的檔案，包含逗號或換行分隔，您想要新增之伺服器的 Fqdn 清單。

    **-- OR --**

**藉由搜尋 Active Directory 中新增伺服器**

 1. 在 [**加入伺服器連接**頁面上，選擇**搜尋 Active Directory** ] 索引標籤。

    ![](../media/launch/search-ad.png)

 2. 輸入您的搜尋準則，然後按一下**搜尋**。 支援萬用字元 （*）。

 3. 搜尋完成後-選取一或多個結果，選擇性地新增標記，然後按一下**新增**。

## <a name="authenticate-with-the-managed-node"></a>向受管理的節點 ##

Windows Admin Center 會支援數種機制，用來驗證與受管理的節點。 單一登入是預設值。

**單一登入**

您可以使用您目前的 Windows 認證來向受管理的節點。 這是預設值，而且 Windows Admin Center 嘗試登入，當您新增的伺服器。 

**單一登入部署為 Windows 伺服器上的服務時**

如果您已在 Windows Server 上安裝 Windows Admin Center，則需要進行單一登入其他組態。  [設定您的環境，以便進行委派](../configure/user-access-control.md)

**-- OR --**

**使用*管理身分*指定認證**

底下**所有連線**，從清單中選取伺服器，然後選擇**管理身分**以指定的認證，您將用來向受管理的節點：

![](../media/launch-use-6.png)

如果 Windows Admin Center 在服務模式中執行 Windows Server 上，但您不需要設定 Kerberos 委派，您必須重新輸入您的 Windows 認證：

![](../media/launch-use-7.png)

您可能會將認證套用至所有連線，請將快取它們，為該特定瀏覽器工作階段。 如果您重新載入您的瀏覽器，您必須重新輸入您**管理身分**認證。

**本機系統管理員密碼解決方案 (LAPS)**

如果您的環境使用[LAPS](https://technet.microsoft.com/mt227395.aspx)，您必須安裝在您的 Windows 10 電腦上的 Windows Admin Center，您可以使用 LAPS 認證來向受管理的節點。 **如果您使用這種情況下，請**[提供意見反應](http://aka.ms/WACFeedback)。

## <a name="using-tags-to-organize-your-connections"></a>使用標記來組織您的連線

您可以使用標記來識別，並在您的連線清單篩選相關的伺服器。  這可讓您檢視您的伺服器在連線清單中的子集。  這是特別有用，如果您有許多的連線。

### <a name="edit-tags"></a>編輯標記

* 所有連線清單中選取伺服器或多部伺服器
* 底下**所有連線**，按一下 **編輯標記**

![](../media/launch/tags-5.png)

**編輯連接標記**窗格可讓您修改、 新增或移除您所選取的連線中的標記：

* 若要將新的標記新增至您所選取的連線中，選取**新增標記**並輸入您想要使用的標記名稱。

* 若要標記與現有的標記名稱選取的連接，請檢查您想要套用的標籤名稱旁的方塊。

* 若要移除所有選取的連接中的標記，取消核取您想要移除的標籤旁的方塊。

* 如果標記套用至所選連接的子集，核取方塊會顯示處於中繼狀態。 您可以按一下此方塊可檢查它，並且將標記套用到所有選取的連線，或按一次將其取消選取，並從所有選取的連線中移除標記。

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>依標記篩選連接

一旦標記已新增至一或多個伺服器連線，您可以在 [連接] 清單中，檢視的標記，並依據標記篩選連接清單。

* 若要依標記篩選，請選取 [搜尋] 方塊旁邊的篩選圖示。
![](../media/launch/tags-7.png)
* 您可以選取"，"and"或"not，"以修改選取的標籤篩選行為。
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>使用 PowerShell 匯入或匯出您的連線 （含標記）

> 適用於：Windows Admin Center 預覽版

Windows Admin Center Preview 包含 PowerShell 模組匯入或匯出您的連線清單。

```powershell
# Load the module
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ConnectionTools"
# Available cmdlets: Export-Connection, Import-Connection

# Export connections (including tags) to .csv files
Export-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
# Import connections (including tags) from .csv files
Import-Connection "https://wac.contoso.com" -fileName "WAC-connections.csv"
```

### <a name="csv-file-format-for-importing-connections"></a>匯入連線的 CSV 檔案格式

CSV 檔案的格式會啟動包含四個標題```"name","type","tags","groupId"```，後面接著新行上的每個連線。

**名稱**是連線的 FQDN

**型別**是連線類型。 若為預設連線隨附於 Windows Admin Center 的情況下，您將使用下列其中一項：

| 連線類型 | 連接字串 |
|------|-------------------------------|
| Windows Server | msft.sme.connection-type.server |
| Windows 10 電腦 | msft.sme.connection-type.windows-client |
| 容錯移轉叢集 | msft.sme.connection-type.cluster |
| 超交集叢集 | msft.sme.connection-type.hyper-converged-cluster |

**標記**會以管道分隔。

**groupId**用於共用的連線。 使用值```global```在本專欄中讓此共用的連線。

### <a name="example-csv-file-for-importing-connections"></a>範例 CSV 檔案匯入連線

```
"name","type","tags","groupId"
"myServer.contoso.com","msft.sme.connection-type.server","hyperv"
"myDesktop.contoso.com","msft.sme.connection-type.windows-client","hyperv"
"teamcluster.contoso.com","msft.sme.connection-type.cluster","legacyCluster|WS2016","global"
"myHCIcluster.contoso.com,"msft.sme.connection-type.hyper-converged-cluster","myHCIcluster|hyperv|JIT|WS2019"
"teamclusterNode.contoso.com","msft.sme.connection-type.server","legacyCluster|WS2016","global"
"myHCIclusterNode.contoso.com","msft.sme.connection-type.server","myHCIcluster|hyperv|JIT|WS2019"
```

## <a name="import-rdcman-connections"></a>匯入 RDCman 連線

使用下列指令碼匯出中儲存的連線[RDCman](https://blogs.technet.microsoft.com/rmilne/2014/11/19/remote-desktop-connection-manager-download-rdcman-2-7/)至檔案。 您可以接著將檔案匯入 Windows Admin Center，維護使用標記 RDCMan 群組階層。 試試看 ！

1. 複製並貼上下列程式碼到您的 PowerShell 工作階段：

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

3. 匯入所產生的。中的 CSV 檔案，以 Windows Admin Center 和所有 RDCMan 群組階層將以連接清單中的標記來表示。 如需詳細資訊，請參閱 <<c0> [ 使用 PowerShell 匯入或匯出您的連線 （含標記）](#use-powershell-to-import-or-export-your-connections-with-tags)。

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>檢視 Windows Admin Center 中使用的 PowerShell 指令碼

一旦您已連線到伺服器、 叢集或電腦，您可以查看的 PowerShell 指令碼，電源可用的 UI 動作的 Windows Admin Center。 從在工具中，按一下最上層應用程式列中的 [PowerShell] 圖示。 選取感興趣的命令，從下拉式清單中，瀏覽至對應的 PowerShell 指令碼。

![](../media/launch/showscript.png)