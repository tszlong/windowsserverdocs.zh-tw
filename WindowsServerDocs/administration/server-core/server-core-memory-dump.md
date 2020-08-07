---
title: 設定 Server Core 安裝的記憶體傾印檔案
description: 瞭解如何為 Windows Server 的 Server Core 安裝設定記憶體傾印檔案
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: ee5786684c4f3a6c75c3b123b9d3ef9d32143949
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895887"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>設定 Server Core 安裝的記憶體傾印檔案

>適用于： Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道) 

使用下列步驟，為您的 Server Core 安裝設定記憶體傾印。

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>步驟1：停用自動系統分頁檔案管理

第一個步驟是手動設定系統失敗和修復選項。 這是完成其餘步驟的必要條件。

執行下列命令：

```
wmic computersystem set AutomaticManagedPagefile=False
```

## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>步驟2：設定記憶體傾印的目的地路徑

您不需要在安裝作業系統的磁碟分割上擁有分頁檔案。 若要將分頁檔放在另一個磁碟分割上，您必須建立名為**DedicatedDumpFile**的新登錄專案。 您可以使用**DumpFileSize**登錄專案來定義分頁檔案的大小。 若要建立 DedicatedDumpFile 和 DumpFileSize 登錄專案，請遵循下列步驟：

1. 在命令提示字元中，執行**regedit**命令以開啟 [登錄編輯程式]。
2. 找出並按一下下列登錄子機碼： HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\CrashControl
3. 按一下 [**編輯] > 新 > 字串值**。
4. 將新值命名為**DedicatedDumpFile**，然後按 enter。
5. 以滑鼠右鍵按一下 [ **DedicatedDumpFile**]，然後按一下 [**修改**]。
6. 在 [**數值資料**類型** \<Drive\> ： \\ \<Dedicateddumpfile.sys\> **] 中，按一下 **[確定]**。

   >[!NOTE]
   > 將取代為 \<Drive\> 具有足夠磁碟空間可供分頁檔使用的磁片磁碟機，並將取代為 \<Dedicateddumpfile.dmp\> 專用檔案的完整路徑。

7. 按一下 [**編輯] > 新 > DWORD 值**。
8. 輸入**DumpFileSize**，然後按 enter。
9. 以滑鼠右鍵按一下 [ **DumpFileSize**]，然後按一下 [**修改**]。
10. 在 [**編輯 DWORD 值**] 的 [**基底**] 底下，按一下 [**十進位**]。
11. 在 [**數值資料**] 中，輸入適當的值，然後按一下 **[確定]**。
    >[!NOTE]
    > 傾印檔案的大小為 mb (MB) 。
12. 結束 [登錄編輯程式]。

在您判斷記憶體傾印的分割區位置之後，請設定分頁檔的目的地路徑。 若要查看分頁檔案的目前目的地路徑，請執行下列命令：

```
wmic RECOVEROS get DebugFilePath
```

**DebugFilePath**的預設目的地為%systemroot%\memory.dmp。 若要變更目前的目的地路徑，請執行下列命令：

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

