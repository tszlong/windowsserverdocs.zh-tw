---
title: bcdboot
description: 適用于**bcdboot**的 Windows 命令主題-快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383445"
---
# <a name="bcdboot"></a>bcdboot



可讓您快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。 系統磁碟分割是藉由將一組簡單的開機設定資料（BCD）檔案複製到現有的空白磁碟分割來設定。

如需 BCDboot 的詳細資訊，包括有關如何尋找 BCDboot 的資訊，以及如何使用此命令的範例，請參閱[BCDboot 命令列選項](https://technet.microsoft.com/library/hh824874.aspx)主題。

## <a name="syntax"></a>語法

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|來源|指定要做為複製開機環境檔案之來源的 Windows 目錄位置。|
|/l|指定地區設定。 預設地區設定為美式英文。|
|/s|指定系統磁碟分割的磁片區代號。 預設值是由固件識別的系統磁碟分割。|

## <a name="BKMK_examples"></a>典型

如需如何使用此命令的更多範例，請參閱[BCDboot 命令列選項](https://technet.microsoft.com/library/hh824874.aspx)主題。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)