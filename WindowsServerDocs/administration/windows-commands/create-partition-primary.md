---
title: create partition primary
description: 建立資料分割主要命令的參考文章，這會在基本磁碟上建立具有焦點的主要磁碟分割。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9014b69528d4a67ba2c9c11b8ec53cf3a0f569f6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929621"
---
# <a name="create-partition-primary"></a>create partition primary

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在基本磁碟上建立具有焦點的主要磁碟分割。 建立磁碟分割之後，焦點會自動移到新磁碟分割。

> [!IMPORTANT]
> 必須選取基本磁碟，此操作才能成功。 您必須使用 [[選取磁片](select-disk.md)] 命令來選取基本磁碟，並將焦點轉移到其上。

## <a name="syntax"></a>語法

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 大小 =`<n>` | 指定磁碟分割大小為 MB。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。 |
| offset =`<n>` | 分割區的建立位移（以 kb 為單位）。 如果沒有指定位移，分割區會從最大磁片範圍的開頭開始，夠大，足以容納它。 |
| align =`<n>` | 將所有資料分割範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號（LUN）陣列搭配使用，以改善效能。 `<n>`這是從磁片開始到最接近對齊界限的 kb 數（KB）。 |
| 識別碼 = { `<byte>  | <guid>` } | 指定磁碟分割類型。 此參數僅供原始設備製造商（OEM）使用。 您可以使用這個參數來指定任何資料分割類型 byte 或 GUID。 DiskPart 不會檢查磁碟分割類型的有效性，除非確定它是十六進位格式或 GUID 的位元組。 **注意：** 使用此參數建立磁碟分割可能會導致您的電腦失敗或無法啟動。 除非您是 OEM 或具有 gpt 磁片經驗的 IT 專業人員，否則請不要使用此參數在 gpt 磁片上建立磁碟分割。 相反地，請一律使用[create partition efi](create-partition-efi.md)命令來建立 efi 系統磁碟分割、建立[磁碟分割 msr](create-partition-msr.md)命令來建立 Microsoft 保留的磁碟分割，以及[建立磁碟分割主要](create-partition-primary.md)）命令（不含 `id={ <byte>  | <guid>` 參數），以在 gpt 磁片上建立主要磁碟分割。<p>**對於主開機記錄（MBR）磁片**，您必須指定磁碟分割的磁碟分割類型 byte （十六進位格式）。 如果未指定此參數，此命令會建立類型的磁碟分割 `0x06` ，指定未安裝檔案系統。 範例包括：<ul><li>**LDM 資料分割：** 0x42</li><li>**修復磁碟分割：** 0x27</li><li>**可識別的 OEM 磁碟分割：** 0x12、0X84、0XDE、0XFE、0xA0</li></ul><p>**對於 GUID 磁碟分割表格（gpt）磁片**，您可以針對想要建立的磁碟分割指定磁碟分割類型 GUID。 可識別的 Guid 包括：<ul><li>**EFI 系統磁碟分割：** c12a7328-f81f-11d2-ba4b-00a0c93ec93b</li><li>**Microsoft 保留的磁碟分割：** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**基本資料分割：** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li><li>**LDM 中繼資料分割（動態磁碟）：** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**LDM 資料分割（動態磁碟）：** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**修復磁碟分割：** de94bba4-06d1-4d40-a16a-bfd50179d6ac<p>如果未針對 gpt 磁片指定此參數，此命令會建立基本資料分割。</li></ul> |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有 noerr 參數時，錯誤會使 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的主要磁碟分割，請輸入：

```
create partition primary size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [指派命令](assign.md)

- [建立命令](create.md)

- [select disk](select-disk.md)
