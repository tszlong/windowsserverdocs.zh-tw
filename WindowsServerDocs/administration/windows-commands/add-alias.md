---
title: 新增別名
description: '**新增別名**的 Windows 命令主題-將別名新增至別名環境。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2834376e497f54eadf1d9077e74f9c398202c5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382823"
---
# <a name="add-alias"></a>新增別名



將別名新增至別名環境。 如果在沒有參數的情況下使用， **add alias**會在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<AliasName >|指定別名的名稱。|
|\<AliasValue >|指定別名的值。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   別名會儲存在中繼資料檔案中，並且會使用 [**載入中繼資料**] 命令載入。

## <a name="BKMK_examples"></a>典型

若要列出所有陰影，包括其別名，請輸入：
```
list shadows all
```
下列摘錄顯示已指派預設別名 VSS_SHADOW_x 的陰影複製：
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
若要將名稱為 System1 的新別名指派給此陰影複製，請輸入：
```
add alias System1 %VSS_SHADOW_1%
```
或者，您可以使用陰影複製識別碼來指派別名：
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)