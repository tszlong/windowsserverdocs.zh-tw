---
title: bitsadmin getaclflags
description: 適用于**bitsadmin getaclflags**的 Windows 命令主題，它會抓取存取控制清單（ACL）傳播旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d53018e2fa5c659c8cf4b0ec985beda848a8c1af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850791"
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
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="remarks"></a>備註

顯示下列一個或多個旗標值：

- **o** -使用 file 複製擁有者資訊。

- **g** -使用 file 複製群組資訊。

- **d** -使用檔案複製任意存取控制清單（DACL）資訊。

- **s** -使用檔案複製系統存取控制清單（SACL）資訊。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取存取控制清單傳播旗標。

```
C:\>bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)