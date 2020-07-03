---
title: pushprinterconnections
description: Pushprinterconnections.exe 命令的參考文章，它會從群組原則讀取已部署的印表機連線設定，並視需要部署/移除印表機連接。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b22fd5143a9477b40a515df44c9a0b5663dfd7a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931382"
---
# <a name="pushprinterconnections"></a>pushprinterconnections

從群組原則讀取已部署的印表機連線設定，並視需要部署/移除印表機連接。

> [!IMPORTANT]
> 此公用程式可用於電腦啟動或使用者登入腳本，不應從命令列執行。

## <a name="syntax"></a>語法

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| <-記錄> | 將每個使用者的 debug 記錄檔寫入 *% temp*，或將每個機器的 debug 記錄檔寫入 *%windir%\temp*。 |
| <-？ > | 在命令提示字元顯示 [說明]。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)

- [使用群組原則部署印表機](https://go.microsoft.com/fwlink/?LinkId=230627)
