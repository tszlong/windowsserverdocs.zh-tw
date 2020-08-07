---
title: 非使用中
description: 非使用中命令的參考文章，會將基本主開機記錄的系統磁碟分割或開機磁碟分割標示為非作用中， (MBR) 磁片。
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e0c1a1bf56212da139fe03b74c10ebc4488d8e5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888336"
---
# <a name="inactive"></a>非使用中

在基本主開機記錄 (MBR) 磁片上，將具有焦點的系統磁碟分割或開機磁碟分割標示為非作用中。

必須選取使用中的系統或開機磁碟分割，此操作才能成功。 使用 [[選取資料分割命令](select-partition.md)] 命令來選取使用中的磁碟分割，並將焦點移至該資料分割。

> [!CAUTION]
> 如果沒有使用中的磁碟分割，電腦可能無法啟動。 請不要將系統或開機磁碟分割標記為非使用中，除非您是有經驗的使用者徹底瞭解 Windows 系列的作業系統。<p>如果您在將系統或開機磁碟分割標示為非作用中之後無法啟動電腦，請將 Windows 安裝程式 CD 插入 CD-ROM 光碟機、重新開機電腦，然後使用修復主控台中的**fixmbr**和**fixboot**命令修復磁碟分割。
>
> 將系統磁碟分割或開機磁碟分割標記為非使用中之後，您的電腦就會從 BIOS 中指定的下一個選項（例如，CD-ROM 光碟機或開機前執行環境）開始， (PXE) 。

## <a name="syntax"></a>語法

```
inactive
```

### <a name="examples"></a>範例

```
inactive
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

- [Windows 開機問題進階疑難排解](/windows/client-management/advanced-troubleshooting-boot-problems)
