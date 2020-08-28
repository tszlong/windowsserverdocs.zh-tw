---
title: '中斷 (陰影複製磁片區) '
description: Break 命令的參考文章，此命令會解除陰影複製磁片區與 VSS 的對應，並使其可作為一般磁片區存取。
ms.topic: reference
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d94275b83ad65f70db4bd1850f55ea527292d1bc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034306"
---
# <a name="break-shadow-copy-volume"></a>中斷 (陰影複製磁片區) 

將陰影複製磁片區與 VSS 解除區隔，並使其可作為一般磁片區來存取。 然後，您可以使用磁碟機號 (（如果指派的) 或磁片區名稱）來存取磁片區。 如果使用時不含參數，請在命令提示字元中使用 **break** 來顯示說明。

> [!NOTE]
> 此命令只與匯入後的硬體陰影複製有關。
>
> 公開的磁片區（例如它們所源自的陰影複製）預設是唯讀的。 磁片區的存取權會直接對硬體提供者進行，而不需記錄磁片區的陰影複製。

## <a name="syntax"></a>語法

```
break [writable] <setid>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 寫 | 啟用磁片區的讀取/寫入存取。 |
| \<setid> | 指定陰影複製集的識別碼。 陰影複製識別碼的別名（由 **load metadata** 命令儲存為環境變數）可以在 *SetID* 參數中使用。 |

## <a name="examples"></a>範例

若要使用別名名稱 Alias1 來建立陰影複製，可作為作業系統中的可寫入磁片區：

```
break writable %Alias1%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)