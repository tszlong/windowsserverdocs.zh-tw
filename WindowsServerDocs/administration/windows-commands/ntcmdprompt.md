---
title: ntcmdprompt
description: Ntcmdprompt 命令的參考文章，它會在執行終止並保持常駐 (TSR) 或從 MS-DOS 應用程式中啟動命令提示字元之後，執行命令直譯器**Cmd.exe**，而不是**Command.com**。
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 53ed39f0fd455314dc5ccd90f0286f6dfd35ea72
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885299"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行終止並保持常駐 (TSR) 或從 MS-DOS 應用程式中啟動命令提示字元之後，執行命令直譯器**Cmd.exe**，而不是**Command.com**。

## <a name="syntax"></a>語法

```
ntcmdprompt
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當**Command.com**正在執行時， **Cmd.exe**的某些功能（例如，命令歷程記錄的**doskey**顯示）無法使用。 如果您想要在啟動終止並保持常駐 (TSR) 或從以 MS-DOS 為基礎的應用程式中啟動命令提示字元之後，執行**Cmd.exe**命令直譯器，您可以使用**ntcmdprompt**命令。 不過，請記住，當您執行**Cmd.exe**時，TSR 可能無法供使用。 您可以將**ntcmdprompt**命令包含在**app.config**檔案中，或在應用程式的程式資訊檔案中加入對等的自訂啟動檔案 (Pif) 。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
