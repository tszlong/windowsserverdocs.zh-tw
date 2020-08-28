---
title: pushprinterconnections
description: Pushprinterconnections.exe 命令的參考文章，可從群組原則讀取已部署的印表機連線設定，並視需要部署/移除印表機連接。
ms.topic: reference
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41ed8af3b4d70058887de10215f36e1aa530e912
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032368"
---
# <a name="pushprinterconnections"></a>pushprinterconnections

從群組原則讀取已部署的印表機連線設定，並視需要部署/移除印表機連接。

> [!IMPORTANT]
> 此公用程式可用於電腦啟動或使用者登入腳本，而不應該從命令列執行。

## <a name="syntax"></a>語法

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| <-記錄> | 將每個使用者的偵測記錄檔寫入 *% temp*，或將每部機器的 debug 記錄寫入 *%windir%\temp*。 |
| <-？ > | 在命令提示字元顯示 [說明]。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)

- [使用群組原則部署印表機](https://go.microsoft.com/fwlink/?LinkId=230627)
