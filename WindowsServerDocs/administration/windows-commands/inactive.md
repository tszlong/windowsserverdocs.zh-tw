---
title: inactive
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a4f731bef9515a387b0343eaf4cb06142e6620
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842121"
---
# <a name="inactive"></a>inactive



在基本主要開機記錄 (MBR) 磁碟上，將具有焦點的系統磁碟分割或開機磁碟分割標記為非使用中。

## <a name="syntax"></a>語法

```
inactive
```

## <a name="remarks"></a>備註

> [!CAUTION]
> 如果沒有使用中的磁碟分割，電腦可能無法啟動。 請不要將系統或開機磁碟分割標記為非使用中，除非您是有經驗的使用者徹底瞭解 Windows 系列的作業系統。</br>> 如果您在將系統或開機磁碟分割標示為非作用中之後無法啟動電腦，請將 Windows 安裝程式 CD 插入 CD-ROM 光碟機中，重新開機電腦，然後使用修復主控台中的**fixmbr**和**fixboot**命令修復磁碟分割。
> -   將系統磁碟分割或開機磁碟分割標記為非使用中之後，您的電腦會從 BIOS 中指定的下一個選項（例如，CD-ROM 光碟機或開機前執行環境（PXE））開始。
> -   必須選取使用中的系統或開機磁碟分割，此操作才能成功。 使用 [**選取資料分割**] 命令來選取使用中的資料分割，並將焦點移至其上。

## <a name="examples"></a><a name=BKMK_examples></a>典型

```
inactive
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

