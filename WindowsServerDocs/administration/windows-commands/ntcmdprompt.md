---
title: ntcmdprompt
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fef1641bf1b48bd1fe4aaf284ed309ab4d4d5f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372675"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行 [終止並保持常駐（TSR）] 或 [從 MS-DOS 應用程式中啟動命令提示字元] 之後，執行命令直譯器**cmd.exe**，而不是**Command.com**。
## <a name="syntax"></a>語法
```
ntcmdprompt
```
### <a name="parameters"></a>參數

| 參數 |             描述              |
|-----------|--------------------------------------|
|    /?     | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註
- 當**Command.com**正在執行時， **cmd.exe**的某些功能（例如，命令歷程記錄的**doskey**顯示）無法使用。 如果您想要在啟動終止並保持常駐（TSR）或從以 MS-DOS 為基礎的應用程式中啟動命令提示字元之後執行**cmd.exe**命令直譯器，可以使用**ntcmdprompt**命令。 不過，請記住，當您執行**cmd.exe**時，TSR 可能無法供使用。 您可以在您的**config.xml**檔案中包含**ntcmdprompt**命令，或在應用程式的程式資訊檔案（Pif）中加入對等的自訂啟動檔案。
  ## <a name="examples"></a>範例
  若要在您的**config.xml**檔案中包含**ntcmdprompt** ，或在 Pif 中指定設定啟動檔案，請輸入： **ntcmdprompt**
  ## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)

