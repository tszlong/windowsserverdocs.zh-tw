---
title: Server Performance Advisor 套件開發指南
description: Server Performance Advisor 套件開發指南
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c27dd0602c5993fd84e6956c2f50f6e2bfec8691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435469"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Server Performance Advisor 套件開發指南

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中，Windows 8、 Windows 10

此開發指南 Microsoft Server Performance Advisor (SPA) 的指導方針可協助開發人員和開發 advisor 組件的系統管理員，來分析伺服器的效能。

它會假設您熟悉以效能記錄及警示 (PLA)、 效能計數器、 登錄設定、 Windows Management Instrumentation (WMI)、 事件追蹤的 Windows (ETW) 和 Transact SQL (T-SQL)。

如需使用 SPA 的詳細資訊，請參閱 < [Server Performance Advisor 使用者指南](server-performance-advisor-users-guide.md)。

## <a name="spa-advisor-pack-overview"></a>SPA Advisor Pack 概觀


Advisor 套件通常專為特定伺服器角色，而它會定義下列：

* 透過 PLA，包括 Windows Management Instrumentation (WMI)、 效能計數器、 登錄設定、 檔案和事件追蹤的 Windows (ETW) 收集資料

* 顯示警示和建議的規則

* （已收集的未經處理資料、 彙總的資料或前 10 名的清單） 顯示資料

* 若要檢視一段時間變更的值的統計資料

* 可以進行趨勢分析的統計資料值

Advisor 套件包含下列元素：

