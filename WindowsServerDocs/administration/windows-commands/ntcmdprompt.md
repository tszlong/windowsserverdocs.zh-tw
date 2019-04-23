---
title: ntcmdprompt
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 65804f99095d0c0a56537b1d155ac26e768f61a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827659"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

執行命令解譯器**Cmd.exe**，而非**Command.com**、 之後執行的終止和保持常駐 (TSR)，或在啟動命令提示字元從 MS-DOS 應用程式中。
## <a name="syntax"></a>語法
```
ntcmdprompt
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   當**Command.com**正在執行的某些功能**Cmd.exe**，這類**doskey**顯示的命令歷程記錄，未提供。 如果您想要執行**Cmd.exe**命令直譯器啟動終止 」 與 「 保持常駐 (TSR) 或啟動命令提示字元從 MS-DOS 為基礎的應用程式內之後，您可以使用**ntcmdprompt**命令。 不過，請記住，TSR 可能無法供使用時您正在**Cmd.exe**。 您可以包含**ntcmdprompt**命令，在您**Config.nt**檔案或應用程式的程式資訊檔 (Pif) 中，對等的自訂啟動檔案。
## <a name="examples"></a>範例
若要包含**ntcmdprompt**中您**Config.nt**檔案或 Pif，型別中指定的組態啟動檔案： **ntcmdprompt**
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

