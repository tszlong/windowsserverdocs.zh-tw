---
title: bitsadmin getaclflags
description: Bitsadmin getaclflags 命令的參考主題，它會抓取存取控制清單（ACL）傳播旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ca541b488c3c83e7a64a138bae0914001778e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718178"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

抓取存取控制清單（ACL）傳播旗標，反映子物件是否繼承專案。

## <a name="syntax"></a>語法

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

傳回下列一個或多個旗標值：

- **o** -使用 file 複製擁有者資訊。

- **g** -使用 file 複製群組資訊。

- **d** -使用檔案複製任意存取控制清單（DACL）資訊。

- **s** -使用檔案複製系統存取控制清單（SACL）資訊。

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的存取控制清單傳播旗標：

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
