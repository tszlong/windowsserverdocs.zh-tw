---
title: create partition msr
description: Create partition msr 的參考文章，可在 GUID 磁碟分割表格上建立 Microsoft Reserved (MSR) 磁碟分割 (gpt) 磁片。
ms.topic: reference
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df792001cf48d9d5fce69de6dc9bc6bdd09a1f8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033226"
---
# <a name="create-partition-msr"></a>create partition msr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 GUID 磁碟分割表格上建立 Microsoft Reserved (MSR) 磁碟分割 (gpt) 磁片。 每個 gpt 磁片都需要 Microsoft 保留的磁碟分割。 此磁碟分割的大小取決於 gpt 磁片的總大小。 Gpt 磁片的大小必須至少為 32 MB，才能建立 Microsoft 保留的磁碟分割。

> [!IMPORTANT]
> 使用此命令時，請務必小心。 因為 gpt 磁片需要特定的磁碟分割配置，所以建立 Microsoft 保留的磁碟分割可能會導致磁片變成無法讀取。
>
> 必須選取基本的 gpt 磁片，此作業才會成功。 您必須使用 [ [選取磁片](select-disk.md) ] 命令來選取基本的 gpt 磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
create partition msr [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 磁碟分割的大小，以 mb (MB) 。 分割區的長度至少與所指定的數位相同 `<n>` 。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。 |
| offset =`<n>` | 以 kb 為單位，指定要在其中建立資料分割的位移（以 kb 為單位） (KB) 。 位移會無條件四捨五入，以完全填滿使用的任何磁區大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

#### <a name="remarks"></a>備註

- 在用來啟動 Windows 作業系統的 gpt 磁片上，可延伸的固件介面 (EFI) 系統磁碟分割是磁片上的第一個磁碟分割，後面接著 Microsoft 保留的磁碟分割。 只用于資料存放區的 gpt 磁片沒有 EFI 系統磁碟分割，在此情況下，Microsoft 保留的磁碟分割是第一個磁碟分割。

- Windows 不會掛接 Microsoft 保留的磁碟分割。 您無法在其上儲存資料，亦無法將其刪除。

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的 Microsoft 保留磁碟分割，請輸入：

```
create partition msr size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)

- [select disk](select-disk.md)
