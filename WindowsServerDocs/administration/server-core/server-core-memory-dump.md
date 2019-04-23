---
title: 設定 Server Core 安裝的記憶體傾印檔案
description: 了解如何設定 Windows Server 的 Server Core 安裝的記憶體傾印檔案
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828839"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>設定 Server Core 安裝的記憶體傾印檔案

>適用於：Windows Server （半年通道） 和 Windows Server 2016

使用下列步驟設定您的 Server Core 安裝的記憶體傾印。 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>步驟 1：停用自動系統分頁檔案管理

第一個步驟是以手動方式設定您的系統失敗與復原選項。 如此才能完成其餘步驟。

執行下列命令： 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>步驟 2：設定的記憶體傾印的目的地路徑

您不必有分頁檔的磁碟分割上安裝作業系統。 若要讓另一個磁碟分割上的分頁檔，您必須建立新的登錄項目，名為**DedicatedDumpFile**。 您可以使用來定義分頁檔的大小**DumpFileSize**登錄項目。 若要建立 DedicatedDumpFile 和 DumpFileSize 登錄項目，請遵循下列步驟： 

1. 在命令提示字元中，執行**regedit**命令來開啟 登錄編輯程式。
2. 找到並按一下以下的登錄子機碼：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. 按一下 **編輯 > 新增 > 字串值**。
4. 新值命名**DedicatedDumpFile**，然後按 ENTER 鍵。
5. 以滑鼠右鍵按一下**DedicatedDumpFile**，然後按一下**修改**。
6. 在 **數值資料**型別**\<磁碟機\>:\\\<Dedicateddumpfile.sys\>**，然後按一下 **確定**.

   >[!NOTE] 
   > 取代\<磁碟機\>磁碟機具有足夠的磁碟空間讓分頁檔，並取代\<Dedicateddumpfile.dmp\>專用的檔案的完整路徑。
 
7. 按一下 **編輯 > 新增 > DWORD 值**。
8. 型別**DumpFileSize**，然後按 ENTER 鍵。
9. 以滑鼠右鍵按一下**DumpFileSize**，然後按一下**修改**。
10. 在 **編輯 DWORD 值**下方**基底**，按一下 **十進位**。
11. 在 **值的資料**，輸入適當的值，然後按一下**確定**。
   >[!NOTE]
   > 傾印檔案的大小是以 mb 為單位 (MB)。
12. 結束登錄編輯器。

判斷記憶體傾印的磁碟分割位置之後，設定分頁檔的目的地路徑。 若要檢視目前的分頁檔的目的地路徑，請執行下列命令：

```
wmic RECOVEROS get DebugFilePath
```

預設目的地**DebugFilePath**是 %systemroot%\memory.dmp。 若要變更目前目的地路徑，請執行下列命令：

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

設定\<FilePath\>為目的路徑。 例如，下列命令設定 C:\WINDOWS\MEMORY 記憶體傾印目的地路徑。DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>步驟 3：設定記憶體傾印的類型

判斷記憶體傾印，若要設定您的伺服器類型。 若要檢視目前的記憶體傾印類型，請執行下列命令：

```
wmic RECOVEROS get DebugInfoType
```

若要變更目前的記憶體傾印類型，請執行下列命令： 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<值\>可以是 0、 1、 2 或 3，定義如下。

- 0:停用記憶體傾印的移除。
- 1：完整記憶體傾印。 當您的電腦非預期地停止時，請記錄所有的系統記憶體的內容。 完整記憶體傾印可能包含從收集記憶體傾印時所執行的處理序的資料。
- 2：核心記憶體傾印 （預設值）。 僅記錄核心記憶體。 這樣會加快您的電腦非預期地停止時，記錄檔中記錄資訊的程序。
- 3：小型記憶體傾印。 記錄的最小的集合，實用的資訊可協助您找出您的電腦意外停止的原因。

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>步驟 4：將伺服器設定為自動產生記憶體傾印之後重新啟動

根據預設，伺服器會自動重新啟動之後就會產生記憶體傾印。 若要檢視目前的組態，請執行下列命令：

```
wmic RECOVEROS get AutoReboot
```

如果值**AutoReboot**為 TRUE 時，伺服器會自動重新啟動之後產生記憶體傾印。 不需要進行設定，您可以繼續進行下一個步驟。

如果值**AutoReboot**為 FALSE 時，伺服器會自動重新啟動。 執行下列命令以變更值：

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>步驟 5：將伺服器設定為覆寫現有的記憶體傾印檔案

根據預設，伺服器會覆寫現有的記憶體傾印檔案新，則會建立一個。 若要判斷是否現有的記憶體傾印檔案已設定為覆寫，請執行下列命令：

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

如果值為 1，伺服器將會覆寫現有的記憶體傾印檔案。 不需要任何設定，以及您可以繼續進行下一個步驟。

如果值為 0，則伺服器不會覆寫現有的記憶體傾印檔案。 執行下列命令以變更值： 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>步驟 6：設定系統管理警示

判斷是否適當，並設定系統管理警示**SendAdminAlert**據此。 若要檢視 SendAdminAlert 目前的值，執行下列命令：

```
wmic RECOVEROS get SendAdminAlert
```

SendAdminAlert 的可能值為 TRUE 或 FALSE。 若要修改現有 SendAdminAlert 值設為 true，執行下列命令： 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>步驟 7：設定記憶體傾印的分頁檔大小

若要檢查目前的分頁檔設定，執行下列命令之一：

   ```
   wmic.exe pagefile
   ```

   或

   ```
   wmic.exe pagefile list /format:list
   ```

例如，執行下列命令來設定分頁檔的初始最大大小：

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>步驟 8：將伺服器設定為產生手動記憶體傾印

您可以使用 ps/2 坫，手動產生記憶體傾印。 根據預設，已停用此功能並不是適用於通用序列匯流排 (USB) 鍵盤。

若要啟用手動的記憶體傾印使用 ps/2 坫，執行下列命令：

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

若要判斷功能是否已正確啟用，請執行下列命令：

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

您必須重新啟動伺服器，變更才會生效。 若要重新啟動伺服器，可以執行下列命令：

```
Shutdown / r / t 0
```

您可以產生與已連線至您的伺服器上，按住右 CTRL 鍵同時按 SCROLL LOCK 鍵兩次 ps/2 坫手動記憶體傾印。 這可讓電腦錯誤，錯誤碼 0xE2 的核取。 重新啟動伺服器之後，新的傾印檔案會出現在您在步驟 2 建立的目的地路徑中。

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>步驟 9：請確認該檔案會建立正確的記憶體傾印

您可以使用 dumpchk.exe utlity 以確認記憶體傾印檔案已正確建立。 Dumpchk.exe 公用程式未使用 Server Core 安裝選項時，安裝，因此您必須從具有桌面體驗的伺服器或 Windows 10，請執行它。 此外，您必須安裝適用於 Windows 產品的偵錯工具。  

Dumpchk.exe 公用程式可讓您使用的媒體，您選擇的另一台電腦傳送記憶體傾印檔案從 Windows Server 2008 的 Server Core 安裝。

> [!WARNING]
> 因此請謹慎考慮傳輸，方法和方法所需資源的分頁檔可以是非常大。
 

其他參考資料

如需使用的記憶體傾印檔案的一般資訊，請參閱[概觀記憶體傾印檔案的選項 Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows)。

如需專用的傾印檔案的詳細資訊，請參閱[如何使用 DedicatedDeumpFile 登錄值，以克服在系統磁碟機的空間限制擷取的系統記憶體傾印時](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/)。



