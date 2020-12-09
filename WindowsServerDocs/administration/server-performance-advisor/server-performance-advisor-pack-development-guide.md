---
title: Server Performance Advisor 套件開發指南
description: Server Performance Advisor 套件開發指南
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b72ea800f63110904af80a178f96af232507344
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864347"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Server Performance Advisor 套件開發指南

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 8 Windows 10

這份適用于 Microsoft Server Performance Advisor (SPA) 的開發指南提供指導方針，協助開發人員和系統管理員開發 Advisor 套件來分析伺服器效能。

它會假設您已熟悉效能記錄檔及警示 (PLA) 、效能計數器、登錄設定、Windows Management Instrumentation (WMI) 、Windows 的事件追蹤 (ETW) ，以及 Transact-sql (T-sql) 。

如需使用 SPA 的詳細資訊，請參閱 [Server Performance Advisor 使用者手冊](server-performance-advisor-users-guide.md)。

## <a name="spa-advisor-pack-overview"></a>SPA Advisor 套件總覽


Advisor 套件通常是針對特定的伺服器角色所設計，並定義下列各項：

* 要透過 PLA 收集的資料，包括 Windows Management Instrumentation (WMI) 、效能計數器、登錄設定、檔案，以及 Windows 的事件追蹤 (ETW) 

* 顯示警示和建議的規則

*  (收集的原始資料、匯總資料或前10個清單中顯示的資料) 

* 用來查看一段時間內變更之值的統計資料

* 可以趨勢的統計資料值

Advisor 套件包含下列元素：

