---
title: create partition primary
description: Create partition primary 命令的參考文章，此命令會在基本磁碟上建立具有焦點的主要磁碟分割。
ms.topic: reference
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 567dd32abc7b34bc6d5f9b4cbfb579672422df06
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629118"
---
# <a name="create-partition-primary"></a>create partition primary

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在基本磁碟上建立具有焦點的主要磁碟分割。 建立磁碟分割之後，焦點會自動移到新磁碟分割。

> [!IMPORTANT]
> 必須選取基本磁碟，此操作才會成功。 您必須使用 [ [選取磁片](select-disk.md) ] 命令來選取基本磁碟，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定磁碟分割大小為 MB。 如果沒有指定大小的話，則磁碟分割會繼續，直到目前的區域中沒有多餘的可用空間為止。 |
| offset =`<n>` | 建立分割區時，以 kb 為單位的位移 (KB) 。 如果沒有指定位移，磁碟分割就會從夠大的磁片區的開頭開始，足以容納它。 |
| align =`<n>` | 將所有資料分割範圍對齊到最接近的對齊界限。 通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| 識別碼 = { `<byte>  | <guid>` } | 指定資料分割類型。 此參數僅適用于原始設備製造商 (OEM) 用途。 您可以使用這個參數指定任何資料分割類型的 byte 或 GUID。 DiskPart 不會檢查資料分割類型的有效性，除非確定它是十六進位格式或 GUID 的位元組。 **注意：** 使用此參數建立磁碟分割可能會導致您的電腦失敗或無法啟動。 除非您是 OEM 或透過 gpt 磁片體驗的 IT 專業人員，否則請勿使用此參數在 gpt 磁片上建立磁碟分割。 相反地，請一律使用 [create partition efi](create-partition-efi.md) 命令來建立 efi 系統磁碟分割、 [建立磁碟分割 msr](create-partition-msr.md) 命令以建立 Microsoft 保留的磁碟分割，並使用 [create partition primary](create-partition-primary.md)) 命令 (不使用 `id={ <byte>  | <guid>` 參數) 在 gpt 磁片上建立主要磁碟分割。<p>若**為主開機記錄 (MBR) 磁片**，您必須以十六進位格式指定磁碟分割的磁碟分割類型位元組。 如果未指定此參數，此命令會建立類型的分割區 `0x06` ，以指定未安裝檔案系統。 範例包括：<ul><li>**LDM 資料分割：** 0x42</li><li>**修復磁碟分割：** 0x27</li><li>**公認的 OEM 磁碟分割：** 0x12、0X84、0XDE、0XFE、0xA0</li></ul><p>**針對 GUID 磁碟分割表格 (gpt) 磁片**，您可以為您想要建立的磁碟分割指定磁碟分割類型 GUID。 辨識的 Guid 包括：<ul><li>**EFI 系統磁碟分割：** c12a7328-f81f-11d2-ba4b-00a0c93ec93b</li><li>**Microsoft 保留的磁碟分割：** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**基本資料分割：** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li><li>**LDM 中繼資料磁碟分割 (動態磁碟) ：** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**LDM 資料磁碟分割 (動態磁碟) ：** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**修復磁碟分割：** de94bba4-06d1-4d40-a16a-bfd50179d6ac<p>如果未指定 gpt 磁片的這個參數，此命令會建立基本的資料分割。</li></ul> |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有 noerr 參數時，錯誤會使 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的主要磁碟分割，請輸入：

```
create partition primary size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [指派命令](assign.md)

- [create 命令](create.md)

- [select disk](select-disk.md)