設定 \<FilePath\> 為目的地路徑。 例如，下列命令會將記憶體傾印目的地路徑設定為 C:\WINDOWS\MEMORY。DMP

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```

## <a name="step-3-set-the-type-of-memory-dump"></a>步驟3：設定記憶體傾印的類型

判斷要為您的伺服器設定的記憶體傾印類型。 若要查看目前的記憶體傾印類型，請執行下列命令：

```
wmic RECOVEROS get DebugInfoType
```

若要變更目前的記憶體傾印類型，請執行下列命令：

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Value\>可以是0、1、2或3，如下所定義。

- 0：停用記憶體傾印的移除。
- 1：完整的記憶體傾印。 當您的電腦意外停止時，記錄系統記憶體的所有內容。 完整記憶體傾印可能包含收集記憶體傾印時正在執行之進程中的資料。
- 2：核心記憶體傾印 (預設) 。 僅記錄核心記憶體。 這可加速當您的電腦意外停止時，將資訊記錄到記錄檔中的程式。
- 3：小型記憶體傾印。 記錄一組最小的實用資訊，可協助識別電腦意外停止的原因。

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>步驟4：將伺服器設定為在產生記憶體傾印之後自動重新開機

根據預設，伺服器會在產生記憶體傾印之後自動重新開機。 若要查看目前的設定，請執行下列命令：

```
wmic RECOVEROS get AutoReboot
```

如果**AutoReboot**的值為 TRUE，伺服器將會在產生記憶體傾印之後自動重新開機。 不需要進行任何設定，而且您可以繼續進行下一個步驟。

如果**AutoReboot**的值為 FALSE，伺服器就不會自動重新開機。 執行下列命令來變更值：

```
wmic RECOVEROS set AutoReboot = true
```

## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>步驟5：設定伺服器以覆寫現有的記憶體傾印檔案

根據預設，伺服器會在建立新檔案時覆寫現有的記憶體傾印檔案。 若要判斷現有的記憶體傾印檔案是否已設定為要覆寫，請執行下列命令：

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

如果值為1，則伺服器將會覆寫現有的記憶體傾印檔案。 不需要進行任何設定，您可以繼續進行下一個步驟。

如果值為0，伺服器將不會覆寫現有的記憶體傾印檔案。 執行下列命令來變更值：

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```

## <a name="step-6-set-an-administrative-alert"></a>步驟6：設定系統管理警示

判斷系統管理警示是否適當，並據此設定**SendAdminAlert** 。 若要查看 SendAdminAlert 的目前值，請執行下列命令：

```
wmic RECOVEROS get SendAdminAlert
```

SendAdminAlert 的可能值為 TRUE 或 FALSE。 若要將現有的 SendAdminAlert 值修改為 true，請執行下列命令：

```
wmic RECOVEROS set SendAdminAlert = true
```

## <a name="step-7-set-the-memory-dumps-page-file-size"></a>步驟7：設定記憶體傾印的分頁檔案大小

若要檢查目前的分頁檔案設定，請執行下列其中一個命令：

   ```
   wmic.exe pagefile
   ```

   或

   ```
   wmic.exe pagefile list /format:list
   ```

例如，執行下列命令來設定分頁檔的初始和最大大小：

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>步驟8：設定伺服器以產生手動記憶體傾印

您可以使用 PS/2 鍵盤手動產生記憶體傾印。 這項功能預設為停用，無法供通用序列匯流排 (USB) 鍵盤使用。

若要使用 PS/2 鍵盤來啟用手動記憶體傾印，請執行下列命令：

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

若要判斷是否已正確啟用此功能，請執行下列命令：

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

您必須重新開機伺服器，變更才會生效。 您可以執行下列命令來重新開機伺服器：

```
Shutdown / r / t 0
```

您可以藉由按住右 CTRL 鍵並同時按兩次滾動鎖定鍵，以連接到伺服器的 PS/2 鍵盤來產生手動記憶體傾印。 這會使電腦錯誤檢查，並出現錯誤碼0xE2。 重新開機伺服器之後，新的傾印檔案會出現在您于步驟2建立的目的地路徑中。

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>步驟9：確認已正確建立記憶體傾印檔案

您可以使用 dumpchk.exe utlity 來確認是否已正確建立記憶體傾印檔案。 dumpchk.exe 公用程式不會與 Server Core 安裝選項一起安裝，所以您必須從具有桌面體驗或 Windows 10 的伺服器執行它。 此外，您還必須安裝 Windows 產品的偵錯工具。

dumpchk.exe 公用程式可讓您使用您選擇的媒體，將記憶體傾印檔案從 Windows Server 2008 的 Server Core 安裝傳輸到另一部電腦。

> [!WARNING]
> 分頁檔案可能非常大，因此請仔細考慮傳送方法和方法所需的資源。


其他參考資料

如需使用記憶體傾印檔案的一般資訊，請參閱[適用于 Windows 的記憶體傾印檔選項總覽](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows)。

如需專用傾印檔案的詳細資訊，請參閱[如何使用 DedicatedDeumpFile 登錄值來克服系統磁片磁碟機上的空間限制，並同時捕捉系統記憶體轉儲](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/)。



