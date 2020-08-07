---
title: gpt
description: Gpt 命令的參考文章，它會將 (s) 的 gpt 屬性指派給具有焦點的磁碟分割。
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561bc4a11580a45452ac71cffddee1c58e48cf86
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888549"
---
# <a name="gpt"></a>gpt

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在基本 GUID 磁碟分割表格上 (gpt) 磁片，此命令會將 gpt 屬性 (s) 指派給具有焦點的磁碟分割。 Gpt 磁碟分割屬性提供有關使用資料分割的其他資訊。 某些屬性是磁碟分割類型 GUID 所特有的。

您必須選擇基本 gpt 磁碟分割，此作業才會成功。 使用 [[選取資料分割] 命令](select-partition.md)來選取基本 gpt 磁碟分割，並將焦點移至該分割區。

> [!CAUTION]
> 變更 gpt 屬性可能會導致您的基本資料磁片區無法指派磁碟機號，或防止檔案系統載入。 我們強烈建議您不要變更 gpt 屬性，除非您是原始設備製造商， (OEM) 或熟悉 gpt 磁片的 IT 專業人員。

## <a name="syntax"></a>語法

```
gpt attributes=<n>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 屬性 =`<n>` | 指定您想要套用至具有焦點之資料分割的屬性值。 [Gpt 屬性] 欄位是包含兩個子欄位的64位欄位。 較高的欄位僅在磁碟分割識別碼的內容中有解譯，而較低的欄位則在所有磁碟分割識別碼中都有解譯。 接受的值包括：<ul><li>**0x0000000000000001** -指定電腦必須要有磁碟分割，才能正常運作。</li><li>**0x8000000000000000** -指定當磁片移至另一部電腦，或是電腦第一次看到磁片時，分割區預設不會收到磁碟機號。</li><li>**0x4000000000000000** -隱藏分割區的磁片區，使其不會被掛接管理員偵測到。</li><li>**0x2000000000000000** -指定分割區是另一個分割區的陰影複製。</li><li>**0x1000000000000000** -指定分割區是唯讀的。 這個屬性會防止將磁片區寫入。</li></ul><p>如需這些屬性的詳細資訊，請參閱[Create_PARTITION_PARAMETERS 結構](/windows/win32/api/vds/ns-vds-create_partition_parameters)的屬性一節。 |

#### <a name="remarks"></a>備註

- EFI 系統磁碟分割僅包含啟動作業系統所需的二進位檔。 這可讓作業系統特定的 OEM 二進位檔或二進位檔可以放在其他磁碟分割中。

### <a name="examples"></a>範例

若要防止電腦自動將磁碟機號指派給具有焦點的磁碟分割，在移動 gpt 磁片時，請輸入：

```
gpt attributes=0x8000000000000000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

- [create_PARTITION_PARAMETERS 結構](/windows/win32/api/vds/ns-vds-create_partition_parameters)
