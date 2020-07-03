---
title: create partition msr
description: 建立分割區 msr 的參考文章，它會在 GUID 磁碟分割表格（gpt）磁片上建立 Microsoft Reserved （MSR）分割區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2714c47c467fda9c6ca3451331ab9bc7991d4591
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929648"
---
# <a name="create-partition-msr"></a>create partition msr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 GUID 磁碟分割表格（gpt）磁片上建立 Microsoft Reserved （MSR）分割區。 每個 gpt 磁片上都必須有 Microsoft 保留的磁碟分割。 此分割區的大小取決於 gpt 磁片的總大小。 Gpt 磁片的大小至少必須為 32 MB，才能建立 Microsoft 保留的磁碟分割。

> [!IMPORTANT]
> 使用此命令時，請務必小心。 因為 gpt 磁片需要特定的磁碟分割配置，所以建立 Microsoft 保留的磁碟分割可能會導致磁片變成無法讀取。
>
> 必須選取基本 gpt 磁片，此操作才能成功。 您必須使用 [[選取磁片](select-disk.md)] 命令來選取基本的 gpt 磁片，並將焦點移至其上。

## <a name="syntax"></a>語法

```
create partition msr [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 大小 =`<n>` | 分割區的大小（以 mb 為單位）。 資料分割至少是以位元組為單位，與所指定的數位相同 `<n>` 。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。 |
| offset =`<n>` | 指定分割區的建立位移（以 kb 為單位）。 位移會無條件進位，以完全填滿所使用的任何磁區大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

#### <a name="remarks"></a>備註

- 在用來啟動 Windows 作業系統的 gpt 磁片上，可延伸韌體介面（EFI）系統磁碟分割是磁片上的第一個磁碟分割，後面接著 Microsoft 保留的磁碟分割。 僅用於資料儲存體的 gpt 磁片沒有 EFI 系統磁碟分割，在此情況下，Microsoft 保留的磁碟分割為第一個磁碟分割。

- Windows 不會掛接 Microsoft 保留的磁碟分割。 您無法在其上儲存資料，亦無法將其刪除。

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的 Microsoft 保留磁碟分割，請輸入：

```
create partition msr size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立命令](create.md)

- [select disk](select-disk.md)
