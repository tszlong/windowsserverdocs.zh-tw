---
title: list
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ac8b19ecae30c339138f61a13c21147d4bcf1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374653"
---
# <a name="list"></a>list



顯示磁片中的磁碟分割、磁片中的磁片區或虛擬硬碟（Vhd）的磁片清單。

## <a name="syntax"></a>語法

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|磁碟|顯示磁片和其相關資訊的清單，例如其大小、可用空間量、磁片是否為基本或動態磁碟，以及磁片是否使用主開機記錄（MBR）或 GUID 磁碟分割表格（GPT）磁碟分割樣式。|
|磁碟分割|顯示目前磁片的磁碟分割資料表中所列的分割區。|
|音量|顯示所有磁碟上的基本和動態磁碟區。|
|vdisk|顯示已附加和/或選取的 Vhd 清單。 此命令會列出已卸離的 Vhd （如果目前已選取）;不過，在附加 VHD 之前，磁片類型會設定為 [未知]。 以星號（*）標示的 VHD 具有焦點。</br>注意：此命令僅適用于 Windows 7 和 Windows Server 2008 R2。|

## <a name="remarks"></a>備註

-   列出動態磁碟上的磁碟分割時，分割區可能不會對應到磁片上的動態磁碟區。 發生這種差異的原因，是因為動態磁碟包含系統磁碟區或開機磁碟區（如果磁片上有）的磁碟分割表中的專案。 它們也包含佔用磁片剩餘部分的分割區，以保留動態磁碟區所使用的空間。
-   以星號（*）標示的物件具有焦點。
-   當列出磁片時，如果磁片遺失，其磁片編號前面會加上 M。例如，第一個遺失的磁片編號為 M0。

## <a name="BKMK_examples"></a>典型

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

