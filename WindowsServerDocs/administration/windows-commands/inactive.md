---
title: 非使用中
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6642288385571b00c3fd0094dcd6cc4237aa492e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812919"
---
# <a name="inactive"></a>非使用中



在基本主要開機記錄 (MBR) 磁碟，標示的系統磁碟分割或開機磁碟分割具有焦點為非作用中。

## <a name="syntax"></a>語法

```
inactive
```

## <a name="remarks"></a>備註

> [!CAUTION]
> 您的電腦可能無法啟動，而不需要作用中磁碟分割。 除非您是有經驗的使用者具有全盤瞭解的 Windows 系列作業系統，請不要將標示為非作用中的系統或開機磁碟分割。</br>> 如果您無法啟動電腦後將標示為非作用中的系統或開機磁碟分割，將 Windows 安裝光碟插入 CD-ROM 光碟機中，重新啟動電腦，然後修復磁碟分割使用**fixmbr**並**fixboot**修復主控台 中的命令。
-   您將標記為非作用中的開機磁碟分割的系統磁碟分割之後，您的電腦會啟動從 BIOS，例如 CD-ROM 光碟機或 Pre-boot eXecution Environment (PXE) 中指定的 [下一步] 選項。
-   這項作業成功時，必須選取作用中的系統或開機磁碟分割。 使用**選取資料分割**命令來選取作用中的資料分割，並將焦點移到它。

## <a name="BKMK_examples"></a>範例

```
inactive
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

