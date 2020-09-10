---
title: assign
description: 指派命令的參考文章，此命令會將磁碟機號或掛接點指派給具有焦點的磁片區。
ms.topic: reference
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ff5f911fdaa4fc3703cbef2374b7431925a44fcf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633460"
---
# <a name="assign"></a>assign

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將磁碟機代號或掛接點指派給帶有焦點的磁碟區。 您也可以使用此命令來變更與卸載式磁片磁碟機相關聯的磁碟機號。 如果未指定磁碟機代號或掛接點，則會指派下一個可用的磁碟機代號。 如果磁碟機代號或掛接點已在使用中，則會產生錯誤。

必須選取一個磁碟區，這項操作才能繼續。 使用 [ **選取磁片** 區] 命令來選取磁片區，並將焦點移至該磁片區。

> [!IMPORTANT]
> 您無法將磁碟機號指派給系統磁碟區、開機磁碟區，或包含分頁檔的磁片區。 此外，您無法將磁碟機號指派給原始設備製造商 (OEM) 磁碟分割或任何 GUID 磁碟分割表格 (gpt) 磁碟分割以外的磁碟分割。

## <a name="syntax"></a>語法

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `letter=<d>` | 您要指派給磁片區的磁碟機號。 |
| `mount=<path>` | 您要指派給磁片區的掛接點路徑。 如需有關如何使用此命令的指示，請參閱 [指派磁片磁碟機的掛接點資料夾路徑](../../storage/disk-management/assign-a-mount-point-folder-path-to-a-drive.md)。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要以焦點將字母 E 指派給磁片區，請輸入：

```
assign letter=e
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取磁片區命令](select-volume.md)
