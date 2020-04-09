---
title: assign
description: Windows 命令的**指派**主題，它會將磁碟機號或掛接點指派給具有焦點的磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4745b0472e2c8ee7a4034d9a06d395d6089db6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851301"
---
# <a name="assign"></a>assign

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將磁碟機代號或掛接點指派給帶有焦點的磁碟區。

## <a name="syntax"></a>語法

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `letter=<d>` | 您想要指派給磁片區的磁碟機號。 |
| `mount=<path>` | 您想要指派給磁片區的掛接點路徑。 如需有關如何使用此命令的指示，請參閱[將掛接點資料夾路徑指派給磁片磁碟機](https://go.microsoft.com/fwlink/?LinkId=207059)。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註

- 如果未指定磁碟機代號或掛接點，則會指派下一個可用的磁碟機代號。 如果磁碟機代號或掛接點已在使用中，則會產生錯誤。

- 使用 assign 命令，您可以變更與卸除式磁碟機相關的磁碟機代號。

- 您不能指派磁碟機代號給系統磁碟區、開機磁碟區，或包含分頁檔的磁碟區。 此外，您不能將磁碟機號指派給原始設備製造商（OEM）磁碟分割，或是基本資料分割以外的任何 GUID 磁碟分割表格（gpt）磁碟分割。

- 必須選取一個磁碟區，這項操作才能繼續。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要將字母 E 指派給焦點的磁片區，請輸入：
```
assign letter=e
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法索引鍵]（command-line-syntax-key.md

