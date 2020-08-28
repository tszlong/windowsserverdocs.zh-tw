---
title: create partition efi
description: Create partition efi 命令的參考文章，可在 GUID 磁碟分割表格上建立可延伸的固件介面 (EFI) 系統磁碟分割 (在 Itanium 電腦上 gpt) 磁片。
ms.topic: reference
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 931f70ed8fabe1dea3ef06c124a696488975d860
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030266"
---
# <a name="create-partition-efi"></a>create partition efi

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 GUID 磁碟分割表格上建立可延伸的固件介面 (EFI) 系統磁碟分割 (以 Itanium 為基礎的電腦上 gpt) 磁片。 建立分割區之後，會將焦點提供給新的磁碟分割。

>[!NOTE]
> 必須選取 gpt 磁片，此操作才能成功。 使用 [ [選取磁片](select-disk.md) ] 命令來選取磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
create partition efi [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 磁碟分割的大小，以 mb (MB) 。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。 |
| offset =`<n>` | 建立分割區時，以 kb 為單位的位移 (KB) 。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

#### <a name="remarks"></a>備註

- 您必須加入至少一個具有 [ **新增磁片** 區] 命令的磁片區，才能使用 **create** 命令。

- 執行 **create** 命令之後，您可以使用 **exec** 命令來執行重複腳本，以從陰影複製進行備份。

- 您可以使用 [ **開始備份** ] 命令來指定完整備份，而不是複本備份。

## <a name="examples"></a>範例

若要在選取的磁片上建立 1000 mb 的 EFI 磁碟分割，請輸入：

```
create partition efi size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)

- [select disk](select-disk.md)
