---
title: bcdboot
description: 適用於 Windows 命令主題**bcdboot** -快速設定一個系統分割區，或修復位於系統磁碟分割上的開機環境。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825879"
---
# <a name="bcdboot"></a>bcdboot



可讓您快速地設定系統磁碟分割設定或修復位於系統磁碟分割上的開機環境。 系統磁碟分割設定，將一組簡單的開機設定資料 (BCD) 檔案複製到現有的空白資料分割。

如需有關 BCDboot，包括有關如何尋找 BCDboot 和範例，示範如何使用此命令，請參閱[BCDboot 命令列選項](https://technet.microsoft.com/library/hh824874.aspx)主題。

## <a name="syntax"></a>語法

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|來源|指定要複製的開機環境檔案做為來源的 Windows 目錄的位置。|
|/l|指定的地區設定。 預設的地區設定是英文 （美國）。|
|/s|指定系統磁碟分割的磁碟區的代號。 預設值為韌體所識別的系統磁碟分割。|

## <a name="BKMK_examples"></a>範例

如需如何使用此命令的範例，請參閱[BCDboot 命令列選項](https://technet.microsoft.com/library/hh824874.aspx)主題。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)