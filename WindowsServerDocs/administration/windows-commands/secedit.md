---
title: secedit 命令
description: Secedit 命令的參考文章，可比較您目前的安全性設定與指定的安全性範本。
ms.topic: reference
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ceb1a28376c17ab9d08689c7b0367dd90fdecc4f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388285"
---
# <a name="secedit-commands"></a>secedit 命令

藉由比較您目前的安全性設定與指定的安全性範本，來設定和分析系統安全性。

> [!NOTE]
> 在 Server Core 上無法使用 Microsoft Management Console (MMC) 和安全性設定及分析嵌入式管理單元。

## <a name="syntax"></a>語法

```
secedit /analyze
secedit /configure
secedit /export
secedit /generaterollback
secedit /import
secedit /validate
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [secedit/analyze](secedit-analyze.md) | 可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。  分析結果會儲存在資料庫的不同區域中，而且可以在 [安全性設定和分析] 嵌入式管理單元中查看。 |
| [secedit/configure](secedit-configure.md) | 可讓您使用儲存在資料庫中的安全性設定來設定系統。 |
| [secedit/export](secedit-export.md) | 可讓您匯出儲存在資料庫中的安全性設定。 |
| [secedit/generaterollback](secedit-generaterollback.md) | 可讓您產生與設定範本相關的復原範本。 |
| [secedit/import](secedit-import.md) | 可讓您將安全性範本匯入資料庫，以便將範本中指定的設定套用至系統，或針對系統進行分析。 |
| [secedit/validate](secedit-validate.md) | 可讓您驗證安全性範本的語法。 |

#### <a name="remarks"></a>備註

- 如果未指定檔案路徑，所有檔案名都會預設為目前的目錄。

- 您的分析結果會儲存在資料庫的另一個區域中，並可在 [安全性設定及分析] 嵌入式管理單元中看到 MMC。

- 如果您的安全性範本是使用 [安全性範本] 嵌入式管理單元建立的，而且如果您針對這些範本執行 [安全性設定及分析] 嵌入式管理單元，則會建立下列檔案：

    | 檔案 | 描述 |
    |--|--|
    | scesrv.dll .log | <ul><li>**位置：**`%windir%\security\logs`</li><li>**建立者：** 作業系統</li><li>**檔案類型：** 文本</li><li>**更新頻率：** 在 `secedit analyze` 執行、或時覆寫 `secedit configure` `secedit export` `secedit import` 。</li><li>**內容：** 包含依原則類型分組的分析結果。</li></ul> |
    | *使用者選取的名稱*。 sdb | <ul><li>**位置：**`%windir%\<user account>\Documents\Security\Database`</li><li>**建立者：** 執行安全性設定及分析嵌入式管理單元</li><li>**檔案類型：** 專有</li><li>**更新頻率：** 每次建立新的安全性範本時更新。</li><li>**內容：** 本機安全性原則和使用者建立的安全性範本。</li></ul> |
    | *使用者選取的名稱*。記錄檔 | <ul><li>**位置：** 使用者定義，但預設為 `%windir%\<user account>\Documents\Security\Logs`</li><li>**建立者：** 執行 `secedit analyze` 或 `secedit configure` 命令，或使用 [安全性設定及分析] 嵌入式管理單元。</li><li>**檔案類型：** 文本</li><li>**更新頻率：** 在執行或時覆寫 `secedit analyze` `secedit configure` ，或使用 [安全性設定及分析] 嵌入式管理單元進行覆寫。</li><li>**內容：** 記錄檔名稱、日期和時間，以及分析或調查的結果。</li></ul> |
    | *使用者選取的名稱*.inf | <ul><li>**位置：**`%windir%\*<user account>\Documents\Security\Templates`</li><li>**建立者：** 正在執行安全性範本嵌入式管理單元。</li><li>**檔案類型：** 文本</li><li>**更新頻率：** 每次更新安全性範本時覆寫。</li><li>**內容：** 包含使用嵌入式管理單元選取的每個原則之範本的設定資訊。</li></ul> |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
