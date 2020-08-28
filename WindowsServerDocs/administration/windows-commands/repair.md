---
title: 修復
description: 修復命令的參考文章，此命令會藉由將失敗的磁片區域取代為指定的動態磁碟來修復 RAID-5 磁片區。
ms.topic: reference
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0768a82f2de22a424b4979aa8844e9fbb4f67e5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027286"
---
# <a name="repair"></a>修復

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由將失敗的磁片區域取代為指定的動態磁碟，修復具有焦點的 RAID-5 磁片區。

必須選取 RAID-5 陣列中的磁片區，這項作業才會成功。 使用 [ **選取磁片** 區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="syntax"></a>語法

```
repair disk=<n> [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 磁片 =`<n>` | 指定將取代失敗磁片區域的動態磁碟。 其中 *n* 必須有大於或等於 raid-5 磁片區中故障磁片區域大小總計的可用空間。 |
| align =`<n>` | 將所有磁片區或磁碟分割範圍對齊到最接近的對齊界限。 其中 *n* 是從磁片開頭到最接近對齊界限的 KB (kb) 數目。 |
| noerr | 僅供腳本之用。 發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要以動態磁碟4取代來取代具有焦點的磁片區，請輸入：

```
repair disk=4
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取磁片區命令](select-volume.md)