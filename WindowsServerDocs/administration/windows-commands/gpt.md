---
title: gpt
description: Gpt 命令的參考文章，此命令會將) 的 gpt 屬性指派給具有焦點的磁碟分割 (s。
ms.topic: reference
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 72260c00830d2d85fbe4324203dcfefe90154c7e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634726"
---
# <a name="gpt"></a>gpt

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在基本 GUID 磁碟分割表格 (gpt) 磁片上，此命令會將 gpt 屬性 (s) 指派給具有焦點的磁碟分割。 Gpt 磁碟分割屬性提供有關使用資料分割的詳細資訊。 某些屬性是磁碟分割類型 GUID 所特有的。

您必須選擇基本的 gpt 磁碟分割，此作業才會成功。 使用 [ [選取資料分割] 命令](select-partition.md) 選取基本的 gpt 磁碟分割，並將焦點移至該分割區。

> [!CAUTION]
> 變更 gpt 屬性可能會導致您的基本資料磁片區無法被指派磁碟機號，或防止載入檔案系統。 強烈建議您不要變更 gpt 屬性，除非您是 (OEM) 的原始設備製造商，或是有 gpt 磁片經驗的 IT 專業人員。

## <a name="syntax"></a>語法

```
gpt attributes=<n>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 屬性 =`<n>` | 指定您想要套用至具有焦點的資料分割之屬性的值。 Gpt 屬性欄位是包含兩個子欄位的64位欄位。 較高的欄位僅在磁碟分割識別碼的內容中有解譯，而較低的欄位則在所有磁碟分割識別碼中都有解譯。 接受的值包括：<ul><li>**0x0000000000000001** -指定電腦必須有磁碟分割才能正常運作。</li><li>**0x8000000000000000** -指定當磁片移至另一部電腦，或電腦第一次看到磁片時，磁碟分割將不會收到磁碟機號。</li><li>**0x4000000000000000** -隱藏磁碟分割的磁片區，使其不會被裝載管理員偵測到。</li><li>**0x2000000000000000** -指定資料分割為另一個分割區的陰影複製。</li><li>**0x1000000000000000** -指定分割區是唯讀的。 這個屬性會防止將磁片區寫入至。</li></ul><p>如需這些屬性的詳細資訊，請參閱 [Create_PARTITION_PARAMETERS 結構](/windows/win32/api/vds/ns-vds-create_partition_parameters)的屬性一節。 |

#### <a name="remarks"></a>備註

- EFI 系統磁碟分割只包含啟動作業系統所需的二進位檔。 這可讓您輕鬆地將作業系統特定的 OEM 二進位檔或二進位檔放在其他磁碟分割中。

### <a name="examples"></a>範例

若要防止電腦自動將磁碟機號指派給具有焦點的磁碟分割，移動 gpt 磁片時請輸入：

```
gpt attributes=0x8000000000000000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取資料分割命令](select-partition.md)

- [create_PARTITION_PARAMETERS 結構](/windows/win32/api/vds/ns-vds-create_partition_parameters)
