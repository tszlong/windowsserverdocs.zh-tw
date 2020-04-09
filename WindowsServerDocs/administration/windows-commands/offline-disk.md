---
title: 離線磁片
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7991f1f5967970690c7051612395fb47a764ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837981"
---
# <a name="offline-disk"></a>離線磁片



將具有焦點的線上磁片帶到離線狀態。

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此 DiskPart 命令。

## <a name="syntax"></a>語法

```
offline disk [noerr]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   此命令會在處於 SAN 線上模式的磁片上操作。 它會將其 SAN 模式變更為離線。
-   如果磁片群組中的動態磁碟已離線，則磁片的狀態會變更為 [**遺失**]，而群組會顯示離線的磁片。 遺失的磁片會移至不正確群組。 如果動態磁碟是群組中的最後一個磁片，則磁片的狀態會變更為 [**離線**]，而空的群組則會被移除。
-   必須選取磁片，[**離線磁片**] 命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要讓磁片具有離線焦點，請輸入：
```
offline disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

