---
title: bitsadmin getaclflags
description: 適用於 Windows 命令主題**bitsadmin getaclflags** -擷取存取控制清單傳用旗標。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861449"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

擷取存取控制清單 (ACL) 傳用旗標。

## <a name="syntax"></a>語法

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

會顯示一或多個下列的旗標值：
-   /O:複製檔案的擁有者資訊。
-   G:複製檔案群組資訊。
-   D:複製檔案的 DACL 資訊。
-   S:SACL 資訊複製檔案。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的存取控制清單傳用旗標*myDownloadJob*。
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)