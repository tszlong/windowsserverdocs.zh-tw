---
title: bitsadmin getaclflags
description: Bitsadmin getaclflags 命令的參考文章，此命令會抓取 (ACL) 傳播旗標的存取控制清單。
ms.topic: reference
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5254d65bb5ba3e35fcf5368e24045530a76bfd95
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033696"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

抓取 (ACL) 傳播旗標的存取控制清單，以反映子物件是否繼承專案。

## <a name="syntax"></a>語法

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

傳回下列一或多個旗標值：

- **o** -複製具有檔案的擁有者資訊。

- **g** -使用 file 複製群組資訊。

- **d** -複製任意存取控制清單 (DACL) 資訊與檔案。

- **s** -複製系統存取控制清單 (SACL) 具有檔案的資訊。

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的存取控制清單傳播旗標：

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
