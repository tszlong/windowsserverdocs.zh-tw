---
title: list
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854669"
---
# <a name="list"></a>list



顯示的磁碟、 磁碟中的資料分割、 在磁碟中，磁碟區或虛擬硬碟 (Vhd) 的清單。

## <a name="syntax"></a>語法

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|磁碟|顯示磁碟和其相關資訊，例如其大小、 數量的可用空間、 磁碟是基本或動態磁碟，以及磁碟使用的主開機記錄 (MBR) 或 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的清單。|
|磁碟分割|顯示目前的磁碟的磁碟分割表格中所列的資料分割。|
|音量|顯示所有磁碟上的基本和動態磁碟區。|
|vdisk|會顯示一份已附加及/或選取的 Vhd。 此命令會列出已中斷連結的 Vhd，如果目前選取;不過，將磁碟類型設定為 未知，直到連接 VHD。 以星號 （*） 標示的 VHD 已取得焦點。</br>注意：此命令只適用於 Windows 7 和 Windows Server 2008 R2。|

## <a name="remarks"></a>備註

-   列出時動態磁碟上的資料分割，分割區可能不會對應到磁碟上的動態磁碟區。 動態磁碟會包含系統磁碟區或開機磁碟區 （如果存在磁碟上） 的資料分割資料表中的項目，就會發生這項差異。 當中也包含以保留動態磁碟區使用的空間會佔用磁碟的剩餘空間的磁碟分割。
-   以星號 （*） 標示該物件擁有焦點。
-   列出時的磁碟，如果磁碟已遺失，其磁碟編號前面會加上 M。比方說，第一個遺失的磁碟編號是 M0。

## <a name="BKMK_examples"></a>範例

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

