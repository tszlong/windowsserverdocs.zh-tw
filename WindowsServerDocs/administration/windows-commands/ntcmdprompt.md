---
title: ntcmdprompt
description: Ntcmdprompt 命令的參考文章，它會在執行 Terminate 並保持常駐 (TSR) 或從 MS-DOS 應用程式中啟動命令提示字元之後，執行命令直譯器 **Cmd.exe**（而不是 **Command.com**）。
ms.topic: reference
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557db55ea99b209f2bca152f1e45622aa0df4887
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032756"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行終止並保持常駐 (TSR) 或從 MS-DOS 應用程式中啟動命令提示字元之後，執行命令直譯器 **Cmd.exe**（而不是 **Command.com**）。

## <a name="syntax"></a>語法

```
ntcmdprompt
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當 **Command.com** 正在執行時，某些 **Cmd.exe**的功能（例如命令歷程記錄的 **doskey** 顯示）無法使用。 如果您想要在啟動結束時執行 **Cmd.exe** 命令直譯器，並保持常駐 (TSR) 或從以 MS-DOS 為基礎的應用程式中啟動命令提示字元，您可以使用 **ntcmdprompt** 命令。 不過，請記住，當您執行 **Cmd.exe**時，TSR 可能無法使用。 您可以在您的**app.config**檔案中包含**ntcmdprompt**命令，或在應用程式的程式資訊檔中包含對等的自訂啟動檔案 (Pif) 。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