* **XML 中繼資料** ( # A0) 

    * [效能記錄檔及警示 (PLA) ](/previous-versions/windows/desktop/pla/pla-portal) 資料收集器集合

    * 報表版面配置

* **SQL 腳本**

    * 主要預存程式

    * SQL 物件，例如預存程式和使用者定義函數

* **ETW 架構** 檔案 (架構。 man) 這是選擇性的

### <a name="advisor-pack-workflow"></a>Advisor 套件工作流程

![advisor 套件工作流程](../media/server-performance-advisor/spa-dev-guide-workflow.png)

在此流程圖中，綠色圓形代表 advisor 套件。 所有其他圓圈都代表在 SPA 架構的程式中執行的階段。 SPA 使用 advisor 套件來收集資料、將資料匯入資料庫、初始化執行環境，以及執行 SQL 腳本。

### <a name="collect-data"></a>收集資料

使用 SPA 將特定伺服器的 advisor 套件排入佇列時，資料收集模組會從 advisor 套件查詢資料收集器集合 XML，並從目標伺服器收集資料。 原始資料會儲存在使用者指定的檔案共用中。 除非超過使用者指定的 SPA 執行持續時間，否則資料收集將不會停止。

### <a name="import-data-into-the-database"></a>將資料匯入資料庫

完成資料收集之後，每個資料類型都會匯入 SQL Server 資料庫的對應資料表中。 例如，登錄設定會匯入名為 registryKeys 的資料表中 \# 。

匯入 ETW 檔案需要 ETW 架構檔案，以解碼 .etl 檔案。 ETW 架構檔案是 XML 檔案。 您可以使用 Windows 隨附的 tracerpt.exe 來產生它。 只有當 advisor 套件需要匯入 ETW 資料時，才需要 ETW 架構檔案。

### <a name="switch-to-low-user-rights"></a>切換至低使用者權限

SPA 架構會自動調整許可權，以將所需的安全性存取層級降至最低。 由於 advisor 套件可由任何人開發或修改，因此 advisor 套件可能會包含已遭篡改的 SQL 腳本。 為了降低安全性風險，建議程式套件的任何 SQL 腳本都應以低使用者權限執行。 它只能存取有限的資料庫物件，例如公開為預存程式的臨時表和 SPA Api。 Advisor 套件中的 SQL 腳本可以呼叫這些存放區程式來與 SPA 架構互動。

### <a name="initialize-execution-environment"></a>初始化執行環境

Advisor 套件可以產生不同類型的輸出，例如通知、建議、事實資料表、統計資料，以及統計資料的圖表。 SQL 腳本會針對收集到的資料執行特定計算。 產生的結果會透過 SPA 公用 Api 儲存在臨時表中。 在初始化階段，必須布建這些臨時表和其他系統資源。

### <a name="run-sql-scripts"></a>執行 SQL 腳本

有一個由 advisor 套件開發人員命名的主要預存程式。 SPA 架構會呼叫這個預存程式來起始計算。 預存程式會使用收集到的資料，並將結束結果傳達給 SPA 架構。

### <a name="switch-to-administrative-rights"></a>切換至系統管理許可權

需要系統管理員許可權才能產生報表。 報表產生是由 SPA 完全控制。 不太可能會遭到修改。

### <a name="generate-a-report"></a>產生報告

在完成 advisor 套件的主要預存程式之前，不會保存所有的計算結果，例如通知和統計資料。 在這個階段中，SPA 架構會將來自臨時表的結束結果傳送至特定格式的資料表。 完成此階段之後，您可以使用 SPA 主控台來查看報告。

## <a name="authoring-an-advisor-pack"></a>撰寫 advisor 套件


### <a name="quick-guidelines"></a>快速指導方針

下列流程圖說明開發完整功能的 advisor 套件的步驟。 本節也包含逐步解說範例，以更清楚地說明每個步驟。

![advisor 套件開發流程](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Advisor 套件通常會以下列方式結構化：

Advisor 套件

ProvisionMetadata.xml

指令碼

main .sql

func .sql

架構男人

每個 advisor 套件都必須有一個稱為 ProvisionMetadata.xml 的檔案。 它會定義基本的 advisor 套件資訊、要收集的資料、通知和規則，以及報表的儲存和顯示方式。 SPA 架構會使用這項資訊來產生臨時表，然後將臨時表中的結果傳送至使用者可以存取的資料表。

所有報表 SQL 腳本都必須儲存在稱為 **腳本** 的子資料夾中。 基於維護目的，建議您將不同的資料庫物件儲存在不同的 SQL Server 檔案中。 至少必須有一個預存程式作為主要進入點。

> [!NOTE]
> 除非您的建議程式套件收集 ETW 追蹤，否則不需要架構 man 檔。 此架構檔案是用來描述 ETW 事件的架構，以及解碼 ETW 事件。

### <a name="defining-basic-information"></a>定義基本資訊

本節說明組成 advisor 套件的一些基本元素，包括 ProvisionMetadata.xml 和屬性。

以下是 ProvisionMetadata.xml 檔案的範例標頭：

``` syntax
<advisorPack
xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
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

### <a name="advisor-pack-version"></a>Advisor 套件版本

屬性名稱： **版本**

Advisor 套件開發人員可以定義 advisor 套件的主要和次要版本：

* 主要版本通常牽涉到顯著的改進。 舊版所產生的結果可能與新版本不相容。 我們強烈建議您在 advisor 套件名稱中包含主要版本。

* SPA 只允許在沒有資料不相容問題的次要變更時升級次要版本。

如需版本控制的詳細資訊，請參閱 [Advanced 主題](#bkmk-advancedtopics)。

### <a name="script-entry-point"></a>腳本進入點

屬性名稱： **reportScript**

SPA 架構會從腳本進入點尋找主要的預存程式名稱，並以安全的方式執行它。

### <a name="other-attributes"></a>其他屬性

以下是一些可以用來識別 advisor 套件的其他屬性：

* 顯示名稱： **displayName**

* 描述： **描述**

* 作者： **作者**

* Framework 版本： **frameworkversion** (預設為 3.0) 

* 最低作業系統版本： **minOSversion** (這會保留給未來的擴充性) 

* 遺失的事件通知： **showEventLostWarning**

### <a name="defining-the-data-collector-set"></a><a href="" id="bkmk-definedatacollector"></a>定義資料收集器集合

資料收集器集合程式會定義 SPA 架構應該從目標伺服器收集的效能資料。 它支援登錄設定、WMI、效能計數器、來自目標伺服器的檔案，以及 ETW。

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
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

在上一個範例中， **&lt; dataCollectorSet &gt; /** 的 **duration** 屬性定義資料收集的持續時間， (時間單位是秒) 。 **duration** 是必要的屬性。 此設定會控制效能計數器和 ETW 所使用的收集持續時間。

### <a name="collect-registry-data"></a>收集登錄資料

您可以從下列登錄區中收集登錄資料：

* HKEY \_ 類別 \_ 根目錄

* HKEY \_ 目前的設定 \_

* HKEY \_ 目前的 \_ 使用者

* HKEY \_ 本機 \_ 電腦

* HKEY \_ 使用者

若要收集登錄設定，請指定值名稱的完整路徑： HKEY \_ LOCAL \_ MACHINE \\ MyKey \\ MyValue

若要收集登錄機碼下的所有設定，請指定登錄機碼的完整路徑： HKEY \_ 本機 \_ 電腦 \\ MyKey\\

若要收集登錄機碼及其子機碼下的所有值 (PLA 以遞迴方式收集登錄資料) ，請使用兩個反斜線做為最後一個路徑分隔符號： HKEY \_ LOCAL \_ MACHINE \\ MyKey\\\\

若要從遠端電腦收集登錄資訊，請在登錄路徑的開頭包含電腦名稱稱： HKEY \_ LOCAL \_ MACHINE \\ MyKey \\ MyValue

例如，您可能會有如下所示的登錄機碼：

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

範例1：只傳回使用中的 PowerSchemes 和其值：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

範例2：傳回此路徑下的所有機碼值組：

> [!NOTE]
> PLA 會以使用者認證執行。 有些登錄機碼需要系統管理認證。 當列舉無法存取任何子機碼時，就會停止。

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

在執行 SQL 報表腳本之前，所有收集的資料都會匯入名為 **\# registryKeys** 的臨時表中。 下表顯示範例2的結果：

KeyName | KeytypeId | 值
------ | ----- | -------
HKEY_LOCAL_MACHINE. ..\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | 已優化電源來源
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

**#RegistryKeys** 資料表的架構如下所示：

資料行名稱 | SQL 資料類型 | 描述
-------- | -------- | --------
KeyName | NVarchar (300) NOT Null | 登錄機碼的完整路徑名稱
KeytypeId | Smallint 非 Null | 內部類型識別碼
值 | NVarchar (4000) NOT Null | 所有值

**KeytypeID** 資料行可以具有下列其中一種類型：

識別碼 | 類型
--- | ---
1 | String
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

您可以新增任何 WMI 查詢。 如需有關撰寫 WMI 查詢的詳細資訊，請參閱 [WQL (適用于 wmi 的 SQL) ](/windows/win32/wmisdk/wql-sql-for-wmi)。下列範例會查詢分頁檔案位置：

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

上述範例中的查詢會傳回一筆記錄：

標題 | 名稱 | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

由於 WMI 會傳回具有不同資料行的資料表，因此當收集的資料匯入資料庫時，SPA 會執行資料正規化，並新增至下列資料表：

**\#WMIObjects 資料表**

SequenceID | 命名空間 | ClassName | Relativepath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage。名稱 =<br>C： \\pagefile.sys | 1

**\#WmiObjectsProperties 資料表**

識別碼 | 查詢
--- | ---
1 | Root\Cimv2： select * FROM Win32_PageFileUsage

**\#WmiQueries 資料表**

識別碼 | 查詢
--- | ---
1 | Root\Cimv2： select * FROM Win32_PageFileUsage

**\#WmiObjects 資料表架構**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceId | Int NOT Null | 使資料列與其屬性相互關聯
命名空間 | NVarchar (200) NOT Null | WMI 命名空間
ClassName | NVarchar (200) NOT Null | WMI 類別名稱
Relativepath | NVarchar (500) NOT Null | WMI 相對路徑
WmiqueryId | Int NOT Null | 將 #WmiQueries 的機碼相互關聯

**\#WmiObjectProperties 資料表架構**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceId | Int NOT Null | 使資料列與其屬性相互關聯
名稱 | NVarchar (1000) NOT Null | 屬性名稱
值 | NVarchar (4000) Null | 目前屬性的值

**\#WmiQueries 資料表架構**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
識別碼 | Int NOT Null | >唯一的查詢識別碼
查詢 | NVarchar (4000) NOT Null | 布建中繼資料中的原始查詢字串

### <a name="collect-performance-counters"></a>收集效能計數器

以下是如何收集效能計數器的範例：

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

**Interval** 屬性是所有效能計數器的必要全域設定。 它會定義間隔 (時間單位是收集效能資料) 的秒數。

在上述範例中， \\ \* \\ 會每秒查詢 counter PhysicalDisk () Avg. Disk sec/Transfer。

可能有兩個實例： **\_ Total** 和 **0 C： D：**，輸出可能如下所示：

timestamp | CategoryName | CounterName | _Total 的實例值 | 實例值為 0 C： D：
---- | ---- | ---- | ---- | ----
13：45：52.630 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00100008362473995 |0.00100008362473995
13：45：53.629 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00280023414927187 | 0.00280023414927187
13：45：54.627 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00385999853230048 | 0.00385999853230048
13：45：55.626 | PhysicalDisk | Avg.Disk sec/Transfer | 0.000933297607934224 | 0.000933297607934224

若要將資料匯入資料庫中，資料將正規化為稱為 **\# PerformanceCounters** 的資料表。

CategoryDisplayName | InstanceName | CounterdisplayName | 值
---- | ---- | ---- | ----
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | 0 C： D： | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | 0 C： D： | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | 0 C： D： | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.000933297607934224
PhysicalDisk | 0 C： D： | Avg.Disk sec/Transfer | 0.000933297607934224

**注意** 當地語系化的名稱（例如 **CategoryDisplayName** 和 **CounterdisplayName**）會根據目標伺服器上所使用的顯示語言而有所不同。 如果您想要建立語言中立的 advisor 套件，請避免使用這些欄位。

**\# PerformanceCounters** 資料表架構

資料行名稱 | SQL 資料類型 | 描述
---- | ---- | ---- | ----
timestamp | datetime2 (3) 非 Null | 在 UNC 中收集的日期時間
CategoryName | NVarchar (200) NOT Null | 類別目錄名稱
CategoryDisplayName | NVarchar (200) NOT Null | 當地語系化的分類名稱
InstanceName | NVarchar (200) Null | 執行個體名稱
CounterName | NVarchar (200) NOT Null | 計數器名稱
CounterdisplayName | NVarchar (200) NOT Null | 當地語系化的計數器名稱
值 | Float 非 Null | 收集的值

### <a name="collect-files"></a>收集檔案

路徑可以是絕對或相對路徑。 檔案名可以包含萬用字元 (\*) ，而問號 (？ ) 。 例如，若要收集暫存資料夾中的所有檔案，您可以指定 c： \\ temp \\ \* 。 萬用字元會套用至指定之資料夾中的檔案。

如果您也想要從指定之資料夾的子資料夾收集檔案，請使用兩個反斜線做為最後一個資料夾分隔符號，例如 c： \\ temp \\ \\ \* 。

以下是查詢 **applicationHost.config** 檔案的範例：

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

您可以 **在稱為檔案的資料表中找到 \#** 結果，例如：

querypath | Fullpath | Parentpath | FileName | 內容
----- | ----- | ----- | ----- | -----
% windir% \...\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | Applicationhost.config g | 0x3C3F78

**\#檔案資料表架構**

資料行名稱 | SQL 資料類型 | 描述
---- | ---- | ----
querypath | NVarchar (300) NOT Null | 原始查詢語句
Fullpath | NVarchar (300) NOT Null | 絕對檔案路徑和檔案名
Parentpath | NVarchar (300) NOT Null | 檔案路徑
FileName | NVarchar (300) NOT Null | 檔案名稱
內容 | Varbinary (MAX) Null | 二進位檔中的檔案內容

### <a name="defining-rules"></a>定義規則

使用來自目標伺服器的 PLA 收集到足夠的資料之後，advisor 套件可以使用此資料進行驗證，並向系統管理員顯示快速摘要。

規則提供伺服器效能的快速總覽。 他們會醒目提示問題並提供建議。 您可以列出您想要針對 advisor 套件進行驗證的所有規則。 例如，如果您想要開發核心的作業系統 advisor 套件，可能的規則可能包括：

* CPU 電源模式是否省電

* 伺服器是否在虛擬化環境中

* 是否有磁片 i/o 壓力

規則包含下列元素：

* 相依閾值 (規則可設定的部分) 

* 規則定義 (警示和建議) 

以下是簡單規則的範例：

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

### <a name="threshold"></a>臨界值

閾值是可設定的因素，可讓系統管理員決定何時規則應顯示良好或不良的狀態。 下列範例顯示的規則可偵測系統磁片磁碟機上的可用空間，以及當可用空間小於 10 GB 時的警告。

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

不過，在此情況下，系統管理員有較小的硬碟。 他認為 5 GB 的可用空間可能仍是不錯的條件，而且不想看到警告。 他可以透過 SPA 主控台將預設值從10更新為5，而不需要瞭解如何開發 advisor 套件。

引入閾值可協助系統管理員快速變更此值，而不需要修改 advisor 套件。

在此範例中，除了 **description** 以外的所有屬性都是必要的。 您可以使用任何數位作為 **值**。

您可以在規則之間共用閾值。

### <a name="alerts-and-recommendations"></a>警示和建議

規則定義不牽涉到任何邏輯計算。 它會定義 UI 的外觀，以及 SQL Server 報表腳本如何將結果傳達給 UI。

規則有三個部分：

* 警示 (規則標題) 

* 建議 (建議) 

* 相關聯的閾值 (相依性的選擇性資訊) 

以下是規則的範例：

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

您可以定義想要的建議數量，而且通常會定義建議。 建議 **等級** 可以是 **成功** 或 **警告**。

您可以依需要連結至任意數量的閾值。 您甚至可以連結至與目前規則無關的臨界值。 連結可協助 SPA 主控台輕鬆地管理閾值。

規則名稱和建議是索引鍵，而且在其範圍內是唯一的。 沒有兩個規則可以有相同的名稱，而且一個規則內的兩個建議都不能有相同的名稱。 當您撰寫 SQL 腳本報表時，這些名稱會很重要。 您可以呼叫 \[ dbo \] 。 \[用 \] 來設定規則狀態的 SetNotification API。

### <a name="defining-ui-display-elements"></a>定義 UI 顯示元素

定義規則之後，系統管理員可以看到報表摘要。 不過，通常系統管理員對匯總資料有興趣，而且想要檢查效能規則中所使用的資料來源。

繼續先前的範例，使用者知道系統磁片磁碟機上是否有足夠的可用磁碟空間。 使用者可能也會想知道可用空間的實際大小。 單一值群組可用來儲存和顯示這類結果。 多個單一值可以分組，並顯示在 SPA 主控台的資料表中。 資料表只有兩個數據行：名稱和值，如下所示。

名稱 | 值
---- | ----
系統磁片磁碟機上的可用磁片大小 (GB)  | 100
安裝的磁片大小總計 (GB)  | 500 

如果使用者想要查看安裝在伺服器上的所有硬碟及其磁片大小的清單，我們可以呼叫清單值，它有三個數據行和多個資料列，如下所示。

磁碟 | 可用磁片大小 (GB)  | 總大小 (GB) 
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

在 advisor 套件中，可能會有許多資料表 (單一值群組和清單值資料表) 。 我們可以使用區段來組織和分類這些資料表。

總而言之，有三種類型的 UI 元素：

* [章節](#bkmk-ui-section)

* [單一值群組](#bkmk-ui-svg)

* [清單值表](#bkmk-ui-lvt)

以下是顯示 UI 元素的範例：

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

### <a name="sections"></a><a href="" id="bkmk-ui-section"></a>章節

區段純粹適用于 UI 版面配置。 它不會參與任何邏輯計算。 每一份報表都包含一組最上層區段，而這些區段沒有父區段。 最上層區段會顯示為報表中的索引標籤。 區段可以有子區段，最多可有10個層級。 最上層區段底下的所有子區段都會顯示在可擴充的區域中。 區段可以包含多個子區段、單一值群組和清單值資料表。 單一值群組和清單值資料表會顯示為數據表。

以下是最上層區段的範例。

``` syntax
<section name="CPU" caption="CPU"/>
```

區段名稱必須是唯一的。 它可用來做為可由其他區段、單一值群組和清單值資料表連結的索引鍵。

下列範例有一個屬性（ **parent**），而且它指向 [CPU] 區段。 CPUFacts 是名為 CPU 之區段的子系。 **父系** 必須參考先前的區段名稱;否則，它可能會產生迴圈。

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

下列單一值群組有一個屬性（ **section**），它可以根據您的 UI 設計來指向任何區段。

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>資料類型

單一值群組和清單值表包含不同類型的資料，例如字串、int 和 float。 因為這些值會儲存在 SQL Server 資料庫中，所以您可以定義每個資料屬性的 SQL 資料類型。 不過，定義 SQL 資料類型相當複雜。 您必須指定長度或有效位數，這可能會很容易變更。

若要定義邏輯資料類型，您可以使用 **&lt; reportdefinition.xsd/ &gt;** 的第一個子系，也就是您可以在其中定義 SQL 資料類型和邏輯型別的對應。

下列範例會定義兩個資料類型。 其中一個是 **字串** ，另一個則是 **companyCode**。

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

資料類型名稱可以是任何有效的字串。 以下是允許的 SQL 資料類型清單：

* BIGINT

* BINARY

* bit

* char

* date

* Datetime

* datetime2

* datetimeoffset

* decimal

* FLOAT

* int

* money

* NCHAR

* NUMERIC

* NVARCHAR

* real

* smalldatetime

* SMALLINT

* SMALLMONEY

* time

* TINYINT

* UNIQUEIDENTIFIER

* varbinary

* varchar

如需這些 SQL 資料類型的詳細資訊，請參閱 [ (transact-sql) 的資料類型 ](/sql/t-sql/data-types/data-types-transact-sql)。

### <a name="single-value-groups"></a><a href="" id="bkmk-ui-svg"></a>單一值群組

單一值群組會將多個單一值群組在一起，以顯示在資料表中，如下所示。

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

在上述範例中，我們定義了單一值群組。 它是 **SystemoverviewSection** 區段的子節點。 此群組具有單一值，也就是 **OsName**、 **Osversion** 和 **OsLocation**。

單一值必須有全域唯一的名稱屬性。 在此範例中，全域唯一名稱屬性為 **Systemoverview**。 唯一名稱將用來產生自訂報表的對應視圖。 每個視圖都包含前置詞 **vw**，例如 vwSystemoverview。

雖然您可以定義多個單一值群組，但是沒有兩個單一值名稱可以相同，即使它們位於不同的群組中也一樣。 SQL 腳本報表會使用單一值名稱，據此設定值。

您可以定義每個單一值的資料類型。 **Type** 的允許輸入是在 **&lt; datatype/ &gt;** 中定義。 最終的報告看起來可能像這樣：

**事實**

名稱 | 值
--- | ---
作業系統 | &lt;_值將由報表腳本設定_&gt;
OS 版本 | &lt;_值將由報表腳本設定_&gt;
OS 位置 | &lt;_值將由報表腳本設定_&gt;

**&lt; 值 &gt; /** 的 **caption** 屬性會顯示在第一個資料行中。 [值] 資料行中的值會在未來透過 dbo 的腳本報表設定 \[ \] 。 \[SetSingleValue \] 。 [ **&lt; 值 &gt; /** ] 的 [**描述**] 屬性會顯示在工具提示中。 工具提示通常會顯示使用者資料的來源。 如需工具提示的詳細資訊，請參閱 [工具提示](#bkmk-tooltips)。

### <a name="list-value-tables"></a><a href="" id="bkmk-ui-lvt"></a>清單值表

定義清單值與定義資料表相同。

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

清單值名稱必須是全域唯一的。 這個名稱會成為臨時表的名稱。 在上述範例中，名為 NetworkAdapterInformation 的資料表 \# 將會在執行環境初始化階段建立，其中包含所描述的所有資料行。 類似于單一值名稱，清單值名稱也會當做自訂視圖名稱的一部分使用，例如，vwNetworkAdapterInformation。

@type of &lt; 資料行/ &gt; 是由 &lt; datatype/&gt;

最終報表的 mock UI 看起來可能如下：

**實體網路介面卡資訊**

ID | 名稱 | 類型 | 加速 (Mbps)  | MAC 位址
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


[資料行/] 的 [ **標題** ] 屬性 &lt; &gt; 會顯示為數據行名稱，而 [資料行/] 的 [ **描述** ] 屬性 &lt; &gt; 會顯示為對應之資料行標題的工具提示。 工具提示通常會顯示使用者資料的來源。 如需詳細資訊，請參閱 [工具提示](#bkmk-tooltips)。

在某些情況下，資料表可能有許多資料行，而且只有幾個資料列，因此，交換資料行和資料列會讓資料表看起來更好。 若要交換資料行和資料列，您可以加入下列樣式屬性：

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>定義圖表元素

您可以挑選任何統計資料索引鍵，並查看歷程記錄圖表或趨勢圖中的值。 有兩種類型的統計資料：

* **靜態統計資料** 在設計階段已知的單一值。 例如，系統磁片磁碟機上的可用磁碟空間會是靜態統計資料。

* **動態統計資料** 在設計階段可能未知。 例如，每個核心的平均 CPU 使用率為動態統計資料，因為您在設計階段不知道系統中有多少 CPU 核心。

統計資料索引鍵的限制是資料必須與 double 資料類型相容。 它可以是整數、十進位或可以轉換成 double 的字串。

SPA 使用單一值群組來支援靜態統計資料，並使用清單值資料表來支援動態統計資料。 下列各節說明如何定義靜態統計資料和動態統計資料索引鍵。

### <a name="static-statistics"></a>靜態統計資料

如先前所述，靜態統計資料是單一值。 邏輯上，任何單一值都可以定義為靜態統計資料。 但是，若要查看無法轉換成數位類型的單一值，則沒有意義。 若要定義靜態統計資料，您可以直接將屬性 **trendable** 新增至對應的單一值索引鍵，如下所示：

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>動態統計資料

在設計階段不知道動態統計資料索引鍵，因此可能的值數目未知。 不過，因為清單值會儲存在多個資料列中，所以使用清單值資料表來儲存動態統計資料很容易。

例如，如果我們需要針對不同核心的平均 CPU 使用率來顯示圖表，我們可以定義具有 **CpuId** 和 **AverageCpuUsage** 資料行的資料表：

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

另一個屬性 **columntype** 可以是索引 **鍵**、 **值** 或 **資訊**。 索引 **鍵** 資料行的資料類型必須是 double 或 double 轉換。 在索引 **鍵** 資料行中，您無法將相同的索引鍵插入資料表中。 **值** 或 **資訊** 資料行沒有這項限制。

統計資料值會儲存在 **值** 資料行中。

**資訊** 資料行就像一般清單值表中的一般資料行。 如果您未指定，**資訊** 是預設資料行類型。 這類資料行不會影響統計資料索引鍵的數目，也不會參與統計資料相關的計算。

繼續先前的範例，如果伺服器有兩個 CPU 核心，則資料表中的結果可能如下所示：

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

同時，SPA 架構會產生兩個統計資料索引鍵。 一個用於 CPU 0，另一個則用於 CPU 1。

如下列範例所示，表示支援多個索引 **鍵** 資料行的多個 **值** 資料行。

CounterName | InstanceName | Average | Sum
--- | :---: | :---: | :---:
% Processor time | _Total | 10 | 20
% Processor time | CPU0 | 20 | 30 

在此範例中，您有兩個索引 **鍵** 資料行和兩個 **值** 資料行。 SPA 會為平均資料行產生兩個統計資料索引鍵，並為 Sum 資料行產生另兩個索引鍵。 統計資料索引鍵為：

* CounterName (% Processor time) /InstanceName (\_ Total) /Average

* CounterName (% Processor time) /InstanceName (CPU0) /Average

* CounterName (% Processor time) /InstanceName (\_ Total) /Sum

* CounterName (% Processor time) /InstanceName (CPU0) /Sum

CounterName 和 InstanceName 會合並為一個索引鍵。 組合索引鍵不能有任何重複。

SPA 會產生許多統計資料索引鍵。 其中有些可能不會對您感興趣，而且您可能會想要從 UI 中隱藏它們。 SPA 可讓開發人員建立篩選器，只顯示有用的統計資料索引鍵。

在上述範例中，系統管理員可能只對 InstanceName 為 Total 或 CPU1 的金鑰感興趣 \_ 。 篩選可以定義如下：

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

**&lt; trendableKeyValues/ &gt;** 可以定義在任何索引鍵資料行底下。 如果有一個以上的索引鍵資料行已設定此篩選準則，則會套用邏輯。

### <a name="developing-report-scripts"></a>開發報表腳本

定義布建中繼資料之後，我們就可以開始撰寫報表腳本，也就是 T-sql 預存程式。

布建中繼資料標頭中有 **name** 和 **reportScript** 屬性，如下所示：

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

主要報表腳本的命名方式是結合 **name** 和 **reportScript** 屬性。 在下列範例中，它將會是 \[ ServerPerformanceAdvisor. CoreOS。 \] \[ReportScript \] 。

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

**Name** 屬性將用來做為資料庫架構名稱，例如命名空間。 此規則會套用至屬於目前 advisor 套件的所有其他資料庫物件，例如清單值和預存程式。

在資料庫物件前面加上此架構名稱的優點包括：

* 避免不同 advisor 套件的命名衝突

* 提供更高的安全性

在 SQL Server 資料庫中，預設架構名稱是 **dbo**。 資料庫擁有者認證通常是在 **dbo** 下操作資料庫物件時所需的認證。 如果我們未建立每個建議程式套件的架構，則兩個 advisor 套件可能會定義名稱相同的清單值。 這應該是不相關的，因為您可以引入架構名稱來解決此問題。 此外，取消布建 advisor 套件會更容易。 因為 advisor 套件物件屬於 **dbo** 以外的架構，所以可讓 SPA 使用較低的使用者權限來存取它們。

一般報表腳本會執行下列動作：

* 存取原始收集的資料

* 根據原始資料執行計算

* 變更警示和建議

* 準備報表檢視的資料

### <a name="access-raw-collected-data"></a>存取原始收集的資料

所有收集到的資料都會匯入至下列對應的資料表。 如需有關資料表架構的詳細資訊，請參閱 [定義資料收集器集合](#bkmk-definedatacollector)。

* 登錄

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* 效能計數器

    * \#PerformanceCounters

* 檔案

    * \#檔案儲存體

* ETW

    * \#事件

    * \#EventProperties

### <a name="set-rule-status"></a>設定規則狀態

\[Dbo \] 。 \[SetNotification \] API 會設定規則狀態，讓您可以在 UI 中看到 **成功** 或 **警告** 圖示。

* @ruleName Nvarchar (50) 

* @adviceName Nvarchar (50) 

警示和建議訊息會儲存在布建中繼資料 XML 檔案中。 這樣可讓報表腳本更容易管理。

一開始，每個規則狀態為 N/A。 您可以使用此 API，藉由指定建議名稱來設定規則狀態。 建議名稱的層級將用來作為規則狀態。

回想一下，我們稍早定義了下列規則：

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

假設可用空間小於 2 GB，則需要將規則設定為 **警告** 層級。 SQL 腳本會如下所示：

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

\[Dbo \] 。 \[GetThreshold \] API 會取得閾值：

* @key Nvarchar (50) 

* @value float 輸出

> [!NOTE]
> 閾值是成對的名稱和值，可在任何規則中參考。 系統管理員可以使用 SPA 主控台來調整臨界值。

 繼續先前的範例，如果是閾值，定義將如下所示：

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

您可以修改報告腳本，如下所示：

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)

```

### <a name="set-or-remove-the-single-value"></a>設定或移除單一值

\[Dbo \] 。 \[SetSingleValue \] API 會設定單一值：

* @key Nvarchar (50) 

* @value sql \_ 變異

這個值可以針對相同的單一值索引鍵執行多次。 最後一個值會儲存。

下列範例顯示一些已定義的單一值：

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

然後您可以設定單一值，如下所示：

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

在罕見的情況下，您可能會想要移除先前使用 dbo 設定的結果 \[ \] 。 \[removeSingleValue \] API。

* @key Nvarchar (50) 

您可以使用下列腳本來移除先前設定的值。

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>取得資料收集資訊

\[Dbo \] 。 \[GetDuration \] API 會取得使用者指定的資料收集持續時間（以秒為單位）：

* @duration int 輸出

以下是範例報表腳本：

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\[Dbo \] 。 \[GetInternal \] API 會取得效能計數器的間隔。 如果目前的報表沒有效能計數器資訊，它可能會傳回 Null。

* @interval int 輸出

以下是範例報表腳本：

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>設定清單值表

沒有可更新清單值資料表的 API。 不過，您可以直接存取清單值資料表。 在初始化階段，將會為每個清單值建立對應的臨時表。

下列範例顯示清單值表：

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

然後您可以撰寫 SQL 腳本來插入、更新或刪除結果：

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

## <a name="development-and-debugging"></a>開發和調試


### <a name="writing-logs"></a>寫入記錄檔

如果有您想要與系統管理員通訊的其他資訊，您可以撰寫記錄檔。 如果有特定報表的任何記錄檔，報表標頭中會顯示黃色橫幅。 下列範例會示範如何撰寫記錄：

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

第一個參數是您想要在記錄檔中顯示的訊息。 第二個參數是記錄層級。 第二個參數的有效輸入可能是 **資訊**、 **警告** 或 **錯誤**。

### <a name="debug"></a>偵錯

SPA 主控台可在兩種模式中執行： Debug 或 Release。 發行模式是預設值，它會在產生報告之後清除所有收集的原始資料。 Debug 模式會保留檔案共用和資料庫中的所有原始資料，讓您可以在未來進行報表腳本的偵錯工具。

**若要對報表腳本進行調試**

1.  安裝 Microsoft SQL Server Management Studio (SSMS) 。

2.  啟動 SSMS 之後，請連線到 localhost \\ SQLExpress。 請注意，您必須使用 localhost，而不是。 . 否則，您可能無法在 SQL Server 中啟動偵錯工具。

3.  執行下列腳本以啟用 Debug 模式：

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  啟動 SPA 主控台，並執行您想要進行偵錯工具的 advisor 套件。

5.  等候工作完成。 如果報表已成功產生，請切換回 SSMS，並尋找最新的工作。

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    例如，輸出可能是：

    識別碼 | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05：35：24.387 | 1

6.  當您想要執行識別碼12的報表腳本時，您可以執行下列腳本多次：

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **注意** 您也可以按 F11 來逐步執行先前的語句和 debug。



正在執行 \[ dbo \] 。 \[DebugReportScript 會傳回 \] 多個結果集，包括：

1.  Microsoft SQL Server 訊息和 advisor 套件記錄檔

2.  規則的結果

3.  統計資料索引鍵和值

4.  單一值

5.  所有清單值資料表

## <a name="best-practices"></a>最佳作法

### <a name="naming-convention-and-styles"></a>命名慣例和樣式

|                                                                 Pascal 大小寫                                                                 |                       駝峰式命名法的大小寫                        |             大寫             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>ProvisionMetadata.xml 中的名稱</li><li>預存程序</li><li>函式</li><li>視圖名稱</li><li>臨時表名稱</li></ul> | <ul><li>參數名稱</li><li>區域變數</li></ul> | 用於所有 SQL 保留關鍵字 |

### <a name="other-recommendations"></a>其他建議

* 將最多的邏輯片段移至其他預存程式和使用者定義函數。

* 讓您的主要腳本盡可能簡短，以進行維護。

* 使用 SQL 物件的完整名稱。

* 將您的 SQL 程式碼視為區分大小寫。

* 將 **SET NOCOUNT** 加入至每個預存程式的開頭。

* 請考慮使用臨時表來傳送大量的資料。

* 如果發生錯誤，請考慮使用 **SET 交易 \_ ABORT** 來終止進程。

* 請一律在 advisor 套件顯示名稱中包含主要版本號碼。

## <a name="advanced-topics"></a><a href="" id="bkmk-advancedtopics"></a>進階主題

### <a name="run-multiple-advisor-packs-simultaneously"></a>同時執行多個 advisor 套件

SPA 支援同時執行多個 advisor 套件。 當您想要同時查看 Internet Information Services (IIS) 和核心作業系統效能時，這項功能特別有用。 「核心 OS 建議程式套件」也可以使用「IIS advisor 套件」使用的許多資料收集器。 當兩個或多個 advisor 套件在同一部目的電腦上執行時，SPA 不會收集相同的資料兩次。

下列範例顯示執行兩個 advisor 套件的工作流程。

![正在執行多個 advisor 套件](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

合併資料收集器集合器只用于收集效能計數器和 ETW 資料來源。 下列合併規則適用：

1. SPA 採用最大的持續時間作為新的持續時間。

2. 如果有合併衝突，則會遵循下列規則：

   1. 以最小間隔做為新的間隔。

   2. 取得效能計數器的超級組。 例如，使用 **進程 (\*) \\ % processor time** 和 **process (\*) \\ \* ， \\ 進程 (\*) \\ \\*** 會傳回更多資料，因此會從合併的資料收集器集合中移除處理常式 **(\*) \\ % 處理器時間** 和 **進程 (\*) \\ \\***。

### <a name="collect-dynamic-data"></a>收集動態資料

SPA 在設計階段需要定義的資料收集器集合。 由於動態資料和查詢路徑的相依資料可供使用，因此不一定可以知道產生報告所需的資料。

例如，如果您想要列出網路介面卡的所有易記名稱，您必須先查詢 WMI 來列舉所有網路介面卡。 每個傳回的 WMI 物件都有一個登錄機碼路徑，它會在其中儲存易記名稱。 登錄機碼路徑在設計階段是未知的。 在此情況下，我們需要動態資料支援。

若要列舉所有網路介面卡，您可以使用下列 WMI 查詢 Windows PowerShell：

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

它會傳回網路介面卡物件的清單。 每個物件都有一個稱為 **PNPDeviceID** 的屬性，它會維護相對的登錄機碼路徑。 以下是前一個查詢的範例輸出：

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

若要尋找 **FriendlyName** 值，請開啟 [登錄編輯程式]，並藉由結合 **HKEY \_ LOCAL \_ MACHINE \\ SYSTEM \\ CurrentControlSet \\ Enum \\** 與上一個範例中的每一行，來流覽至登錄設定。 例如： **HKEY \_ LOCAL \_ MACHINE \\ SYSTEM \\ CurrentControlSet \\ Enum \\ ROOT \\ \* IPHTTPS \\ 0000**。

若要將先前的步驟轉譯成 SPA 布建中繼資料，請在下列程式碼範例中新增腳本：

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

在此範例中，您會先在 managementpaths 下新增 WMI 查詢，然後定義索引鍵名稱 **NetworkAdapter**。 然後您會新增登錄機碼，並使用 " **(NetworkAdapter. PNPDeviceID)** 語法來參考 **NetworkAdapter** 。

下表定義 SPA 中的資料收集器是否支援動態資料，以及是否可供其他資料收集器參考：

資料類型 | 支援動態資料 | 可以參考
--- | :---: | :---:
登錄機碼 | 是 | 是
WMI | 是 | 是
檔案 | 是 | 否
效能計數器 | 否 | 否
ETW | 否 | 否

針對 WMI 資料收集器，每個 WMI 物件都有許多附加屬性。 任何類型的 WMI 物件一律有三個屬性： \_ \_ NAMESPACE、 \_ \_ CLASS 和 \_ \_ RELpath。

若要定義其他資料收集器所參考的資料收集器，請在 ProvisionMetadata.xml 中指派具有唯一索引鍵的 **名稱** 屬性。 相依資料收集器會使用此索引鍵來產生動態資料。

以下是登錄機碼的範例：

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

以及 WMI 的範例：

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

若要定義相依的資料收集器，請使用下列語法： $ (*{name}*。*{attribute}*) 。

*{name}* 和 *{attribute}* 是預留位置。

當 SPA 從目標伺服器收集資料時，它會動態地取代模式 $ (\* 。 \*) 與參考資料收集器 (登錄機碼/WMI) 的實際收集資料，例如：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**注意** SPA 支援無限制的參考深度，但如果您有太多層級，請注意效能負擔。 請確定沒有迴圈參考或不支援的自我參考。

### <a name="versioning-limitations"></a>版本控制限制

SPA 支援重設和次要版本更新。 這些進程會使用相同的演算法。 程式是重新整理所有資料庫物件和臨界值設定，但保留現有的資料。 這可以升級為較高的版本，或降級為較低的版本。 選取 advisor 套件，然後在 SPA 的 [設定建議程式 **套件**] 對話方塊中按一下 [**重設**]，以重設或套用或更新。

這項功能主要是用於次要更新。 您無法大幅變更 UI 顯示元素。 如果您想要進行重大變更，則必須建立不同的 advisor 套件。 您應在 advisor 套件名稱中包含主要版本。

次要版本修改的限制是，您 **無法** 執行下列任何一項動作：

* 變更架構名稱

* 變更任何單一值群組的資料類型或清單值資料表的資料行

* 新增或移除閾值

* 新增或移除規則

* 新增或移除建議

* 新增或移除單一值

* 新增或移除清單值

* 新增或移除清單值的資料行

### <a name="tooltips"></a><a href="" id="bkmk-tooltips"></a>提示

幾乎所有的 **描述** 屬性都會顯示為 SPA 主控台中的工具提示。

針對清單值表，您可以藉由新增下列屬性來達成以資料列為基礎的工具提示：

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

**DescriptionColumn** 屬性是指資料行的名稱。 在此範例中，[描述] 資料行不會顯示為實體資料行。 但是，當您將滑鼠移至第一個資料行的每個資料列時，它會顯示為工具提示。

建議您將資料來源顯示給使用者。 以下是用來顯示資料來源的格式：

資料來源 | [格式] | 範例
--- | --- | ---
WMI | WMI： &lt; wmiclass &gt; / &lt; 欄位&gt; | WMI： Win32_OperatingSystem/Caption
效能計數器 | Perfcounter： &lt; 類別名稱 &gt; / &lt; InstanceName&gt; | Perfcounter：進程/處理器時間百分比
登錄 | 登錄： &lt; registerKey&gt; | 登錄： HKLM\SOFTWARE\Microsoft<br>\\ASP.NET \\ Rootver
組態檔 | Read-configfile： &lt; Filepath &gt; \[ ;Xpath： &lt; xpath&gt;\]<br>**注意**<br>Xpath 是選擇性的，只有當檔案是 xml 檔案時，它才有效。 | Read-configfile： windir% \\ System32 \\ inetsrv\config \\applicationHost.config<br>Xpath &frasl; ： configuration system.webserver<br>&frasl;HTTPProtocol&frasl;@allowKeepAlive
ETW | ETW： &lt; Provider/ &gt; (關鍵字)  | ETW： Windows 核心追蹤 (進程、net) 

### <a name="table-collation"></a>資料表定序

當 advisor 套件變得更複雜時，您可以建立自己的變數資料表或臨時表，以在報表腳本中儲存中繼結果。

定序字串資料行可能會有問題，因為您建立的資料表定序可能與 SPA 架構所建立的定序不同。 如果您將不同資料表中的兩個字串資料行相互關聯，您可能會看到定序錯誤。 若要避免這個問題，您應該一律將資料行定序的字串定義為 **SQL \_ 採用 latin1-general \_ 一般 \_ CP1 \_ CI， \_ 如同** 當您定義資料表時一樣。

以下是如何定義變數資料表：

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>收集 ETW

以下是如何在 ProvisionMetadata.xml 檔案中定義 ETW：

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

下列提供者屬性可用於收集 ETW：

屬性 | 類型 | 描述
--- | --- | ---
guid | GUID | 提供者 GUID
工作階段 (session) | 字串 | ETW 會話名稱 (選擇性，只有核心事件才需要) 
keywordsany | Hex | 任何關鍵字 (選擇性，沒有0x 前置詞) 
keywordsAll | Hex | 所有關鍵詞 (選擇性) 
properties | Hex | 屬性 (選擇性) 
等級 | Hex | 層級 (選擇性) 
bufferSize | Int | 緩衝區大小 (選擇性) 
flushtime | Int | 清除時間 (選擇性) 
maxBuffer | Int | 最大緩衝區 (選擇性) 
minBuffer | Int | 最小緩衝區 (選擇性) 

有兩個輸出資料表，如下所示。

**\#事件資料表架構**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceID | Int NOT Null | 相互關聯序列識別碼
EventtypeId | Int NOT Null | 事件種類識別碼 (請參閱 [dbo]。[Eventtypes] ) 
ProcessId | BigInt 非 Null | 處理序識別碼
ThreadId | BigInt 非 Null | 執行緒識別碼
timestamp | datetime2 NOT Null | timestamp
Kerneltime | BigInt 非 Null | 核心時間
Usertime | BigInt 非 Null | 使用者時間

**\#EventProperties 資料表架構**

資料行名稱 | SQL 資料類型 | 描述
--- | --- | ---
SequenceID | Int NOT Null | 相互關聯序列識別碼
名稱 | NVarchar (100)  | 屬性名稱
值 | Nvarchar(4000) | 值

### <a name="etw-schema"></a>ETW 架構

您可以針對 etl 檔案執行 tracerpt.exe 來產生 ETW 架構。 產生架構的 man 檔。 由於 .etl 檔案的格式與電腦相依，因此下列腳本僅適用于下列情況：

1.  在收集對應的 .etl 檔案的電腦上執行腳本。

2.  或在已安裝相同作業系統和元件的電腦上執行腳本。

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>詞彙


本檔中使用下列詞彙：

**Advisor 套件**

Advisor 套件是中繼資料和 SQL 腳本的集合，可處理從目標伺服器收集而來的效能記錄。 Advisor 套件接著會從效能記錄資料產生報告。 Advisor 套件中的中繼資料會定義要從目標伺服器收集以進行效能測量的資料。 中繼資料也會定義一組規則、臨界值和報表格式。 最常見的情況是，建議程式套件專門針對單一伺服器角色撰寫，例如 Internet Information Services (IIS) 。

**SPA 主控台**

SPA 主控台是指 SpaConsole.exe，也就是 Server Performance Advisor 的核心部分。 SPA 不需要在您要測試的目標伺服器上執行。 SPA 主控台包含 SPA 的所有使用者介面，從設定專案到執行分析和查看報表都是如此。 SPA 在設計上是兩層式應用程式。 SPA 主控台包含 UI 層和一部分的商務邏輯層。 SPA 主控台會排定並處理效能分析要求。

**SPA 架構**

SPA 包含兩個主要部分：架構和 advisor 套件。 SPA 架構會提供所有使用者介面、效能記錄檔處理、設定、錯誤處理和資料庫 Api，以及管理程式。

**SPA 專案**

SPA 專案是一種資料庫，其中包含在 advisor 套件的目標伺服器上產生之目標伺服器、advisor 套件和效能分析報表的所有相關資訊。 您可以比較和查看相同 SPA 專案內的歷程記錄和趨勢圖。 使用者可以建立一個以上的專案。 SPA 專案彼此獨立，不會在專案之間共用任何資料。

**目標伺服器**

目標伺服器是以特定伺服器角色（例如 IIS）執行 Windows Server 的實體電腦或虛擬機器。

**資料分析會話**

資料分析會話是特定目標伺服器上的效能分析。 資料分析會話可以包含多個 advisor 套件。 來自這些 advisor 套件的資料收集器集合程式會合並成單一資料收集器集合。 單一資料分析會話的所有效能記錄都會在相同的時間週期內收集。 分析在相同資料分析會話中執行的 advisor 套件所產生的報表，可協助使用者瞭解整體效能狀況，並找出效能問題的根本原因。

**Windows 事件追蹤**

Windows[事件追蹤](/windows/win32/etw/event-tracing-portal) (ETW) 是 windows 作業系統中提供的高效能、低負擔、可擴充的追蹤系統。 它會提供程式碼剖析和偵錯工具，可用於針對各種案例進行疑難排解。 SPA 使用 ETW 事件做為產生效能報告的資料來源。 如需 ETW 的一般資訊，請參閱 [使用 Etw 改善偵錯工具和效能微調](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw)。

**WMI 查詢**

Windows Management Instrumentation (WMI) 是 Windows 作業系統中管理資料和作業的基礎結構。 您可以撰寫 WMI 腳本或應用程式，將遠端電腦上的系統管理工作自動化。 WMI 也會提供管理資料給作業系統的其他部分和產品。 SPA 使用 WMI 類別資訊和資料點做為產生效能報表的來源。

**效能計數器**

效能計數器的用途是提供作業系統或應用程式、服務或驅動程式的執行狀況優劣相關資訊。 效能計數器資料有助於判斷系統瓶頸並微調系統和應用程式效能。 作業系統、網路和裝置提供應用程式可取用的計數器資料，讓使用者能夠以圖形方式查看系統的執行狀況。 SPA 使用效能計數器資訊和資料點做為來源，以產生效能報告。

**效能記錄檔及警示**

效能記錄檔及警示 (PLA) 是 Windows 作業系統中的內建服務。 它是設計用來收集效能記錄和追蹤，而且在符合特定觸發程式時也會引發效能警示。 PLA 可以用來收集效能計數器、Windows 的事件追蹤 (ETW) 、WMI 查詢、登錄機碼和設定檔案。 PLA 也支援透過遠端程序呼叫， (RPC) 進行遠端資料收集。 使用者定義資料收集器集合程式，其中包括要收集之資料的相關資訊、資料收集的頻率、資料收集持續時間、篩選，以及儲存結果檔案的位置。 SPA 使用 PLA 收集目標伺服器的所有效能資料。

**單一報表**

單一報表是根據單一目標伺服器上一個 advisor 套件的一個資料分析會話所產生的 SPA 報告。 它可以包含通知和各種資料區段。

**並存報表**

並列報表是一份 SPA 報表，可比較相同 advisor 套件的兩個單一報表。 這兩份報告可以從不同的目標伺服器產生，或從相同目標伺服器上的個別效能分析執行。 並存報表會建立比較兩份報告的功能，以協助使用者識別其中一個報表中的異常行為或設定。 並列報表包含通知和各種資料區段。 在每個區段中，這兩個報表的資料會並存列出。

**趨勢圖**

趨勢圖是用來調查效能問題重複模式的 SPA 報表。 許多重複的效能問題是因為排程的伺服器從伺服器或用戶端電腦載入變更所造成，而這可能會在每日或每週發生。 SPA 提供24小時的趨勢圖，以及7天的趨勢圖來識別這些問題。

使用者可以一次選擇一或多個資料數列，也就是單一報表內的數值，例如 **平均 CPU 使用率總計**。 更明確地說，數值是單一伺服器的純量值，該伺服器是由指定時間實例的單一 AP 所產生。 SPA 會將這些值分組到24個群組中，一天的每個小時會有一個 (七天的報表，每一周的每一天) 。 SPA 會計算每個群組的平均、最小值、最大值及標準差。

**歷程記錄圖表**

歷程記錄圖表是 SPA 報表，可用來顯示給定伺服器和 advisor 套件組在一段時間內特定數值的變更。 使用者可以選擇多個資料數列，並在歷程記錄圖表中一起顯示，以瞭解不同資料數列之間的相互關聯。

**資料數列**

資料數列是一段時間內從相同資料來源收集的數值資料。 相同的來源表示資料必須來自相同的目標伺服器，例如在一部伺服器上的 IIS 平均要求佇列長度。

**規則**

規則是邏輯、閾值和描述的組合。 它們代表潛在的效能問題。 每個 advisor 套件包含多個規則。 每個規則都是由報表產生進程所觸發。 規則會將邏輯和臨界值套用至單一報表中的資料。 如果符合條件，則會引發警告通知。 如果沒有，則會將通知設定為 [ **確定]** 狀態。 如果規則不適用，則會將通知設定為不適用的 (**NA**) 狀態。

**通知**

通知是規則顯示給使用者的資訊。 它包含規則的狀態 (**[確定]、[** **NA**] 或 [ **警告** ]) 、規則的名稱，以及可解決效能問題的可能建議。
