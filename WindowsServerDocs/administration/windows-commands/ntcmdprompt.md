---
title: ntcmdprompt
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25afab00ced3cb14771c18aa38c7fd8c98aecc0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838041"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

執行 [終止並保持常駐（TSR）] 或 [從 MS-DOS 應用程式中啟動命令提示字元] 之後，執行命令直譯器**cmd.exe**，而不是**Command.com**。
## <a name="syntax"></a>語法
```
ntcmdprompt
```
#### <a name="parameters"></a>參數

| 參數 |             描述              |
|-----------|--------------------------------------|
|    /?     | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註
- 當**Command.com**正在執行時， **cmd.exe**的某些功能（例如，命令歷程記錄的**doskey**顯示）無法使用。 如果您想要在啟動終止並保持常駐（TSR）或從以 MS-DOS 為基礎的應用程式中啟動命令提示字元之後執行**cmd.exe**命令直譯器，可以使用**ntcmdprompt**命令。 不過，請記住，當您執行**cmd.exe**時，TSR 可能無法供使用。 您可以在您的**config.xml**檔案中包含**ntcmdprompt**命令，或在應用程式的程式資訊檔案（Pif）中加入對等的自訂啟動檔案。
  ## <a name="examples"></a>範例
  若要在您的**config.xml**檔案中包含**ntcmdprompt** ，或在 Pif 中指定設定啟動檔案，請輸入： **ntcmdprompt**
  ## <a name="additional-references"></a>其他參考資料
- - [命令列語法關鍵](command-line-syntax-key.md)

