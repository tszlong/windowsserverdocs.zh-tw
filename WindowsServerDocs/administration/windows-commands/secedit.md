---
title: secedit
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8aaa40f21c6f14dc7d686261e9980594c14a8032
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818459"
---
# <a name="secedit"></a>secedit



設定，並藉由比較您目前的組態，以指定的安全性範本來分析系統安全性。

## <a name="syntax"></a>語法

```
secedit 
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|可讓您分析目前的系統設定，針對儲存在資料庫的基準設定。  分析結果會儲存在資料庫的不同區域，而且您可以在 安全性設定及分析嵌入式管理單元檢視。|
|[Secedit： 設定](secedit-configure.md)|可讓您使用儲存在資料庫中的安全性設定來設定系統。|
|[Secedit:export](secedit-export.md)|可讓您匯出儲存在資料庫中的安全性設定。|
|[Secedit:generaterollback](secedit-generaterollback.md)|可讓您產生復原範本相關的組態範本。|
|[Secedit:import](secedit-import.md)|可讓您匯入的安全性範本資料庫，因此可以套用至系統或系統針對分析範本中指定的設定。|
|[Secedit： 驗證](secedit-validate.md)|可讓您驗證的安全性範本的語法。|

## <a name="remarks"></a>備註

針對所有的檔案名稱，如果未指定路徑，會使用目前的目錄。

當安全性範本會建立使用安全性範本嵌入式管理單元和安全性設定並執行分析 嵌入式管理單元時，會建立下列檔案：

|檔案|描述|
|----|-----------|
|Scesrv.log|**位置**: %windir%\security\logs</br>**藉由建立**： 作業系統</br>**檔案類型**： 文字</br>**重新整理的頻率**:覆寫時 secedit / 分析 / 設定/匯出或 /import 會執行。</br>**內容**：包含原則類型分組的分析的結果。|
|*使用者選取名稱*.sdb|**位置**: %windir%\*使用者帳戶 * \Documents\Security\Database</br>**藉由建立**： 安全性設定及分析嵌入式管理單元執行</br>**檔案類型**： 專屬</br>**重新整理的頻率**:每當建立新的安全性範本時，就會更新。</br>**內容**：本機安全性原則和使用者建立的安全性範本。|
|*使用者選取名稱*.log|**位置**：使用者定義，但預設值為 %windir%\*使用者帳戶 * \Documents\Security\Logs</br>**建立者**：執行 / 分析和 / 設定子命令 （或使用 安全性設定及分析嵌入式管理單元）</br>**檔案類型**： 文字</br>**重新整理的頻率**:執行 / 分析和 / 設定子命令 （或使用 安全性設定及分析嵌入式管理單元）;覆寫。</br>**內容**：</br>1.記錄檔名稱</br>2.日期和時間</br>3.分析或調查的結果。|
|*使用者選取名稱*.inf|**位置**: %windir%\*使用者帳戶 * \Documents\Security\Templates</br>**藉由建立**： 安全性範本嵌入式管理單元執行</br>**檔案類型**： 文字</br>**重新整理的頻率**： 每次更新時的安全性範本</br>**內容**：包含針對每個嵌入式管理單元使用選取的原則範本的資訊設定。|

> [!NOTE]
> Microsoft Management Console (MMC) 和 安全性設定及分析嵌入式管理單元並不適用於 Server Core。

#### <a name="additional-references"></a>其他參考資料

如需如何使用此命令的範例，請參閱 < 範例 > 一節中的任何子命令的檔案。
-   [命令列語法關鍵](command-line-syntax-key.md)