---
title: bitsadmin getaclflags
description: 適用于**bitsadmin getaclflags**的 Windows 命令主題-抓取存取控制清單傳播旗標。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381795"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

抓取存取控制清單（ACL）傳播旗標。

## <a name="syntax"></a>語法

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

顯示下列一個或多個旗標值：
-   I/O使用檔案複製擁有者資訊。
-   G使用 file 複製群組資訊。
-   D:使用 file 複製 DACL 資訊。
-   今日使用 file 複製 SACL 資訊。

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取存取控制清單傳播旗標。
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)