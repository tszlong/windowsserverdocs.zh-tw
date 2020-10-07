---
title: 適用於行動寬頻網路 (MBN) 的 Netsh 命令
description: 使用 netsh mbn 來查詢及設定行動寬頻設定和參數。
ms.topic: article
author: apdutta
ms.author: apdutta
ms.date: 02/20/2020
ms.openlocfilehash: 926e28cff1815e5f6a82185ad78a3825ed188def
ms.sourcegitcommit: ad2c13b09044710bf14236308ade8d74877c3e0d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2020
ms.locfileid: "91591158"
---
# <a name="netsh-mbn-commands"></a>Netsh mbn 命令


使用 **netsh mbn** 來查詢及設定行動寬頻設定和參數。

> [!TIP]
> 若要取得有關 netsh mbn 命令的協助，請使用：
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh mbn /?

可用的 netsh mbn 命令如下：

- [add](#add)
- [connect](#connect)
- [delete](#delete)
- [disconnect](#disconnect)
- [diagnose](#diagnose)
- [dump](#dump)
- [說明](#help)
- [set](#set)
- [show](#show)
- [測試](#test)

## <a name="add"></a>新增

將組態項目新增到資料表。

可用的 netsh mbn add 命令如下：

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

在設定檔資料存放區中新增 DM 組態設定檔。

**語法**

```powershell
add dmprofile [interface=]<string> [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 設定檔 XML 檔案的名稱。 這是包含設定檔資料的 XML 檔案名稱。     | 必要 |


**範例**

```powershell
add dmprofile interface="Cellular" name="Profile1.xml"
```


### <a name="profile"></a>profile

在設定檔資料存放區中新增網路設定檔。

**語法**

```powershell
add profile [interface=]<string> [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 設定檔 XML 檔案的名稱。 這是包含設定檔資料的 XML 檔案名稱。     | 必要 |


**範例**

```powershell
add profile interface="Cellular" name="Profile1.xml"
```


## <a name="connect"></a>connect

連線到行動寬頻網路。

**語法**

```powershell
connect [interface=]<string> [connmode=]tmp|name [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **connmode**  | 指定連線參數的提供方式。 使用設定檔 XML，或使用先前使用 "netsh mbn add profile" 命令儲存在行動寬頻設定檔資料存放區中的設定檔 XML 的設定檔名稱，可以要求連線。 在先前的情況下，connmode 參數應將 "tmp" 保留為值。 不過，其在後來的情況下應該是 "name"                                       | 必要 |
| **name**      | 設定檔 XML 檔案的名稱。 這是包含設定檔資料的 XML 檔案名稱。     | 必要 |


**範例**

```powershell
connect interface="Cellular" connmode=tmp name=d:\profile.xml
connect interface="Cellular" connmode=name name=MyProfileName
```


## <a name="delete"></a>[刪除]

刪除資料表中的組態項目。

可用的 netsh mbn delete 命令如下：

- [dmprofile](#dmprofile)
- [profile](#profile)

### <a name="dmprofile"></a>dmprofile

刪除設定檔資料存放區中的 DM 組態設定檔。

**語法**

```powershell
delete dmprofile [interface=]<string> [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 設定檔 XML 檔案的名稱。 這是包含設定檔資料的 XML 檔案名稱。     | 必要 |

**範例**

```powershell
delete dmprofile interface="Cellular" name="myProfileName"
```

### <a name="profile"></a>profile

刪除設定檔資料存放區中的網路設定檔。

**語法**

```powershell
delete profile [interface=]<string> [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 設定檔 XML 檔案的名稱。 這是包含設定檔資料的 XML 檔案名稱。     | 必要 |


**範例**

```powershell
delete profile interface="Cellular" name="myProfileName"
```

## <a name="diagnose"></a>diagnose

執行基本行動電話問題的診斷。

**語法**

```powershell
diagnose [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
diagnose interface="Cellular"
```


## <a name="disconnect"></a>中斷連線

中斷與行動寬頻網路的連線。

**語法**

```powershell
disconnect [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
disconnect interface="Cellular"
```


## <a name="dump"></a>dump

顯示組態指令碼。

建立包含目前組態的設定檔。  如果儲存到檔案中，此指令碼即可用來還原已變更的組態設定。

**語法**

```powershell
dump
```

## <a name="help"></a>help

顯示命令清單。

**語法**

```powershell
help
```

## <a name="set"></a>設定

設定組態資訊。

可用的 netsh mbn set 命令如下：

- [acstate](#acstate)
- [dataenablement](#dataenablement)
- [dataroamcontrol](#dataroamcontrol)
- [enterpriseapnparams](#enterpriseapnparams)
- [highestconncategory](#highestconncategory)
- [powerstate](#powerstate)
- [profileparameter](#profileparameter)
- [slotmapping](#slotmapping)
- [tracing](#tracing)

### <a name="acstate"></a>acstate

針對指定的介面設定行動寬頻資料自動連線狀態。

**語法**

```powershell
set acstate [interface=]<string> [state=]autooff|autoon|manualoff|manualon
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 要設定的自動連線狀態。 下列其中一個值：<br>autooff：關閉自動連線權杖。<br>autoon：開啟自動連線權杖。<br>manualoff：關閉手動連線權杖。<br>manualon：開啟手動連線權杖。 | 必要 |


**範例**

```powershell
set acstate interface="Cellular" state=autoon
```


### <a name="dataenablement"></a>dataenablement

針對指定的設定檔集和介面，開啟或關閉行動寬頻資料。

**語法**

```powershell
set dataenablement [interface=]<string> [profileset=]internet|mms|all [mode=]yes|no
```

**參數**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **profileset** | 設定檔集的名稱。                                                                      | 必要 |
| **mode**       | 下列其中一個值：<br>yes：啟用目標設定檔集。<br>否：停用目標設定檔集。| 必要 |


**範例**

```powershell
set dataenablement interface="Cellular" profileset=mms mode=yes
```


### <a name="dataroamcontrol"></a>dataroamcontrol

針對指定的設定檔集和介面，設定行動寬頻資料漫遊控制。

**語法**

```powershell
set dataroamcontrol [interface=]<string> [profileset=]internet|mms|all [state=]none|partner|all
```

**參數**

|                |                                                                                               |          |
|----------------|-----------------------------------------------------------------------------------------------|----------|
| **interface**  | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **profileset** | 設定檔集的名稱。                                                                      | 必要 |
| **mode**       | 下列其中一個值：<br>none：僅限家用電信業者。<br>partner：僅限家用和合作夥伴電信業者。<br>all：家用、合作夥伴和漫遊電信業者。| 必要 |


**範例**

```powershell
set dataroamcontrol interface="Cellular" profileset=mms mode=partner
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

針對指定的介面設定行動寬頻資料 enterpriseAPN 參數。

**語法**

```powershell
set enterpriseapnparams [interface=]<string> [allowusercontrol=]yes|no|nc [allowuserview=]yes|no|nc [profileaction=]add|delete|modify|nc
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **allowusercontrol** | 下列其中一個值：<br>yes：允許終端使用者控制 enterpriseAPN。<br>no：不允許終端使用者控制 enterpriseAPN。<br>nc：沒有變更。 | 必要 |
| **allowuserview** |下列其中一個值：<br>yes：允許終端使用者檢視 enterpriseAPN。<br>no：不允許終端使用者檢視 enterpriseAPN。<br>nc：沒有變更。 | 必要 |
| **profileaction** | 下列其中一個值：<br>add：只會新增 enterpriseAPN 設定檔。<br>delete：只會刪除 enterpriseAPN 設定檔。<br>modify：只會修改 enterpriseAPN 設定檔。<br>nc：沒有變更。 | 必要 |


**範例**

```powershell
set enterpriseapnparams interface="Cellular" profileset=mms mode=yes
```


### <a name="highestconncategory"></a>highestconncategory

針對指定的介面設定行動頻寬資料最高連線類別。

**語法**

```powershell
set highestconncategory [interface=]<string> [highestcc=]admim|user|operator|device
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **highestcc** | 下列其中一個值：<br>admin：系統管理員佈建的設定檔。<br>user：使用者佈建的設定檔。<br>operator：操作員佈建的設定檔。<br>device：裝置佈建的設定檔。 | 必要 |


**範例**

```powershell
set highestconncategory interface="Cellular" highestcc=operator
```


### <a name="powerstate"></a>powerstate

針對指定的介面開啟或關閉行動寬頻無線電。

**語法**

```powershell
set powerstate [interface=]<string> [state=]on|off
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **state**      | 下列其中一個值：<br>on：開啟無線電收發器電源。<br>off：關閉無線電收發器電源。 | 必要 |


**範例**

```powershell
set powerstate interface="Cellular" state=on
```


### <a name="profileparameter"></a>profileparameter

在行動寬頻網路設定檔中設定參數。

**語法**

```powershell
set profileparameter [name=]<string> [[interface=]<string>] [[cost]=default|unrestricted|fixed|variable]
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | 要修改的設定檔名稱。 若已指定介面，則只會修改該介面上的設定檔。 | 必要 |
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 選用 |
| **cost**      | 與設定檔相關聯的成本。                                                             | 選用 |


**備註**

必須在介面名稱和成本之間至少指定一個參數。


**範例**

```powershell
set profileparameter name="profile 1" cost=default
```


### <a name="slotmapping"></a>slotmapping

針對指定的介面設定行動寬頻數據機插槽對應。

**語法**

```powershell
set slotmapping [interface=]<string> [slotindex=]<integer>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **slotindex** | 要設定的插槽索引。                                                                         | 必要 |


**範例**

```powershell
set slotmapping interface="Cellular" slotindex=1
```


### <a name="tracing"></a>tracing

啟用或停用追蹤。

**語法**

```powershell
set tracing [mode=]yes|no
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **mode**      | 下列其中一個值：<br>yes：啟用行動寬頻的追蹤。<br>否：停用行動寬頻的追蹤。     | 必要 |


**範例**

```powershell
set tracing mode=yes
```

## <a name="show"></a>顯示

顯示行動寬頻網路資訊。

可用的 netsh mbn show 命令如下：

- [acstate](#acstate-1)
- [capability](#capability)
- [connection](#connection)
- [dataenablement](#dataenablement-1)
- [dataroamcontrol](#dataroamcontrol-1)
- [dmprofiles](#dmprofiles)
- [enterpriseapnparams](#enterpriseapnparams-1)
- [highestconncategory](#highestconncategory-1)
- [homeprovider](#homeprovider)
- [interfaces](#interfaces)
- [netlteattachinfo](#netlteattachinfo)
- [pin](#pin)
- [pinlist](#pinlist)
- [preferredproviders](#preferredproviders)
- [profiles](#profiles)
- [profilestate](#profilestate)
- [provisionedcontexts](#provisionedcontexts)
- [purpose](#purpose)
- [radio](#radio)
- [readyinfo](#readyinfo)
- [signal](#signal)
- [slotmapping](#slotmapping-1)
- [slotstatus](#slotstatus)
- [smsconfig](#smsconfig)
- [tracing](#tracing-1)
- [visibleproviders](#visibleproviders)

### <a name="acstate"></a>acstate

針對指定的介面顯示行動寬頻資料自動連線狀態。

**語法**

```powershell
show acstate [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show acstate interface="Cellular"
```


### <a name="capability"></a>capability

針對指定的介面顯示介面功能資訊。

**語法**

```powershell
show capability [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show capability interface="Cellular"
```


### <a name="connection"></a>connection

針對指定的介面顯示目前連線資訊。

**語法**

```powershell
show connection [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show connection interface="Cellular"
```


### <a name="dataenablement"></a>dataenablement

針對指定的介面顯示行動寬頻資料啟用狀態。

**語法**

```powershell
show dataenablement [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show dataenablement interface="Cellular"
```


### <a name="dataroamcontrol"></a>dataroamcontrol

針對指定的介面顯示行動寬頻資料漫遊控制狀態。

**語法**

```powershell
show dataroamcontrol [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show dataroamcontrol interface="Cellular"
```


### <a name="dmprofiles"></a>dmprofiles

顯示系統上設定的 DM 組態設定檔清單。

**語法**

```powershell
show dmprofiles [[name=]<string>] [[interface=]<string>]
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | 要顯示的設定檔名稱。                                                               | 選用 |
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 選用 |

**備註**

顯示設定檔資料，或列出系統上的設定檔。

若已指定設定檔名稱，則會顯示設定檔的內容。 否則會列出介面的設定檔。

若已指定介面名稱，則只會列出指定介面上指定的設定檔。 否則會顯示第一個相符的設定檔。

**範例**

```powershell
show dmprofiles name="profile 1" interface="Cellular"
show dmprofiles name="profile 2"
show dmprofiles
```


### <a name="enterpriseapnparams"></a>enterpriseapnparams

針對指定的介面顯示行動寬頻資料 enterpriseAPN 參數。

**語法**

```powershell
show enterpriseapnparams [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show enterpriseapnparams interface="Cellular"
```


### <a name="highestconncategory"></a>highestconncategory

針對指定的介面顯示行動頻寬資料最高連線類別。

**語法**

```powershell
show highestconncategory [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show highestconncategory interface="Cellular"
```


### <a name="homeprovider"></a>homeprovider

針對指定的介面顯示家用提供者資訊。

**語法**

```powershell
show homeprovider [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show homeprovider interface="Cellular"
```


### <a name="interfaces"></a>interfaces

顯示系統上的行動寬頻介面清單。 這個命令沒有參數。

**語法**

```powershell
show interfaces
```


### <a name="netlteattachinfo"></a>netlteattachinfo

針對指定的介面顯示行動寬頻網路 LTE 連結資訊。

**語法**

```powershell
show netlteattachinfo [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show netlteattachinfo interface="Cellular"
```

### <a name="pin"></a>釘選

針對指定的介面顯示 pin 資訊。

**語法**

```powershell
show pin [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show pin interface="Cellular"
```


### <a name="pinlist"></a>pinlist

針對指定的介面顯示 pin 清單資訊。

**語法**

```powershell
show pinlist [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show pinlist interface="Cellular"
```


### <a name="preferredproviders"></a>preferredproviders

針對指定的介面顯示慣用提供者清單。

**語法**

```powershell
show preferredproviders [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show preferredproviders interface="Cellular"
```


### <a name="profiles"></a>profiles

顯示系統上設定的設定檔清單。

**語法**

```powershell
show profiles [[name=]<string>] [[interface=]<string>] [[purpose=]<string>]
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **name**      | 要顯示的設定檔名稱。                                                               | 選用 |
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 選用 |
| **purpose**   | 用途 | 選用 |

**備註**

若已指定設定檔名稱，則會顯示設定檔的內容。 否則會列出介面的設定檔。

若已指定介面名稱，則只會列出指定介面上指定的設定檔。 否則會顯示第一個相符的設定檔。

若已提供目的，則只會顯示具有相符目的 GUID 的設定檔。  否則設定檔不會依照目的進行篩選。  字串可以是具有大括弧或下列其中一個字串的 GUID：internet、supl、mms、ims 或 allhost。

**範例**

```powershell
show profiles interface="Cellular" purpose="{00000000-0000-0000-0000-000000000000}"
show profiles name="profile 1" interface="Cellular"
show profiles name="profile 2"
show profiles
```

### <a name="profilestate"></a>profilestate

針對指定的介面顯示行動寬頻設定檔狀態。

**語法**

```powershell
show profilestate [interface=]<string> [name=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |
| **name**      | 設定檔的名稱。 這是具有要顯示狀態的設定檔名稱。            | 必要 |

**範例**

```powershell
show profilestate interface="Cellular" name="myProfileName"
```

### <a name="provisionedcontexts"></a>provisionedcontexts

針對指定的介面顯示佈建的內容資訊。

**語法**

```powershell
show provisionedcontexts [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show provisionedcontexts interface="Cellular"
```


### <a name="purpose"></a>purpose

顯示可用來篩選裝置上設定檔的目的群組 GUID。 這個命令沒有參數。

**語法**

```powershell
show purpose
```


### <a name="radio"></a>radio

針對指定的介面顯示無線電狀態資訊。

**語法**

```powershell
show radio [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show radio interface="Cellular"
```


### <a name="readyinfo"></a>readyinfo

針對指定的介面顯示就緒資訊。

**語法**

```powershell
show readyinfo [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show readyinfo interface="Cellular"
```


### <a name="signal"></a>signal

針對指定的介面顯示訊號資訊。

**語法**

```powershell
show signal [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show signal interface="Cellular"
```


### <a name="slotmapping"></a>slotmapping

針對指定的介面顯示行動寬頻數據機插槽對應。

**語法**

```powershell
show slotmapping [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show slotmapping interface="Cellular"
```


### <a name="slotstatus"></a>slotstatus

針對指定的介面顯示行動寬頻數據機插槽狀態。

**語法**

```powershell
show slotstatus [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show slotstatus interface="Cellular"
```


### <a name="smsconfig"></a>smsconfig

針對指定的介面顯示簡訊 (SMS) 組態資訊。

**語法**

```powershell
show smsconfig [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show smsconfig interface="Cellular"
```


### <a name="tracing"></a>tracing

顯示行動寬頻追蹤已啟用或已停用。

**語法**

```powershell
show tracing
```


### <a name="visibleproviders"></a>visibleproviders

針對指定的介面顯示可見提供者清單。

**語法**

```powershell
show visibleproviders [interface=]<string>
```

**參數**

|               |                                                                                               |          |
|---------------|-----------------------------------------------------------------------------------------------|----------|
| **interface** | 介面名稱。 這是 "netsh mbn show interfaces" 命令所顯示的其中一個介面名稱。 | 必要 |


**範例**

```powershell
show visibleproviders interface="Cellular"
```

## <a name="test"></a>test

在收集記錄時，執行特定功能區域的測試。

**語法**
```
test [feature=<feature area>] [testPath=<path>] [taefPath=<path>] [param=<test input params>]
```

**參數**

| 標籤 | 值 | 選擇性？ |
|---|---|---|
| **功能** | 下面列出支援功能區域中的功能區 | 必要 |
| **testpath** | 包含測試二進位檔的路徑 | 如果已安裝 HLK 伺服器，則為選擇性 |
| **taefpath** | 包含 TAEF 二進位檔的路徑 | 如果已安裝 HLK 伺服器，則為選擇性 |
| **param** | 以逗號分隔的參數，用於測試 | 對於特定功能區為必要性，對於其他功能區為選擇性 |

**備註**

支援的功能區為：
- 連線能力
- power
- radio
- esim
- sms
- dssa
- lte
- bringup

某些測試額外測試參數，這些參數必須在 `param` 欄位中提供。
以下列出功能的必要參數。
- **connectivity**：AccessString、UserName (如果適用)、Password (如果適用)
- **radio**：AccessString、UserName (如果適用)、Password (如果適用)
- **esim**：ActivationCode
- **bringup**：AccessString、UserName (如果適用)、Password (如果適用)

**範例**

```
test feature=connectivity param="AccessString=internet"
test feature=lte testpath="C:\\data\\test\\bin" taefpath="C:\\data\\test\\bin"
test feature=lte
```
