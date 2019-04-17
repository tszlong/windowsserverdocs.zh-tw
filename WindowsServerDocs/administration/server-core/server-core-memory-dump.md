---
title: 設定伺服器核心安裝的記憶體傾印檔案
description: 了解如何設定 Windows Server 的伺服器核心安裝的記憶體傾印檔案
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1447975"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>設定伺服器核心安裝的記憶體傾印檔案

>適用於：Windows Server (半年度管道) 和 Windows Server 2016

使用下列步驟來設定您的伺服器核心安裝的記憶體傾印。 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>步驟 1： 停用自動系統] 頁面上的檔案管理

第一個步驟是以手動方式設定您的系統失敗及復原選項。 這需要完成的其餘步驟。

執行下列命令： 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>步驟 2： 設定記憶體傾印的目的地路徑

您不需要具有分割區上的頁面檔案已安裝作業系統。 若要放在另一個分割區上的頁面檔案，您必須建立名為**DedicatedDumpFile**新的登錄項目。 您可以使用**DumpFileSize**登錄項目定義分頁檔案的大小。 若要建立的 DedicatedDumpFile 和 DumpFileSize 登錄項目，請遵循下列步驟： 

1. 在命令提示字元中執行**regedit**命令，以開啟 [登錄編輯程式。
2. 找出，然後按一下 [下列登錄子機碼： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. 按一下 [**編輯 > 新增 > 字串值**。
4. 命名新值**DedicatedDumpFile**，並按 ENTER。
5. 以滑鼠右鍵按一下**DedicatedDumpFile**，並再按一下 [**修改**]。
6. 在 [**數值資料**類型**\ < Drive\ >: \\\ < Dedicateddumpfile.sys\ >**，然後按一下 [**確定]**。

   >[!NOTE] 
   > 取代 \ < Drive\ > 與磁碟機具有足夠的磁碟空間分頁檔案，並取代 \ < Dedicateddumpfile.dmp\ > 專用檔案的完整路徑。
 
7. 按一下 [**編輯 > 新增 > DWORD 值**。
8. 輸入**DumpFileSize**，並按 ENTER。
9. 以滑鼠右鍵按一下**DumpFileSize**，並再按一下 [**修改**]。
10. 在 [**編輯 DWORD 值**] [**基底**，按一下 [**十進位**。
11. 在 [**數值資料**] 輸入適當的值，然後按一下 [**確定]**。
   >[!NOTE]
   > 傾印檔案的大小為 (mb)。
12. 結束登錄編輯程式。

決定記憶體傾印的分割區位置之後，設定] 頁面上檔案目的路徑。 若要檢視目前的目的路徑] 頁面上的檔案，請執行下列命令：

```
wmic RECOVEROS get DebugFilePath
```

**DebugFilePath**的預設目的地是 %systemroot%\memory.dmp。 若要變更目前的目的路徑，請執行下列命令：

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

設定 \ < FilePath\ > 目的地路徑。 例如，下列命令會將記憶體傾印目的路徑] 設定為 C:\WINDOWS\MEMORY。DMP： 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>步驟 3： 設定的記憶體傾印類型

決定如何設定您的伺服器的記憶體傾印的類型。 若要檢視目前的記憶體傾印類型，請執行下列命令：

```
wmic RECOVEROS get DebugInfoType
```

若要變更目前的記憶體傾印類型，請執行下列命令： 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\ < Value\ > 可以是 0、 1、 2 或 3、，如下所定義。

- 0： 停用記憶體傾印移除。
- 1： 完整記憶體傾印。 當您的電腦意外停止會記錄所有系統記憶體的內容。 完整的記憶體傾印可能包含資料時所收集的記憶體傾印所執行的程序。
- 2： 核心記憶體傾印 （預設值）。 僅記錄核心記憶體。 這會加速通話資訊記錄在記錄檔中的電腦意外停止時的程序。
- 3： 小記憶體傾印。 記錄組人數最少的有用的資訊可以協助識別您的電腦意外停止的原因。

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>步驟 4： 設定自動產生記憶體傾印之後重新啟動伺服器

根據預設，伺服器會自動重新啟動之後則會產生記憶體傾印。 若要檢視目前的設定，請執行下列命令：

```
wmic RECOVEROS get AutoReboot
```

如果**AutoReboot**的值為 TRUE，伺服器會自動重新啟動後產生記憶體傾印。 不需要設定與您可以繼續下一個步驟。

如果**AutoReboot**的值為 FALSE，伺服器將不自動重新啟動。 執行下列命令以變更值：

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>步驟 5： 設定伺服器覆寫現有的記憶體傾印檔案

根據預設，伺服器會覆寫現有的記憶體傾印檔案時建立一個新。 若要判斷現有的記憶體傾印檔案若已設定為覆寫該檔案，請執行下列命令：

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

如果值為 1，伺服器就會覆寫現有的記憶體傾印檔案。 不需要設定，以及您可以繼續下一個步驟。

如果值為 0，伺服器不會覆寫現有的記憶體傾印檔案。 執行下列命令以變更值： 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>步驟 6： 設定系統管理提醒

判斷是否適當系統管理通知與據此設定**SendAdminAlert** 。 若要檢視目前的值為 SendAdminAlert，執行下列命令：

```
wmic RECOVEROS get SendAdminAlert
```

SendAdminAlert 的可能值都是 TRUE 或 FALSE。 若要修改現有的 SendAdminAlert 值設為 true，執行下列命令： 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>步驟 7： 將記憶體傾印] 頁面上的檔案大小

若要檢查目前的頁面檔案設定，請執行下列命令之一：

   ```
   wmic.exe pagefile
   ```

   或

   ```
   wmic.exe pagefile list /format:list
   ```

例如，執行下列命令來設定初始及最大分頁檔案大小：

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>步驟 8： 設定伺服器產生手動記憶體傾印

您可以使用 PS/2 鍵盤手動產生記憶體傾印。 預設會停用此功能並不是可供通用序列匯流排 (USB) 鍵盤。

若要啟用手動記憶體傾印使用 PS/2 鍵盤，執行下列命令：

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

若要判斷功能啟用是否正確，請執行下列命令：

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

您必須重新啟動伺服器有變更才會生效。 您可以執行下列命令來重新啟動伺服器：

```
Shutdown / r / t 0
```

您可以產生與連線至您的伺服器時按下 SCROLL LOCK 鍵兩次按住右 CTRL 鍵 PS/2 鍵盤手動記憶體傾印。 這會使電腦 bug] 核取錯誤碼 0xE2。 重新啟動伺服器之後，您在步驟 2 中建立的目的路徑會顯示新的傾印檔案。

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>步驟 9： 確認正確要建立檔案的記憶體傾印

您可以使用 dumpchk.exe utlity 確認記憶體傾印檔案已正確建立。 Dumpchk.exe 公用程式未安裝的伺服器核心安裝選項，與因此您必須從具有桌面體驗的伺服器或 Windows 10 執行它。 此外，還必須安裝 Windows 產品的偵錯工具。  

Dumpchk.exe 公用程式可讓您使用您選擇的中型至另一部電腦傳輸記憶體傾印檔案從 Windows Server 2008 的伺服器核心安裝。

> [!WARNING]
> 所以請謹慎考慮傳輸方法和方法會要求的資源] 頁面上的檔案可以是非常大。
 

其他參考

如需使用記憶體傾印檔案的一般資訊，請參閱[Overview of windows 記憶體傾印檔選項。](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows)

如需專用傾印檔案的詳細資訊，請參閱[如何使用 DedicatedDeumpFile 登錄值來克服時擷取系統記憶體傾印的系統磁碟機的空間限制](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/)。



