---
title: 收集診斷資料與儲存空間直接存取
description: 了解儲存體空間直接存取資料收集工具，以特定範例，示範如何執行，並使用它們。
keywords: 儲存空間、 資料收集、 疑難排解、 事件通道，Get SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 51cf96fb462b68f2ba01d49642a858430c71e9f5
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469607"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>收集診斷資料與儲存空間直接存取

> 適用於：Windows Server 2019，Windows Server 2016

有一些可用來收集資料的儲存空間直接存取 」 和 「 容錯移轉叢集疑難排解所需的各種診斷工具。 在本文中，我們將著重**Get SDDCDiagnosticInfo** -一種有一個觸控工具，將會收集所有相關的資訊，協助您診斷您的叢集。

假設記錄和其他資訊， **Get SDDCDiagnosticInfo**是密集，疑難排解下方顯示的資訊很有幫助以進階已呈報的和，可能的問題進行疑難排解需要將資料傳送給 Microsoft 進行分級。

## <a name="installing-get-sddcdiagnosticinfo"></a>安裝 Get SDDCDiagnosticInfo

**Get SDDCDiagnosticInfo** PowerShell cmdlet （也稱為 **Get-PCStorageDiagnosticInfo**，先前稱為**測試 StorageHealth**) 可用來收集記錄檔和執行健康情況檢查容錯移轉叢集 （叢集、 資源、 網路、 節點） 的儲存空間 （實體磁碟機箱，虛擬磁碟）、 叢集共用磁碟區、 SMB 檔案共用和重複資料刪除。 

有兩種方法安裝指令碼，這兩者都是以下的外框。

### <a name="powershell-gallery"></a>PowerShell 資源庫

[PowerShell 資源庫](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo)是 GitHub 存放庫的快照集。 請注意，從 「 PowerShell 資源庫安裝項目需要最新版本的 PowerShellGet 模組，位於 Windows 10 中，Windows Management Framework (WMF) 5.0、 中或在 MSI 安裝程式 （適用於 PowerShell 3 和 4）。

您可以執行下列命令在 PowerShell 中使用系統管理員權限來安裝模組：

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

若要更新模組，請在 PowerShell 中執行下列命令：

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

[GitHub 存放庫](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/)是最新版本的模組，因為我們會持續地逐一查看這裡。 若要從 GitHub 安裝模組，下載最新的模組，從[封存](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip)並解壓縮到正確的 PowerShell 模組路徑所指向的 PrivateCloud.DiagnosticInfo 目錄 ```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
} 
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

``` 

如果您需要離線的叢集上取得此模組，請下載 zip 檔，將它移至您的目標伺服器節點，並安裝模組。

## <a name="gathering-logs"></a>正在蒐集的記錄檔

您已啟用事件通道，並完成安裝程序之後，您可以使用模組中取得 SDDCDiagnosticInfo PowerShell cmdlet 來取得：

- 儲存裝置健康情況，加上 狀況不良的元件的詳細資料上的報表
- 報告的儲存體容量集區、 磁碟區和重複資料刪除磁碟區
- 從所有叢集節點和摘要的錯誤報告的事件記錄檔

假設您的存放裝置叢集的名稱 *"CLUS01 」。*

若要執行此選項，針對遠端存放裝置叢集：

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

若要在本機執行，叢集的儲存體節點上：

``` PowerShell
Get-SDDCDiagnosticInfo
```

若要將結果儲存至指定的資料夾中：

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

外觀將實際的叢集上的範例如下：

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

如您所見，指令碼也會進行驗證，目前的叢集狀態

![資料收集 powershell 螢幕擷取畫面](media/data-collection/CollectData.png)

如您所見，所有資料都要都寫入 SDDCDiagTemp 資料夾

![在 檔案總管的螢幕擷取畫面中的資料](media/data-collection/CollectDataFolder.png)

指令碼將會完成之後，它會在您的使用者目錄中建立 ZIP

![powershell 螢幕擷取畫面中的資料壓縮](media/data-collection/CollectDataResult.png)

讓我們產生文字檔報表

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt
    
```

如需參考，以下是連結[範例報表](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt)並[範例 zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP)。

若要檢視 Windows Admin Center （及更新版本版本 1812年），請瀏覽至*診斷* 索引標籤。您會看到下面的螢幕擷取畫面中，您可以 

- 安裝診斷工具
- 更新 （如果它們是已過時）， 
- 排程每日的診斷執行 (它們有低影響您的系統上，通常會採用 < 5 分鐘在背景中，而且不會佔用您叢集上超過 500 mb)
- 檢視先前收集的診斷資訊，如果您需要提供支援，或自行分析它。

![wac 診斷螢幕擷取畫面](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>取得 SDDCDiagnosticInfo 輸出

以下是 Get SDDCDiagnosticInfo 壓縮輸出中包含的檔案。

### <a name="health-summary-report"></a>健康情況摘要報告

健全狀況摘要報表會儲存為：
- 0_CloudHealthSummary.log

剖析的所有資料收集，並只為了提供您的系統的快速摘要之後，會產生這個檔案。 它包含：

- 系統資訊
- 存放裝置健康情況概觀 （增加資源線上，叢集共用磁碟區上線的節點、 狀況不良的元件等的數目。）
- 狀況不良的元件 （叢集資源離線、 失敗或線上擱置） 的詳細資訊
- 韌體和驅動程式的資訊
- 集區、 實體磁碟和磁碟區詳細資料
- 儲存體效能 （效能計數器的收集）

此報表會持續更新為包含更有用的資訊。 如需最新資訊，請參閱[GitHub 讀我檔案](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md)。

### <a name="logs-and-xml-files"></a>記錄檔和 XML 檔案

指令碼會執行收集指令碼的各種記錄檔，並將輸出儲存為 xml 檔案。 我們會收集叢集及健康記錄、 系統資訊 (MSInfo32)、 未篩選的事件記錄檔 （容錯移轉叢集、 dis 診斷、 hyper-v、 儲存空間和更多） 和儲存體的診斷資訊 （作業記錄檔）。 如需所收集的資訊最新的資訊，請參閱[（什麼我們收集） 的 GitHub 讀我檔案](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include)。

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>如何使用 Get PCStorageDiagnosticInfo 的 XML 檔案
您可以取用從所收集的資料中提供的 XML 檔案資料**Get PCStorageDiagnosticInfo** cmdlet。 這些檔案包含資訊的虛擬磁碟、 實體磁碟、 叢集基本資訊和其他 PowerShell 的相關輸出。 

若要查看這些輸出結果，請開啟 PowerShell 視窗並執行下列步驟。 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>接下來會遇到什麼？
許多改善和新的 cmdlet，以分析 SDDC 系統健全狀況。
在您想要查看所提出的問題提供意見反應[此處](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues)。 此外，請隨意很有幫助變更提供給指令碼，提交[提取要求](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)。
