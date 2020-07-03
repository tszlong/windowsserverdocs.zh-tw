---
title: list
description: List 命令的參考文章，其中顯示磁片中的磁碟分割清單、磁片中的磁片區，或虛擬硬碟（Vhd）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fda60b0520da00659d0ac8fc9cab483c62e2a6a7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931713"
---
# <a name="list"></a>list

顯示磁片中的磁碟分割、磁片中的磁片區或虛擬硬碟（Vhd）的磁片清單。

## <a name="syntax"></a>語法

```
list { disk | partition | volume | vdisk }
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| disk | 顯示磁碟清單及其相關資訊，例如，其大小、可用空間數量、磁碟是基本磁碟還是動態磁碟，以及磁碟使用的是主要開機記錄 (MBR) 還是 GUID 磁碟分割表 (GPT) 磁碟分割樣式。 |
| partition | 顯示目前磁碟中磁碟分割表格所列出的磁碟分割。 |
| 磁碟區 | 顯示所有磁碟上的基本和動態磁碟區清單。 |
| vdisk | 顯示已附加和/或選取的 Vhd 清單。 此命令會列出已卸離的 Vhd （如果目前已選取）;不過，在附加 VHD 之前，磁片類型會設定為 [未知]。 以星號（*）標示的 VHD 具有焦點。 |

#### <a name="remarks"></a>備註

- 列出動態磁碟上的磁碟分割時，分割區可能不會對應到磁片上的動態磁碟區。 發生此差異是因為系統磁碟區或開機磁碟區 (如果磁碟上有) 的動態磁碟區的磁碟分割表中包含項目。 它們也包含佔用磁片剩餘部分的分割區，以保留動態磁碟區所使用的空間。

- 以星號（*）標示的物件具有焦點。

- 當列出磁片時，如果磁片遺失，其磁片編號前面會加上 M。例如，第一個遺失的磁片編號為*M0*。

### <a name="examples"></a>範例

```
list disk
list partition
list volume
list vdisk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