* **XML 中繼資料**(ProvisionMetadata.xml)

    * [效能記錄檔及警示 (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)資料收集器集合工具

    * 報表配置

* **SQL 指令碼**

    * 主要的預存程序

    * SQL 物件，例如預存程序和使用者定義函式

* **ETW 結構描述檔案**(Schema.man) 這是選擇性的

### <a name="advisor-pack-workflow"></a>Advisor 組件工作流程

![Advisor 組件工作流程](../media/server-performance-advisor/spa-dev-guide-workflow.png)

此流程圖中，綠色的圓形代表 advisor 組件。 所有其他的圓形代表 SPA 架構的過程中執行的階段。 SPA 會使用 advisor 套件來收集資料、 將資料匯入資料庫、 執行環境初始化和執行 SQL 指令碼。

### <a name="collect-data"></a>收集資料

Advisor 套件已排入佇列的特定伺服器來使用 SPA，當資料集合模組會查詢資料收集器集合 XML 從 advisor 組件，並會從目標伺服器收集資料。 未經處理的資料會儲存在使用者指定的檔案共用。 直到已超過執行持續期間由使用者指定的 SPA，不會停止資料收集。

### <a name="import-data-into-the-database"></a>資料匯入資料庫

資料收集完成之後，每種類型的資料會匯入 SQL Server 資料庫中對應的資料表中。 例如，將登錄設定匯入資料表時呼叫\#registryKeys。

檔案匯入 ETW 解碼.etl 檔案需要的 ETW 結構描述檔案。 ETW 結構描述檔案是 XML 檔案。 它可以產生使用 tracerpt.exe，這是隨附於 Windows。 ETW 結構描述檔案，才需要 ETW 資料匯入 advisor 套件時才需要。

### <a name="switch-to-low-user-rights"></a>切換至低的使用者權限

SPA 架構會自動調整以降低所需的安全性存取層級的權限。 因為可以開發或任何人修改 advisor 組件，它可能會包含遭竄改的 SQL 指令碼的建議程式組件。 若要減輕安全性風險，應該具有低的使用者權限執行 advisor 組件的任何 SQL 指令碼。 它只能存取有限的資料庫物件，例如暫存資料表和 SPA Api 公開為預存程序。 Advisor 組件中的 SQL 指令碼可以呼叫這些預存程序與 SPA 架構進行互動。

### <a name="initialize-execution-environment"></a>初始化執行環境

Advisor 組件可以產生不同類型的輸出，例如通知 」、 「 建議 」、 「 事實資料表 」、 「 統計資料和 「 統計資料的圖表。 SQL 指令碼會執行某些計算，收集的資料。 產生的結果會儲存在暫存資料表透過 SPA 公用 Api。 在初始化階段中，這些暫存資料表和其他系統資源必須將他們佈建。

### <a name="run-sql-scripts"></a>執行 SQL 指令碼

沒有主要預存程序，也稱為 advisor 套件開發人員。 SPA 架構會呼叫此預存程序起始計算。 預存程序會取用收集的資料，並進行通訊以 SPA 架構的最終結果。

### <a name="switch-to-administrative-rights"></a>切換至 系統管理權限

系統管理員權限，才能產生報告。 報告產生完全是由 SPA 控制。 就比較不容易遭到竄改。

### <a name="generate-a-report"></a>產生報告

Advisor 組件的主要預存程序完成之前，不會保存所有導出的結果，例如通知和統計資料。 在這個階段，SPA 架構會將最終的結果，從暫存資料表中以特定格式的資料表。 完成此階段之後，您可以使用 SPA 主控台來檢視報表。

## <a name="authoring-an-advisor-pack"></a>編寫 advisor 套件


### <a name="quick-guidelines"></a>快速的指導方針

下列流程圖說明地開發功能完整的建議程式組件的步驟。 本節也包含深入說明每個步驟的逐步說明範例。

![advisor 套件開發程序](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Advisor 套件通常是結構化方式如下：

Advisor 套件

ProvisionMetadata.xml

指令碼

main.sql

func.sql

Schema.man

每個 advisor 組件必須有一個稱為 ProvisionMetadata.xml 檔案。 它會定義基本 advisor 組件資訊，要通知和規則，以及報表需要的方式儲存並顯示收集的資料。 SPA 架構會使用此資訊來產生暫存資料表，然後將暫存資料表中的結果傳送至使用者可以存取的資料表。

所有報告的 SQL 指令碼必須儲存在子資料夾**指令碼**。 為了進行維護，我們建議您在不同的 SQL Server 檔案中儲存不同的資料庫物件。 做為主要進入點，必須要有至少一個預存程序。

> [!NOTE]
> Schema.man 檔案並非必要，除非您的 advisor 組件會收集 ETW 追蹤。 此結構描述檔案用來描述 ETW 事件的結構描述，以及為 ETW 事件解碼。

### <a name="defining-basic-information"></a>定義基本資訊

本節說明一些基本項目組成的 advisor 組件，包括 ProvisionMetadata.xml 和屬性。

以下是範例標頭 ProvisionMetadata.xml 檔案：

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Advisor pack 版本

屬性名稱：**版本**

Advisor 套件開發人員可以定義 advisor 組件的主要和次要版本：

* 主要的版本通常牽涉到明顯的改進。 可能不相容於新舊版本所產生的結果。 我們強烈建議在 advisor 組件名稱中包括的主要版本。

* 只需微幅變更，沒有任何資料不相容性問題時，SPA 可讓次要版本升級。

如需版本控制的詳細資訊，請參閱[進階主題](#bkmk-advancedtopics)。

### <a name="script-entry-point"></a>指令碼進入點

屬性名稱： **reportScript**

SPA 架構會從指令碼進入點中尋找主要預存程序名稱，並執行它以安全的方式。

### <a name="other-attributes"></a>其他屬性

以下是一些可用來識別 advisor 組件的其他屬性：

* 顯示名稱： **displayName**

* 描述：**描述**

* 作者：**作者**

* Framework 版本： **frameworkversion** （預設為 3.0）

* 最低作業系統版本： **minOSversion** （這保留供未來擴充性）

* 遺失事件通知： **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>定義資料收集器集合工具

資料收集器集合工具定義 SPA 架構應從目標伺服器收集的效能資料。 它支援登錄設定、 WMI、 效能計數器，從目標伺服器和 ETW 檔案。

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

**工期**屬性 **&lt;dataCollectorSet /&gt;** 在上述範例中定義的資料收集 （的時間單位為秒） 的持續時間。 **持續時間**是必要的屬性。 此設定會控制由效能計數器和 ETW 集合持續時間。

### <a name="collect-registry-data"></a>收集登錄資料

您可以從下列登錄區，以收集登錄資料：

* HKEY\_類別\_根

* HKEY\_CURrenT\_CONFIG

* HKEY\_CURrenT\_USER

* HKEY\_本機\_機器

* HKEY\_使用者

若要收集的登錄設定，指定的值名稱的完整路徑：HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

若要收集的所有登錄機碼下設定，指定登錄機碼的完整路徑：HKEY\_LOCAL\_MACHINE\\MyKey\\

若要收集登錄機碼和 （PLA 以遞迴方式收集登錄資料） 及其子機碼下的所有值，請使用兩個反斜線的最後一個路徑分隔符號：HKEY\_LOCAL\_MACHINE\\MyKey\\\\

若要從遠端電腦收集的登錄資訊，包括在登錄路徑開頭的電腦名稱：HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

例如，您可能會出現，如下所示的登錄機碼：

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

範例 1：傳回只 active PowerSchemes 和它們的值：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

範例 2：傳回此路徑下的所有機碼值組：

> [!NOTE]
> 使用者的認證下執行 PLA。 某些登錄機碼需要系統管理認證。 無法存取任何子機碼時，就會停止列舉型別。

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

所有收集的資料將匯入至暫存資料表，稱為 **\#registryKeys** SQL 報表之前執行指令碼。 下表顯示結果，例如 2:

KeyName | KeytypeId | 值
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | 電源最佳化
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

結構描述 **#registryKeys**資料表是做為如下所示：

資料行名稱 | SQL 資料類型 | 描述
-------- | -------- | --------
KeyName | Nvarchar(300 不是 NULL | 登錄機碼的完整路徑名稱
KeytypeId | smallint 非 NULL | 內部類型識別碼
值 | Nvarchar(4000) 不是 NULL | 所有的值

**KeytypeID**資料行可以具有下列類型之一：

識別碼 | 類型
--- | ---
1 | 字串
2 | expandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | 連結
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | Qword

### <a name="collect-wmi"></a>收集 WMI

您可以新增任何 WMI 查詢。 如需撰寫 WMI 查詢的詳細資訊，請參閱[WQL (SQL for WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx)。下列範例會查詢頁面檔案位置：

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

上述範例中的查詢會傳回一筆記錄：

標題 | 名稱 | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

由於收集的資料匯入資料庫時，WMI 就會傳回不同的資料行的資料表，SPA 執行資料正規化，並加入至下列資料表：

**\#WMIObjects 資料表**

SequenceID | 命名空間 | ClassName | Relativepath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\pagefile.sys | 1

**\#WmiObjectsProperties 資料表**

識別碼 | 查詢
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#WmiQueries 資料表**

識別碼 | 查詢
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#WmiObjects 資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceId | int NOT NULL | 相互關聯的資料列和其屬性
命名空間 | Nvarchar(200 不是 NULL | WMI 命名空間
ClassName | Nvarchar(200 不是 NULL | WMI 類別名稱
Relativepath | Nvarchar(500) 不是 NULL | WMI 的相對路徑
WmiqueryId | int NOT NULL | 相互關聯 #WmiQueries 的索引鍵

**\#WmiObjectProperties 資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceId | int NOT NULL | 相互關聯的資料列和其屬性
名稱 | Nvarchar(1000) 不是 NULL | 內容名稱
值 | Nvarchar(4000) NULL | 目前屬性的值

**\#WmiQueries 資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
Id | int NOT NULL | > 唯一查詢識別碼
查詢 | Nvarchar(4000) 不是 NULL | 在佈建中繼資料中的原始查詢字串

### <a name="collect-performance-counters"></a>收集效能計數器

以下範例如何收集效能計數器：

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

**間隔**屬性是必要的全域設定，所有的效能計數器。 它會定義 （的時間單位為秒） 的間隔收集效能資料。

在上述範例中，計數器\\PhysicalDisk (\*)\\avg.Disk sec/Transfer 將會查詢每秒。

可能有兩個執行個體： **\_總**和**0 c:D:** ，並輸出可能如下：

timestamp | CategoryName | CounterName | _Total 執行個體值 | 執行個體值 0 的 c:D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Avg.Disk sec/Transfer | 0.000933297607934224 | 0.000933297607934224

若要將資料匯入資料庫中，資料將會正規化名的資料表 **\#PerformanceCounters**。

CategoryDisplayName | InstanceName | CounterdisplayName | 值
---- | ---- | ---- | ----
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.000933297607934224
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.000933297607934224

**附註**當地語系化名稱，例如**CategoryDisplayName**並**CounterdisplayName**，會根據在目標伺服器上使用的顯示語言而有所不同。 請避免使用這些欄位，如果您想要建立的非語言相關 advisor 組件。

**\#PerformanceCounters**資料表結構描述

資料行名稱 | SQL 資料類型 | 描述
---- | ---- | ---- | ----
timestamp | datetime2(3) NOT NULL | UNC 中收集的日期時間
CategoryName | Nvarchar(200 不是 NULL | 類別名稱
CategoryDisplayName | Nvarchar(200 不是 NULL | 當地語系化類別目錄名稱
InstanceName | Nvarchar(200) NULL | 執行個體名稱
CounterName | Nvarchar(200 不是 NULL | 計數器名稱
CounterdisplayName | Nvarchar(200 不是 NULL | 當地語系化的計數器名稱
值 | 浮點數不是 NULL | 收集的值

### <a name="collect-files"></a>收集檔案

路徑可以是絕對或相對。 檔案名稱可以包含萬用字元 (\*) 和問號 （？）。 例如，若要收集的暫存資料夾中的所有檔案，您可以指定 c:\\temp\\\*。 萬用字元會套用至指定的資料夾中的檔案。

如果您想要同時從指定的資料夾的子資料夾中收集檔案，使用兩個反斜線的最後一個資料夾分隔符號，例如 c:\\temp\\\\\*。

以下範例會查詢**applicationHost.config**檔案：

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

可以呼叫的資料表中找到結果 **\#檔案**，例如：

querypath | fullpath | Parentpath | FileName | 內容
----- | ----- | ----- | ----- | -----
%windir%\...\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#檔案資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
---- | ---- | ----
querypath | Nvarchar(300 不是 NULL | 原始的查詢陳述式
fullpath | Nvarchar(300 不是 NULL | 絕對檔案路徑和檔案名稱
Parentpath | Nvarchar(300 不是 NULL | 檔案路徑
FileName | Nvarchar(300 不是 NULL | 檔案名稱
內容 | Varbinary （max) NULL | 二進位檔中的檔案內容

### <a name="defining-rules"></a>定義規則

從目標伺服器使用 PLA 收集足夠的資料之後，advisor 套件可以使用這項資料進行驗證，並顯示快速摘要給系統管理員。

規則快速介紹有關伺服器效能。 它們會反白顯示問題，並提供建議。 您可以列出您想要驗證的 advisor 組件的所有規則。 例如，如果您想要開發的核心作業系統 advisor 組件，可能包括可能的規則：

* CPU 電源模式是否正在 power 儲存

* 伺服器是否在虛擬環境

* 是否有磁碟 I/O 壓力

規則包含下列項目：

* 相依的臨界值 （可設定規則的一部分）

* （「 警示 」 和 「 建議 」） 的規則定義

以下簡單規則的範例：

``` syntax
<advisorPack>

  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>Threshold

臨界值是可設定的因素，可讓系統管理員，來決定當規則應該會顯示一個良好或不良的狀態。 下列範例顯示小於 10 GB 的可用空間時，偵測上的系統磁碟機和警告的可用空間的規則。

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

不過，在此情況下，系統管理員具有較小的硬碟。 他認為 5 GB 的可用空間可能仍會是狀況良好，以及他不想要看到一則警告。 他可以更新透過 SPA 主控台 5 到 10 的預設值，而不需要了解如何開發的建議程式組件。

導入臨界值，可協助快速變更的值，而不需要修改 advisor 套件的系統管理員。

在範例中，所有屬性，除了**描述**所需。 您可以使用的任何數字**值**。

在規則可以共用臨界值。

### <a name="alerts-and-recommendations"></a>警示和建議

規則定義不包含任何邏輯計算。 它會定義 UI 的可能外觀和 SQL Server 報告指令碼的方式進行通訊的 ui 的結果。

規則有三個部分：

* 警示 （規則標題）

* 建議 （建議）

* 相關聯的臨界值 （選擇性相依性的相關資訊）

規則的範例如下：

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

您可以定義您想，以及您通常會定義建議盡可能建議。 **層級**建議可以**成功**或是**警告**。

您可以連結至您想要數目的臨界值。 您甚至可以連結至不會影響目前的規則的臨界值。 連結有助於 SPA 主控台輕鬆地管理 臨界值。

規則名稱 」 和 「 建議索引鍵，且都是唯一的範圍。 任何兩個規則都可以有相同的名稱，和一個規則中的沒有任何兩個建議都可以有相同的名稱。 當您撰寫 SQL 指令碼的報表時，這些名稱會是非常重要。 您可以呼叫\[dbo\]。\[SetNotification\] API 來設定規則狀態。

### <a name="defining-ui-display-elements"></a>定義 UI 顯示項目

定義規則之後，系統管理員可以看到報表的摘要。 不過，通常系統管理員有興趣的彙總的資料，而且他們想要檢查資料來源中的效能規則所使用。

延續上一個範例，使用者就會知道是否有足夠的可用磁碟空間系統磁碟機上。 使用者也可能感興趣的可用空間的實際大小。 單一值群組用來儲存及顯示這類的結果。 多個單一值，可以分組及 SPA 主控台中的資料表 所示。 資料表會有只有兩個資料行名稱和值，如下所示。

名稱 | 值
---- | ----
系統磁碟機 (GB) 上的可用磁碟大小 | 100
安裝的總磁碟大小 (GB) | 500 

如果使用者想要看到的所有伺服器和其磁碟大小已安裝的硬碟機清單中，我們可以呼叫清單的值，其具有三個資料行和多個資料列，如下所示。

Disk | 可用磁碟大小 (GB) | 大小總計 (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

Advisor 組件中可能有許多資料表 （單一值群組和列出值的資料表）。 我們可以使用一個區段，來組織及分類這些資料表。

在 [摘要] 中，有三種類型的 UI 項目：

* [區段](#bkmk-ui-section)

* [單一值群組](#bkmk-ui-svg)

* [列出值資料表](#bkmk-ui-lvt)

以下範例顯示的 UI 項目：

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>區段

區段是純粹的 UI 配置。 不會參與邏輯的任何計算。 每個單一的報表包含一組沒有父區段的最上層區段。 最上層的區段會顯示為報表中的索引標籤。 區段可以有小節中的，最多 10 個等級。 在最上層區段下的所有子區段會顯示可展開的區域中。 區段只能包含多個小節、 單一值群組和列出值的資料表。 單一值群組和列出值的資料表會顯示為資料表中。

以下是最上層區段的範例。

``` syntax
<section name="CPU" caption="CPU"/>
```

區段名稱必須是唯一的。 它正做為可由其他區段、 單一值群組，以及列出值資料表連結到索引鍵。

下列範例所擁有的屬性**父**，並且指向區段 CPU。 CPUFacts 是名為 CPU 區段的子系。 **父**必須參考先前的區段名稱; 否則可能會產生迴圈。

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

下列的單一值群組所擁有的屬性**一節**，並且可指向任何區段中，根據您的 UI 設計。

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>資料類型

單一值群組和清單值表格包含不同類型的資料，例如字串、 int、 和浮點數。 因為這些值會儲存在 SQL Server 資料庫中，您可以定義每個資料屬性的 SQL 資料類型。 不過，定義 SQL 資料類型是相當複雜。 您必須指定長度或有效位數，可能容易發生變更。

若要定義邏輯的資料類型，您可以使用的第一個子系 **&lt;reportDefinition /&gt;** ，這是您可以在其中定義的 SQL 資料類型和您的邏輯型別對應。

下列範例會定義兩種資料類型。 其中一個是**字串**，另一個是**companyCode**。

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

資料型別名稱可以是任何有效的字串。 以下是允許的 SQL 資料類型的清單：

* bigint

* 二進位檔

* 位元

* Char

* date

* datetime

* datetime2

* DateTimeOffset

* 十進位

* FLOAT

* ssNoversion

* money

* nchar

* 數值

* nvarchar

* real

* smalldatetime

* smallint

* SMALLMONEY

* time

* tinyint

* uniqueidentifier

* varbinary

* varchar

如需這些 SQL 資料類型的詳細資訊，請參閱[資料類型 & Amp;#40;transact-SQL&AMP;#41;](https://msdn.microsoft.com/library/ms187752.aspx)。

### <a href="" id="bkmk-ui-svg"></a>單一值群組

單一值群組分組在一起，以呈現在資料表中，如下所示的多個單一值。

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

在上述範例中，我們會定義單一值 」 群組。 它是子節點之區段**SystemoverviewSection**。 此群組具有單一值，也就是**OsName**， **Osversion**，並**OsLocation**。

單一的值必須是全域唯一的名稱屬性。 在此範例中，是全域唯一的名稱屬性**Systemoverview**。 唯一的名稱會用來產生自訂報表的對應檢視中。 每個檢視包含的前置詞**vw**，例如 vwSystemoverview。

雖然您可以定義多個單一值群組，但沒有兩個單一值的名稱可以是相同的即使它們位於不同的群組。 單一值的名稱供 SQL 指令碼報表中，適當地設定值。

您可以定義每個單一值的資料類型。 允許的輸入**型別**中定義 **&lt;資料類型 /&gt;** 。 最終的報告看起來像這樣：

**Facts**

名稱 | 值
--- | ---
作業系統 | &lt;_值，將會設定由報表的指令碼_&gt;
OS 版本 | &lt;_值，將會設定由報表的指令碼_&gt;
OS 位置 | &lt;_值，將會設定由報表的指令碼_&gt;

**Caption**屬性 **&lt;值 /&gt;** 第一個資料行所示。 值資料行中的值為在未來透過指令碼報告所\[dbo\]。\[SetSingleValue\]。 **描述**屬性 **&lt;值 /&gt;** 顯示工具提示中。 通常，工具提示會顯示使用者的資料來源。 如需工具提示的詳細資訊，請參閱[工具提示](#bkmk-tooltips)。

### <a href="" id="bkmk-ui-lvt"></a>列出值資料表

定義清單的值為定義的資料表相同。

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

清單值的名稱必須是全域唯一的。 此名稱會變成暫時資料表的名稱。 在上一個範例中，資料表名為\#NetworkAdapterInformation 將會建立在執行環境初始化階段，其中包含所述的所有資料行。 類似於單一值的名稱，清單值的名稱也會做為自訂的檢視表名稱，比方說，vwNetworkAdapterInformation 的一部分。

@type &lt;資料行 /&gt;由此&lt;資料型別 /&gt;

最終的報告的模擬 （mock) 的 UI 可能如下所示：

**實體網路介面卡資訊**

識別碼 | 名稱 | 類型 | 速度 (Mbps) | MAC 位址
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


**標題**屬性&lt;資料行 /&gt;會顯示為資料行名稱，而**描述**屬性&lt;資料行 /&gt;會顯示為工具提示對應的資料行標頭。 通常，工具提示會向使用者顯示的資料來源。 如需詳細資訊，請參閱 <<c0> [ 工具提示](#bkmk-tooltips)。

在某些情況下，資料表可能有許多資料行，只有少數資料列，讓交換的資料行和資料列會使資料表看起來更好。 若要交換的資料行和資料列，您可以加入下列樣式屬性：

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>定義圖表的項目

您可以挑選任何統計資料索引鍵，並檢視歷程記錄圖或趨勢圖表中的值。 有兩種類型的統計資料：

* **靜態的統計資料**單一值，也就是在設計階段。 例如，在系統磁碟機上的可用磁碟空間會是靜態的統計資料。

* **動態的統計資料**可能會在設計階段無法辨識。 例如，每個核心的平均 CPU 使用量是動態的統計資料，因為您不知道在設計階段無法在系統中的多少 CPU 核心。

統計資料索引鍵有限制的資料必須與 double 資料類型相容。 它可以是整數、 十進位或字串，可以轉換為 double。

SPA 會使用單一值的群組來支援靜態的統計資料和清單值資料表來支援動態的統計資料。 下列各節說明如何定義靜態的統計資料和動態的統計資料索引鍵。

### <a name="static-statistics"></a>靜態的統計資料

如先前所述，靜態的統計資料就會是單一值。 邏輯上來說，任何單一值可定義為靜態的統計資料。 不過，它沒有意義，檢視無法轉換成字類型的單一值。 若要定義靜態的統計資料，您可以直接將屬性**trendable**以對應單一值的索引鍵如下所示下方：

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>動態的統計資料

動態的統計資料索引鍵要知道在設計階段，因此可能的值數目是未知。 不過，因為清單值儲存在多個資料列中，很容易使用的清單值資料表來儲存動態的統計資料。

例如，如果我們需要顯示不同的核心的平均 CPU 使用量的圖表，我們可以定義的資料行的資料表**CpuId**並**AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

另一個屬性， **columntype**，可以是**金鑰**，**值**，或**資訊**。 資料類型**金鑰**資料行必須是雙精度浮點或 double 的轉換。 在 [**金鑰**] 欄中，您不能插入資料表中的相同的索引鍵。 **值**或是**Informational**資料行不會有這項限制。

統計資料值會儲存在**值**資料行。

**參考**就像正常的清單值資料表中的一般資料行的資料行。 **參考**是預設的資料行類型，如果您未指定其中一個。 這類資料行不會影響統計資料索引鍵數目，或參與統計資料相關的計算。

繼續上述範例中，如果伺服器具有兩個 CPU 核心，資料表中的結果看起來可能像這樣：

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

在此同時，SPA 架構會產生兩個統計資料索引鍵。 另一個做為 CPU 0 和另一個則用於 CPU 1。

如下列範例指出多個**值**具有多個資料行**金鑰**在支援的資料行。

CounterName | InstanceName | 平均 | Sum
--- | :---: | :---: | :---:
處理器時間百分比 | _Total | 10 | 20
處理器時間百分比 | CPU0 | 20 | 30 

在此範例中，您有兩個**金鑰**資料行和第二個**值**資料行。 SPA 會產生平均的資料行的兩個統計資料索引鍵和加總資料行的其他兩個索引鍵。 統計資料索引鍵為：

* CounterName (%Processor time) / 執行個體名稱 (\_總和) / 平均

* CounterName (%Processor time) / 執行個體名稱 (CPU0) / 平均

* CounterName (%Processor time) / 執行個體名稱 (\_總和) / 加總

* CounterName (%Processor time) / 執行個體名稱 (CPU0) / 加總

CounterName 和 InstanceName 會結合為一個索引鍵中。 結合的索引鍵不能有任何重複項目。

SPA 會產生多統計資料索引鍵。 其中部分可能不是您感興趣，您可能想要隱藏它們從 UI。 SPA 可讓開發人員建立篩選，以顯示只有有用的統計資料索引鍵。

如上述範例中，系統管理員可能只感興趣的執行個體名稱的索引鍵\_CPU1 或合計。 可以定義篩選條件如下所示：

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**&lt;trendableKeyValues /&gt;** 可以定義任何索引鍵資料行底下。 如果多個索引鍵資料行已設定，這類篩選，且會套用邏輯。

### <a name="developing-report-scripts"></a>開發報表的指令碼

佈建中繼資料定義之後，我們可以開始撰寫報表的指令碼，也就是 T-SQL 預存程序。

有**名稱**並**reportScript**屬性佈建中繼資料標頭中，如下所示：

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

主報表中的指令碼的命名方式結合**名稱**並**reportScript**屬性。 在下列範例中，將予以\[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\]。\[ReportScript\]。

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

**名稱**屬性將用於做為資料庫結構描述名稱，例如命名空間。 此規則套用至所有其他屬於目前的建議程式組件，例如清單值和預存程序的資料庫物件。

若要讓此結構描述名稱，前面的資料庫物件的優點包括：

* 避免命名衝突，不同的 advisor 組件

* 提供更高的安全性

在 SQL Server 資料庫中，預設結構描述名稱是**dbo**。 資料庫擁有者認證通常必須運作下的資料庫物件**dbo**。 如果我們不會建立每個 advisor 組件的結構描述，它可能是兩個 advisor 組件會定義具有相同名稱的清單值。 這應該是不相關，因為您可能會導致結構描述名稱來解決此問題。 此外，取消佈建的 advisor 組件會更加容易。 因為建議程式組件物件屬於結構描述以外**dbo**，這可讓 SPA 正常使用較低的使用者權限可以存取它們。

一般報表指令碼會執行下列作業：

* 存取未經處理收集的資料

* 根據原始資料的計算

* 變更警示和建議

* 報表檢視會準備資料

### <a name="access-raw-collected-data"></a>存取未經處理收集的資料

所有收集的資料會匯入下列對應的資料表。 如需資料表結構描述的詳細資訊，請參閱[定義的資料收集器集合工具](#bkmk-definedatacollector)。

* 登錄

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* 效能計數器

    * \#PerformanceCounters

* 檔案

    * \#檔案

* ETW

    * \#事件

    * \#EventProperties

### <a name="set-rule-status"></a>設定規則狀態

\[Dbo\]。\[SetNotification\] API 設定規則狀態，因此您可以看到**成功**或是**警告**在 UI 中的圖示。

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

警示和建議訊息會儲存在佈建中繼資料 XML 檔案。 這可讓報表指令碼更容易管理。

一開始，每個規則的狀態是 n/A。 您可以使用此 API 來設定規則狀態，藉由指定的建議名稱。 層級的建議名稱將用於規則的狀態。

您應該記得我們稍早定義下列規則：

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

假設的可用空間小於 2 GB，我們需要將規則設為**警告**層級。 SQL 指令碼會如下所示：

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>取得臨界值

\[Dbo\]。\[GetThreshold\] API 取得的臨界值：

* @key nvarchar(50)

* @value 浮點數輸出

> [!NOTE]
> 臨界值是名稱 / 值組，並在任何規則中所參考。 系統管理員可以使用 SPA 主控台，來調整臨界值。

 繼續上述範例中，臨界值，定義將會顯示如下：

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

可以修改報表指令碼，如下所示：

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)

```

### <a name="set-or-remove-the-single-value"></a>設定或移除單一值

\[Dbo\]。\[SetSingleValue\] API 集的一個值：

* @key nvarchar(50)

* @value sql\_variant

這個值可以多次執行相同的單一值索引鍵。 儲存最後的值。

下列範例會示範一些定義單一的值：

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

然後，如下所示，您可以設定的一個值：

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

在罕見的情況下，您可能想要移除您先前設定的使用結果\[dbo\]。\[removeSingleValue\] API。

* @key nvarchar(50)

您可以使用下列指令碼，以移除先前設定的值。

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>取得資料收集資訊

\[Dbo\]。\[GetDuration\] API 取得使用者的指定期間 （秒） 的資料收集：

* @duration int 輸出

以下範例會報告指令碼：

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\[Dbo\]。\[GetInternal\] API 取得效能計數器的間隔。 如果目前報表沒有效能計數器資訊，它可以傳回 NULL。

* @interval int 輸出

以下範例會報告指令碼：

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>設定的清單值表格

更新清單值的資料表沒有任何 API。 不過，您可以直接存取的清單值的資料表。 在初始化階段中，對應的暫存資料表將會建立每個清單的值。

下列範例顯示的清單值的表格：

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

然後，您可以撰寫 SQL 指令碼來插入、 更新或刪除的結果：

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (

)
```

## <a name="development-and-debugging"></a>開發和偵錯


### <a name="writing-logs"></a>寫入記錄檔

如果您想要傳達給系統管理員的任何進一步資訊，您可以寫入記錄。 如果沒有任何記錄檔以取得特定報表，就會顯示黃色橫幅中，報表頁首。 下列範例會示範如何寫入記錄檔：

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

第一個參數是您想要的訊息會顯示記錄檔中。 第二個參數是記錄層級。 第二個參數的有效輸入可能**Informational**，**警告**，或**錯誤**。

### <a name="debug"></a>偵錯

SPA 主控台可以執行兩種模式中偵錯或發行。 發行模式是預設值，可在報告產生之後清除所有收集的未經處理資料。 偵錯模式會保留所有未經處理資料的檔案共用和資料庫，以便您日後可以偵錯報表指令碼。

**偵錯報表指令碼**

1.  安裝 Microsoft SQL Server Management Studio (SSMS)。

2.  啟動 SSMS 之後，連線到 localhost\\SQLExpress。 請注意，您必須使用 localhost，而不是。 . 否則，您可能無法啟動 SQL Server 中的偵錯工具。

3.  執行下列指令碼，以啟用偵錯模式：

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  啟動 SPA 主控台並執行您想要偵錯的建議程式組件。

5.  等候工作完成。 如果已成功產生報告時，切換回 SSMS，並尋找最新的工作。

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    例如，輸出可能是：

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  您可以執行下列指令碼，依您想要執行識別碼 12 報表指令碼次數：

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **請注意**您也可以按 F11 來逐步執行和偵錯的前一個陳述式。



執行\[dbo\]。\[DebugReportScript\]傳回多個結果集，包括：

1.  Microsoft SQL Server 訊息和 advisor 套件記錄檔

2.  規則的結果

3.  統計資料索引鍵和值

4.  單一值

5.  清單值的所有資料表

## <a name="best-practices"></a>最佳作法

### <a name="naming-convention-and-styles"></a>命名慣例和樣式

|                                                                 Pascal 命名法大小寫                                                                 |                       駝峰式命名法大小寫                        |             大寫             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>ProvisionMetadata.xml 中的名稱</li><li>預存程序</li><li>函式</li><li>檢視名稱</li><li>暫存資料表名稱</li></ul> | <ul><li>參數名稱</li><li>本機變數</li></ul> | 適用於所有 SQL 保留關鍵字 |

### <a name="other-recommendations"></a>其他建議

* 將最合乎邏輯的項目移到其他的預存程序和使用者定義函式。

* 請您主要的指令碼越簡短越可能為了進行維護。

* 使用 SQL 物件的完整名稱。

* 您的 SQL 程式碼視為區分大小寫。

* 新增**SET NOCOUNT ON**至每個預存程序的開頭。

* 請考慮使用暫存資料表來傳輸大量資料。

* 請考慮使用**設定 XACT\_中止 ON**結束這個處理序，如果發生錯誤。

* 永遠納入 advisor 套件顯示名稱的主要版本號碼。

## <a href="" id="bkmk-advancedtopics"></a>進階的主題

### <a name="run-multiple-advisor-packs-simultaneously"></a>同時執行多個 advisor 套件

SPA 可支援同時執行多個 advisor 組件。 當您想要查看 Internet Information Services (IIS) 和核心作業系統效能，在此同時，這是特別有用。 Core OS advisor 套件也可能會使用許多資料收集器所使用的 IIS advisor 組件。 當兩個或多個 advisor 組件都在相同的目標電腦上執行時，SPA 不會兩次收集相同的資料。

下列範例示範的工作流程執行兩個 advisor 組件。

![執行多個 advisor 套件](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

合併資料收集器集合工具是僅用來收集效能計數器和 ETW 資料來源。 下列合併式規則適用於：

1. SPA 需要做為新的持續時間的最大持續時間。

2. 其中有合併衝突，請遵循下列規則：

   1. 需要最小間隔為新的間隔。

   2. 需要效能計數器的超集。 比方說，使用**程序 (\*)\\%Processor time**並**程序 (\*)\\\*，\\程序 (\*)\\\\** * 傳回更多資料，因此**程序 (\*)\\%處理器時間**並**程序 (\*)\\ \\** * 移除從合併的資料收集器集合工具。

### <a name="collect-dynamic-data"></a>收集動態資料

SPA 需求定義的資料收集器集合工具在設計階段。 您不一定能夠知道因為動態的資料和查詢路徑之前，無法得知其相依的資料可供使用，需要哪些資料用於產生報告。

比方說，如果您想要列出所有網路介面卡的好記名稱，您必須先查詢 WMI，以列舉所有網路介面卡。 每個傳回的 WMI 物件已登錄機碼路徑，它會儲存好記的名稱。 在設計階段是未知的登錄機碼路徑。 在此案例中，我們需要動態資料支援。

若要列舉所有網路介面卡，您可以使用 Windows PowerShell 使用下列的 WMI 查詢：

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

它會傳回一份網路介面卡物件。 每個物件具有名**PNPDeviceID**，可維護相對的登錄機碼路徑。 以下是從先前的查詢的範例輸出：

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

若要尋找**FriendlyName**值中，開啟登錄編輯器並瀏覽到登錄設定結合**HKEY\_本機\_機器\\SYSTEM\\CurrentControlSet\\Enum\\** 前一個範例中的每一行。 例如：**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Enum\\ ROOT\\\*IPHTTPS\\0000**.

若要解譯成 SPA 佈建中繼資料先前的步驟，新增下列程式碼範例中的指令碼：

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

在此範例中，您第一次新增 WMI 查詢下 managementpaths 並定義索引鍵名稱**NetworkAdapter**。 然後新增登錄機碼，並參考**NetworkAdapter**藉由使用語法 **$(NetworkAdapter.PNPDeviceID)** 。

下表會定義如果 SPA 中的資料收集器支援動態資料，以及是否由其他資料收集器參考：

資料類型 | 支援動態資料 | 可以參考
--- | :---: | :---:
登錄機碼 | 是 | 是
WMI | 是 | 是
檔案 | 是 | 否
效能計數器 | 否 | 否
ETW | 否 | 否

WMI 資料收集器，每個 WMI 物件都有許多的附加的屬性。 任何類型的 WMI 物件一律有三個屬性：\_\_命名空間\_\_類別，並\_ \_RELpath。

若要定義其他資料收集器所參考的資料收集器，將指派**名稱**ProvisionMetadata.xml 中以唯一索引鍵的屬性。 相依的資料收集器會使用此金鑰來產生動態資料。

以下範例登錄機碼：

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

與 WMI 的範例：

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

若要定義相依的資料收集器，使用下列語法: $( *{name}* 。 *{屬性}* ).

*{name}* 並 *{屬性}* 是預留位置。

當 SPA 會從目標伺服器中收集資料時，以動態方式取代模式 $(\*。\*) 與實際從其參考的資料收集器收集的資料 (登錄機碼 / WMI)，例如：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**請注意**SPA 支援無限的深度的參考，但如果您有太多層級是注意效能額外負荷。 請確定沒有循環參考，或自我參考不支援。

### <a name="versioning-limitations"></a>版本控制限制

SPA 支援重設和次要版本更新。 這些程序會使用相同的演算法。 此程序是重新整理所有資料庫物件和臨界值設定，但保留現有的資料。 這可以為更高的版本升級或降級至較低版本。 選取的建議程式組件，然後按一下**重設**中**設定 Advisor 組件**中重設，或套用的 SPA 或更新的對話方塊。

這項功能主要是為次要更新。 您不能大幅變更的 UI 顯示項目。 如果您想要進行重大變更，您必須建立不同的 advisor 組件。 您應該 advisor 組件名稱中包含的主要版本。

修改次要版本的限制是您**無法**執行下列任一項：

* 變更結構描述名稱

* 變更任何單一值 」 群組的資料類型或清單值資料表的資料行

* 新增或移除臨界值

* 新增或移除規則

* 新增或移除建議

* 新增或移除單一值

* 新增或移除清單值

* 新增或移除清單值的資料行

### <a href="" id="bkmk-tooltips"></a>工具提示

幾乎所有**描述**屬性將會顯示為 SPA 主控台中的工具提示。

清單值資料表中，資料列為主的工具提示可以藉由將下列屬性：

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

**DescriptionColumn**屬性參考的資料行名稱。 在此範例中，描述資料行不會顯示為實體的資料行。 不過，它會顯示為工具提示時，您將滑鼠移的第一個資料行的每個資料列。

我們建議的工具提示向使用者顯示的資料來源。 以下是用於顯示資料來源的格式：

資料來源 | 格式 | 範例
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;/&lt;欄位&gt; | WMI:Win32_OperatingSystem/Caption
效能計數器 | Perfcounter:&lt;CategoryName&gt;/&lt;InstanceName&gt; | Perfcounter:Process/%處理器時間
登錄 | 登錄： &lt;registerKey&gt; | 登錄：HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
組態檔 | ConfigFile:&lt;Filepath&gt;\[;Xpath:&lt;Xpath&gt;\]<br>**注意**<br>Xpath 是選擇性的而且檔案是 xml 檔案時，才很有效。 | ConfigFile: windir %\\System32\\inetsrv\config\\applicationHost.config<br>Xpath： 組態&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW:&lt;提供者 /&gt;(Keywords) | ETW:Windows 核心追蹤 （處理序，net）

### <a name="table-collation"></a>資料表定序

更複雜的 advisor 組件時，您可以建立您自己的變數的資料表或暫存資料表來儲存報表的指令碼中的中繼結果。

「 字串資料行的定序可能會造成問題，因為您所建立的資料表定序可能會不同於所建立的 SPA 架構。 如果您將相互關聯不同的資料表中的兩個字串資料行，您可能會看到定序錯誤。 若要避免這個問題，您應該一律定義為資料行定序的字串**SQL\_Latin1\_一般\_CP1\_CI\_AS**當您定義資料表。

以下如何定義變數的資料表：

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>收集 ETW

以下如何定義 ETW ProvisionMetadata.xml 檔案中：

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

下列提供者屬性可用於收集 ETW:

屬性 | 類型 | 描述
--- | --- | ---
guid | GUID | 提供者 GUID
工作階段 | 字串 | （選擇性，只有所需的核心事件） 的 ETW 工作階段名稱
keywordsany | Hex | 任何關鍵字 (選擇性的沒有 0x 前置詞)
keywordsAll | Hex | （選擇性） 的所有關鍵字
內容 | Hex | （選擇性） 的屬性
level | Hex | （選擇性） 的層級
bufferSize | 整數 | 緩衝區大小 （選擇性）
flushtime | 整數 | （選擇性） 清除時間
maxBuffer | 整數 | （選擇性） 的最大緩衝區
minBuffer | 整數 | （選擇性） 的最小緩衝區

有兩個輸出資料表，如下所示。

**\#事件的資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceID | int NOT NULL | 相互關聯順序識別碼
EventtypeId | int NOT NULL | 事件類型識別碼 (請參閱 [dbo]。 [Eventtypes])
ProcessId | BigInt NOT NULL | 處理序識別碼
ThreadId | BigInt NOT NULL | 執行緒識別碼
timestamp | datetime2 NOT NULL | timestamp
Kerneltime | BigInt NOT NULL | 核心時間
Usertime | BigInt NOT NULL | 使用者時間

**\#EventProperties 資料表結構描述**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceID | int NOT NULL | 相互關聯順序識別碼
名稱 | Nvarchar(100) | 內容名稱
值 | Nvarchar(4000) | 值

### <a name="etw-schema"></a>ETW 架構

可以針對.etl 檔案執行 tracerpt.exe 來產生 ETW 結構描述。 會產生 schema.man 檔案。 因為.etl 檔案的格式是相依的電腦，下列指令碼僅適用於下列情況：

1.  收集相對應的.etl 檔案的所在的電腦上執行指令碼。

2.  或者，使用相同的作業系統和元件安裝在電腦上執行指令碼。

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>詞彙


本文件中使用下列詞彙：

**Advisor 套件**

Advisor 套件是中繼資料和處理效能記錄檔所收集從目標伺服器的 SQL 指令碼的集合。 然後，advisor 套件會從效能記錄資料產生報表。 Advisor 套件中的中繼資料會定義要從目標伺服器的效能度量收集的資料。 中繼資料也會定義一組規則、 臨界值和報表的格式。 大多數情況下，advisor 組件會寫入專為單一伺服器角色、 網際網路資訊服務的例如 (IIS)。

**SPA 主控台**

SPA 主控台是指 SpaConsole.exe，也就是 Server Performance Advisor 的中央部分。 不需要 SPA 不會在您要測試的目標伺服器上執行。 SPA 主控台的 SPA，包含所有的使用者介面，從設定專案以執行分析，並檢視報表。 根據設計，SPA 會是一個兩層式應用程式。 SPA 主控台包含 UI 層和商務邏輯層的一部分。 SPA 主控台排程和處理效能分析要求。

**SPA 架構**

SPA 包含兩個主要部分、 架構和 advisor 組件。 SPA 架構會提供所有的使用者介面、 效能記錄檔處理、 組態、 錯誤處理和資料庫 Api，以及管理程序。

**SPA 專案**

SPA 專案是包含關於目標伺服器、 advisor 組件和效能分析報表所產生的建議程式組件的目標伺服器上的所有資訊的資料庫。 您可以比較，然後檢視相同的 SPA 專案中的歷程記錄和趨勢圖表。 使用者可以建立多個專案。 SPA 專案無關，並沒有在專案之間共用的資料。

**目標伺服器**

目標伺服器是實體電腦或與特定伺服器角色，例如 IIS 執行 Windows Server 虛擬機器。

**資料分析工作階段**

資料分析工作階段是效能分析與特定目標伺服器上。 資料分析工作階段可以包含多個 advisor 組件。 資料收集器集合工具從那些 advisor 組件會合併成單一的資料收集器集合工具。 單一資料分析工作階段的所有效能記錄檔會都收集在相同的時間期間。 分析執行相同的資料分析工作階段中的 advisor 組件所產生的報告，可協助使用者了解整體的效能情況，並找出效能問題的根本原因。

**Windows 事件追蹤**

[事件追蹤](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)Windows (ETW) 是 Windows 作業系統中提供的高效能、 低負擔、 可擴充的追蹤系統。 它提供程式碼剖析和偵錯功能，可用來針對各種案例進行疑難排解。 SPA 做為資料來源會使用 ETW 事件，來產生效能報告。 如需 ETW 的一般資訊，請參閱 <<c0> [ 改善偵錯和效能調整使用 ETW](https://msdn.microsoft.com/magazine/cc163437.aspx)。

**WMI 查詢**

Windows Management Instrumentation (WMI) 是 Windows 作業系統中的作業以及管理資料的基礎結構。 您可以撰寫 WMI 指令碼或應用程式，以在遠端電腦上的系統管理工作自動化。 WMI 也提供其他部分的作業系統和產品的管理資料。 SPA 使用 WMI 類別資訊和資料點做為來源產生的效能報告。

**效能計數器**

效能計數器用來提供作業系統或應用程式、 服務或驅動程式會執行程度的相關資訊。 效能計數器資料可協助判斷系統瓶頸並微調系統和應用程式的效能。 作業系統、 網路和裝置提供應用程式可以使用為使用者提供系統的執行程度的圖形化檢視的計數器資料。 SPA 使用效能計數器資訊和資料點做為來源來產生效能報告。

**效能記錄及警示**

效能記錄檔及警示 (PLA) 是 Windows 作業系統中內建的服務。 它可收集效能記錄檔和追蹤，並也會引發效能警示符合特定觸發程序時。 PLA 可用來收集效能計數器、 事件追蹤 Windows (ETW)、 WMI 查詢、 登錄機碼和組態檔。 PLA 也支援透過遠端程序呼叫 (RPC) 的遠端資料收集。 使用者定義資料收集器集合，其中包含要收集的資料、 資料收集、 資料集合的持續時間、 篩選和位置來儲存結果檔的頻率的相關資訊。 SPA 可以用以 PLA 從目標伺服器中收集所有效能資料。

**單一報表**

單一的報表是 SPA 產生的報表，根據一個單一的目標伺服器上的一個 advisor 套件的資料分析工作階段。 它可以包含通知和各種資料區段。

**並排顯示報表**

並排顯示報表是 SPA 報表來比較兩個相同的 advisor 組件的單一報表。 來自不同的目標伺服器，或是從相同的目標伺服器上的個別效能分析執行時，可能會產生兩個報表。 並排顯示的報表建立功能，來比較兩個報告，可協助使用者識別異常行為或其中一個報表中的設定。 並排顯示報表包含通知和各種資料區段。 在每個區段中，從這兩個報表的資料會列出由並存。

**趨勢圖**

趨勢圖是 SPA 報表用來調查效能問題的重複模式。 許多重複的效能問題是從伺服器或用戶端電腦，就會發生這個每日或每週，由排程的伺服器負載變更所造成。 SPA 提供的 24 小時制的趨勢圖和 7 天的趨勢圖，以找出這些問題。

使用者可以選擇一或多個資料數列，一次，也就是一個數字的值，在單一報表中，這類**平均總 CPU 使用量**。 更具體來說，數字的值是從在指定的時間的執行個體的單一 AP 所產生的單一伺服器的純量值。 SPA 會將這些值分組 24 群組，其中每個小時的日期 （7 天報表，其中每一天的一週的七個）。 SPA 計算平均、 最小值、 最大值及針對每個群組的標準差。

**歷程記錄圖表**

歷程記錄的圖表是用來顯示特定數字的值，指定的伺服器和 advisor 套件組的單一報表內的變化，經過一段時間的 SPA 報表。 使用者可以選擇多個資料數列，並一起顯示在歷程記錄圖表以了解不同的資料數列之間的相互關聯。

**資料序列**

資料序列是從相同的資料來源收集一段時間的數值資料。 相同的來源表示資料必須來自相同的目標伺服器，例如一部伺服器上 IIS 的平均要求佇列長度。

**規則**

規則是邏輯、 臨界值和描述的組合。 它們代表潛在的效能問題。 每個 advisor 套件包含多個規則。 每個規則會觸發報表的產生程序。 規則適用於單一報表中的資料的邏輯與臨界值。 如果符合這些準則，則會引發警告通知。 如果沒有，請通知設為**確定**狀態。 如果規則不適用，通知會設為 不適用 (**NA**) 狀態。

**通知**

通知是規則會向使用者顯示的資訊。 它包含規則的狀態 ( **[確定]** ， **NA**，或**警告**)，則規則，並解決效能問題的可能建議的名稱。
