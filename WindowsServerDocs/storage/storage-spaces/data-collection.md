---
title: 使用儲存空間直接存取收集診斷資料
description: 瞭解儲存空間直接存取資料收集工具，以及如何執行和使用它們的特定範例。
keywords: 儲存空間、資料收集、疑難排解、事件通道、SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 67f35e3afa8e9eafabe7b22eb60cc85c7be6cb23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402879"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>使用儲存空間直接存取收集診斷資料

> 適用於：Windows Server 2019、Windows Server 2016

您可以使用各種診斷工具來收集疑難排解儲存空間直接存取和容錯移轉叢集所需的資料。 在本文中，我們將著重于**SDDCDiagnosticInfo** -一種觸控工具，它會收集所有相關資訊，以協助您診斷叢集。

假設**SDDCDiagnosticInfo**的記錄和其他資訊很密集，下列有關疑難排解的資訊將有助於疑難排解已升級的 advanced 問題，而且可能需要將資料傳送至Microsoft 進行分級。

## <a name="installing-get-sddcdiagnosticinfo"></a>安裝 SDDCDiagnosticInfo

**SDDCDiagnosticInfo** PowerShell Cmdlet （也稱為 **PCStorageDiagnosticInfo**（先前稱為**測試 StorageHealth**）可用來收集和執行容錯移轉叢集的健康狀態檢查（叢集、資源、網路、節點）、儲存空間（實體磁片、主機殼、虛擬磁片）、叢集共用磁片區、SMB 檔案共用和重復資料刪除。 

有兩種方法可以安裝腳本，這兩者都是下面的概述。

### <a name="powershell-gallery"></a>PowerShell 資源庫

[PowerShell 資源庫](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo)是 GitHub 存放庫的快照集。 請注意，從 PowerShell 資源庫安裝專案需要最新版本的 PowerShellGet 模組，其適用于 Windows 10、Windows Management Framework （WMF）5.0，或以 MSI 為基礎的安裝程式（適用于 PowerShell 3 和4）。

您可以在 PowerShell 中以系統管理員許可權執行下列命令來安裝模組：

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

[GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/)存放庫是模組的最新版本，因為我們會持續在此反復查看。 若要從 GitHub 安裝模組，請從封存中下載最[新的模組，並將](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip)PrivateCloud. DiagnosticInfo 目錄解壓縮至 ```$env:PSModulePath``` 所指向的正確 PowerShell 模組路徑

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

如果您需要在離線叢集上取得此模組，請下載 zip、將它移至您的目標伺服器節點，然後安裝模組。

## <a name="gathering-logs"></a>正在收集記錄檔

當您啟用事件通道並完成安裝程式之後，您可以在模組中使用 SDDCDiagnosticInfo PowerShell Cmdlet 來取得：

- 報告儲存體健全狀況，並詳細說明狀況不良的元件
- 依集區、磁片區及重復資料刪除磁片區的儲存容量報告
- 所有叢集節點的事件記錄檔和摘要錯誤報表

假設您的存放裝置叢集名稱是 *"CLUS01"。*

若要對遠端存放裝置叢集執行：

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

若要在本機叢集存放裝置節點上執行：

``` PowerShell
Get-SDDCDiagnosticInfo
```

若要將結果儲存至指定的資料夾：

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

以下是此查詢在真實叢集上的外觀範例：

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

如您所見，腳本也會驗證目前的叢集狀態

![資料收集 powershell 螢幕擷取畫面](media/data-collection/CollectData.png)

如您所見，所有資料都會寫入至 SDDCDiagTemp 資料夾

![檔案瀏覽器螢幕擷取畫面中的資料](media/data-collection/CollectDataFolder.png)

腳本完成後，它會在您的使用者目錄中建立 ZIP

![powershell 中的資料 zip 螢幕擷取畫面](media/data-collection/CollectDataResult.png)

讓我們在文字檔中產生報表

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

以下是[範例報表](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt)和[範例 zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP)的連結，供您參考。

若要在 Windows 系統管理中心（版本1812）中查看此功能，請流覽至 [*診斷*] 索引標籤。如下列螢幕擷取畫面所示，您可以 

- 安裝診斷工具
- 更新它們（如果已過期）。 
- 排定每日診斷執行（這些會對您的系統造成較低的影響，通常會在背景中花 < 5 分鐘，而不會佔用叢集上的500mb）
- 如果您需要讓它自行支援或分析，請參閱先前收集的診斷資訊。

![wac 診斷螢幕擷取畫面](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>SDDCDiagnosticInfo 輸出

下列是 SDDCDiagnosticInfo 的壓縮輸出中所包含的檔案。

### <a name="health-summary-report"></a>健康情況摘要報告

健康情況摘要報告會儲存為：
- 0_CloudHealthSummary .log

這個檔案是在剖析所有收集到的資料之後產生的，而且是為了提供系統的快速摘要。 其中包含：

- 系統資訊
- 存放裝置健全狀況總覽（節點數目、線上資源、叢集共用磁片區上線、狀況不良的元件等）
- 狀況不良元件（離線、失敗或線上擱置中的叢集資源）的詳細資料
- 固件和驅動程式資訊
- 集區、實體磁片和磁片區詳細資料
- 儲存體效能（收集效能計數器）

此報表會持續更新以包含更多有用的資訊。 如需最新資訊，請參閱[GITHUB 讀我檔案](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md)。

### <a name="logs-and-xml-files"></a>記錄檔和 XML 檔案

腳本會執行各種記錄收集腳本，並將輸出儲存為 xml 檔案。 我們會收集叢集和健全狀況記錄、系統資訊（MSInfo32）、未篩選的事件記錄檔（容錯移轉叢集、未通過診斷、hyper-v、儲存空間等），以及儲存體診斷資訊（操作記錄）。 如需所收集之資訊的最新資訊，請參閱[GITHUB 讀我檔案（我們所收集的內容）](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include)。

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>如何使用 PCStorageDiagnosticInfo 的 XML 檔案
您可以使用**PCStorageDiagnosticInfo**指令程式所收集資料中所提供的 XML 檔案中的資料。 這些檔案包含虛擬磁片、實體磁片、基本叢集資訊和其他 PowerShell 相關輸出的資訊。 

若要查看這些輸出的結果，請開啟 PowerShell 視窗，然後執行下列步驟。 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>接下來該怎麼做？
許多改良功能和新的 Cmdlet 可分析 SDDC 系統健全狀況。
請在[這裡](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues)提出問題，以提供您想要查看之內容的意見反應。 此外，您也可以藉由提交[提取要求](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)來提供對腳本的實用變更。
