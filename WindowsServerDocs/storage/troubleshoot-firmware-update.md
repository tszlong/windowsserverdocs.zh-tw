---
ms.assetid: 13210461-1e92-48a1-91a2-c251957ba256
title: 對磁碟機韌體更新進行疑難排解
ms.prod: windows-server
ms.author: toklima
manager: masriniv
ms.technology: storage
ms.topic: article
author: toklima
ms.date: 04/18/2017
ms.openlocfilehash: b62fdfe64ea579f61239dc582c639fb10ec1371c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820881"
---
# <a name="troubleshooting-drive-firmware-updates"></a>對磁碟機韌體更新進行疑難排解

>適用於：Windows 10、Windows Server (半年度管道)、

Windows 10 版本 1703 和更新版本，以及 Windows Server (半年度管道) 包含可更新已使用韌體升級 AQ (其他辨識符號) 透過 PowerShell 取得認證之 HDD 和 SSD 韌體的功能。

您可以在下列位置找到更多有關此功能的資訊：

- [更新 Windows Server 2016 中的磁片磁碟機固件](update-firmware.md)
- [更新儲存空間直接存取的磁片磁碟機固件而不停機](https://channel9.msdn.com/Blogs/windowsserver/Update-Drive-Firmware-Without-Downtime-in-Storage-Spaces-Direct)

韌體更新可能會因為各種原因而失敗。 本文章的目的是要協助您進行進階疑難排解。

  > [!Note]
  > 視問題而定，本文中的資訊可能不足以完全偵錯所有可能的失敗情況。

## <a name="common-issues"></a>常見問題
從架構上來說，這項新功能需要依賴 Windows 儲存堆疊所實作的 API，PowerShell 會呼叫到這些 API。 儲存堆疊需要仰賴驅動程式和硬體，才能正確實作業界定義的命令。 這就產生了許多可能發生失敗的問題點。 最常發覺的問題：

1.  指定的磁碟機無法正確實作業界標準命令 (沒有 AQ)
2.  執行更新所需的 API 未實作或有錯誤 (如果使用協力廠商驅動程式)
3.  API 運作正常，但是韌體本身有問題 (映像無效/損毀、…)

下列各節根據使用的是 Microsoft 還是協力廠商的驅動程式，概述疑難排解資訊。

## <a name="identifying-inappropriate-hardware"></a>識別不適當的硬體
識別裝置是否支援正確命令集最快速方法就是，直接啟動 PowerShell，並將磁碟的象徵 PhysicalDisk 物件傳遞至 Get-StorageFirmwareInfo Cmdlet。 以下是範例：

```powershell
Get-PhysicalDisk -SerialNumber 15140F55976D | Get-StorageFirmwareInformation
```
輸出範例如下︰

```
PhysicalDisk          : MSFT_PhysicalDisk (ObjectId = "{1}\\TOKLIMA-DL380\root/Microsoft/Windo...)
SupportsUpdate        : True
NumberOfSlots         : 1
ActiveSlotNumber      : 0
SlotNumber            : {0}
IsSlotWritable        : {True}
FirmwareVersionInSlot : {0013}
```

SupportsUpdate 欄位 (至少 SATA 和 NVMe 裝置的這個欄位) 將會指出內建 PowerShell 功能是否可以用來升級韌體。

無法使用業界標準命令查詢適當命令支援時，SupportsUpdate 欄位對於 SAS 連結裝置永遠回報 "True"。

要驗證 SAS 裝置是否支援所需的命令集，有兩個方式可以選擇︰
1.  透過 Update-StorageFirmware Cmdlet，以適當的韌體映像來試試看，或者
2.  請參閱 Windows Server 目錄，以識別哪些 SAS 裝置已成功取得 FW 更新 AQ （ https://www.windowsservercatalog.com/)

### <a name="remediation-options"></a>修復選項
如果您測試中的特定裝置不支援適當命令集，請向廠商查詢以了解是否有更新的韌體提供所需的命令集，或是查閱 Windows Server 目錄以確認裝置是否有實作適當命令集的來源。

## <a name="troubleshooting-with-3rd-party-drivers-sas"></a>協力廠商驅動程式 (SAS) 疑難排解
與硬體最密切互動的軟體元件是 Windows 儲存堆疊中的 Mini-Port 驅動程式。 對於一些像 SATA 及 NVMe 這樣的存放裝置通訊協定，Microsoft 會提供原生 Windows 驅動程式。 這些驅動程式可以提供額外的偵錯資訊。 不過，第三方硬體和軟體廠商可以自行撰寫他們裝置的 Miniport 驅動程式，但其偵錯資訊支援層級可能會有所不同。

若要在不考慮 Miniport 驅動程式的情況下，確認韌體下載發生的狀況，並啟用儲存堆疊中由上向傳送的 API，請參閱下列事件記錄檔通道：

事件檢視器 - 應用程式及服務記錄檔 - Microsoft - Windows - StorDiag - **Microsoft-Windows-存放裝置-ClassPnP/操作**

此通道記錄有關向下傳送至 Miniport 驅動程式之 Windows API 及其回應的資訊。 例如，嘗試將韌體映像下載到透過未正確實作由 SAS 至 SATA 所需轉譯之 SAS HBA 連接的 SATA 裝置時，會顯示下面出現的錯誤條件：

```powershell
Get-PhysicalDisk -SerialNumber 44GS103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.enc -SlotNumber 0
```
以下是輸出的範例：

```
Update-StorageFirmware : Failed

Extended information:
A warning or error has been encountered during storage firmware update.
Incorrect function.

Activity ID: {1224482b-2315-4a38-81eb-27bb7de19c00}
At line:1 char:47
+ ... S103UT5EW | Update-StorageFirmware -ImagePath C:\Firmware\J3E160@3.en ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Update-StorageFirmware], CimException
+ FullyQualifiedErrorId : StorageWMI 4,Microsoft.Management.Infrastructure.CimCmdlets.InvokeCimMethodCommand,Update-StorageFirmware
```
    
PowerShell 會擲回錯誤，並且收到表示所呼叫之功能 (亦即核心 API) 不正確的資訊。 這可能表示 API 不是由協力廠商 SAS Mini-Port 埠驅動程式所實作 (本案例即是如此)，或是 API 因其他緣故 (例如下載區段未對齊) 而失敗。

```
EventData
DeviceGUID  {132EDB55-6BAC-A3A0-C2D5-203C7551D700}
DeviceNumber    1
Vendor  ATA 
Model   TOSHIBA THNSNJ12
FirmwareVersion 6101
SerialNumber    44GS103UT5EW
DownLevelIrpStatus  0xc0000185
SrbStatus   132
ScsiStatus  2
SenseKey    5
AdditionalSenseCode 36
AdditionalSenseCodeQualifier    0
CdbByteCount    10
CdbBytes    3B0E0000000001000000
NumberOfRetriesDone 0
```

來自通道的 ETW 事件507顯示 SCSI SRB 要求失敗，並提供 SenseKey 為 ' 5 ' （不合法的要求）的其他資訊，而且 AdditionalSense 資訊是 ' 36 ' （CDB 中不合法的欄位）。

   > [!Note]
   > 此資訊是由前述 Miniport 提供，而該資訊的正確性取決於 Miniport 驅動程式的實作和複雜性。

如果 Miniport 驅動程式沒有釐清錯誤碼之間的分別，不同的錯誤條件有可能會顯示相同的錯誤碼。 例如，嘗試透過 SAS HBA 將無效的韌體映像下載到 SATA 裝置 (預期此裝置會失敗) 可能會轉譯成相同的失敗碼。

在通訊協定為混合式且發生轉譯 (亦即 SATA 後置 SAS) 的情況下，最好要測試直接與 SATA 控制連接的 SATA 裝置，以排除其有潛在問題。

### <a name="remediation-options"></a>修復選項
如果協力廠商驅動程式經辨識為無法實作所需的 API 或轉譯，則可以swap to the Microsoft 提供的 SATA (StorAHCI.sys) 和 NVMe (StorNVMe.sys) 替代驅動程式，或是連絡提供 SAS 驅動程式的 OEM 或 HBA 廠商，並查詢是否有提供正確支援的較新版本。

## <a name="additional-troubleshooting-with-microsoft-drivers-satanvme"></a>Microsoft 驅動程式 (SATA/NVMe) 其他疑難排解
使用 Windows 原生驅動程式 (例如 StorAHCI.sys 或 StorNVMe.sys) 來支援存放裝置時，可以在韌體更新作業期間取得有關可能失敗情況的其他資訊。

除了 ClassPnP 操作通道之外，StorAHCI 和 StorNVMe 會在下列 ETW 通道中記錄裝置的通訊協定特定傳回碼：

事件檢視器 - 應用程式及服務記錄檔 - Microsoft - Windows - StorDiag - **Microsoft-Windows-存放裝置-StorPort/診斷**

預設不會顯示診斷記錄檔，但可以按一下事件檢視器中的 [檢視]，並從下拉式功能表選取 [顯示分析與偵錯記錄] 來啟用/顯示。

若要收集這些進階記錄檔項目，請啟用記錄檔、重現韌體更新失敗，並儲存診斷記錄檔。

以下是有關 SATA 裝置因下載映像無效 (事件識別碼：258) 而失敗的韌體更新範例：

``` 
EventData
MiniportName    storahci
MiniportEventId 19
MiniportEventDescription    Firmware Activate Completion
PortNumber  0
Bus 2
Target  0
LUN 0
Irp 0xffff8c84cd45aca0
Srb 0xffffab0024030bc0
Parameter1Name  SrbStatus
Parameter1Value 130
Parameter2Name  ReturnCode
Parameter2Value 0
Parameter3Name  FeaturesReg
Parameter3Value 15
Parameter4Name  SectorCountReg
Parameter4Value 0
Parameter5Name  DriveHeadReg
Parameter5Value 160
Parameter6Name  CommandReg
Parameter6Value 146
Parameter7Name  NULL
Parameter7Value 0
Parameter8Name  NULL
Parameter8Value 0
```

上述事件的參數值 2 至 6 包含詳細的裝置資訊。 我們在這裡看到了各種 ATA 暫存器值。 ATA ACS 規格可以用來解碼下面的下載微碼命令失敗值：
- 傳回碼：0 (0000 0000) (不適用 - 無意義，因為沒有傳輸任何承載)
- 功能：15（0000 1111）（Bit 1 已設定為 ' 1 '，表示「中止」）
- 磁區計數：0 (0000 0000) (不適用)
- 磁碟機磁頭：160 (1010 0000) (不適用 – 只有過時的位元已設定)
- 命令：146（1001 0010）（Bit 1 設定為 ' 1 '，表示有意義資料的可用性）

這就是說，韌體更新作業已由裝置中止。

搭配 Windows 原生 NVMe 驅動程式 (StorNVMe.sys) 使用 NVMe 裝置時，此通道提供類似的偵錯資訊層級。
