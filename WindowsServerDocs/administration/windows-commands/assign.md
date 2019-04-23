---
title: assign
description: 適用於 Windows 命令主題**指派**-將磁碟機代號或掛接點指派給具有焦點的磁碟區。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d830f9b1ec894c1bc136a99e7beb5703f840dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889289"
---
# <a name="assign"></a>assign

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將磁碟機代號或掛接點指派給具有焦點的磁碟區中。

## <a name="syntax"></a>語法
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|letter=<d>|您想要指派給磁碟區的磁碟機代號。|
|mount=<path>|您想要指派給磁碟區掛接點路徑。<br /><br />如需如何使用此命令的相關資訊的指示，請參閱[指派磁碟機的掛接點資料夾路徑](https://go.microsoft.com/fwlink/?LinkId=207059)(https://go.microsoft.com/fwlink/?LinkId=207059)。|
|noerr|針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。|
## <a name="remarks"></a>備註
-   如果未不指定任何磁碟機代號或掛接點，則會指派下一個可用的磁碟機代號。 如果磁碟機代號或掛接點已在使用中，會產生錯誤。
-   藉由使用 [指派] 命令，您可以變更與卸除式磁碟機相關聯的磁碟機代號。
-   您無法將磁碟機代號指派給系統磁碟區、 開機磁碟區或包含分頁檔的磁碟區。 此外，您不能指派磁碟機代號的原始設備製造商 (OEM) 磁碟分割或基本資料磁碟分割以外的任何 GUID 磁碟分割表格 (gpt) 磁碟分割。
-   這項作業成功，就必須選取磁碟區。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。
## <a name="BKMK_examples"></a>範例
若要將 E 的字母指派到焦點的磁碟區，請輸入：
```
assign letter=e
```

