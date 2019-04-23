---
title: 新增別名
description: 適用於 Windows 命令主題**新增別名**-將別名新增至 「 別名 」 環境。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50de932ea0153546816face61f0852a08707ea85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862219"
---
# <a name="add-alias"></a>新增別名



將別名新增至 「 別名 」 環境中。 如果未指定參數，使用**新增別名**在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<AliasName >|指定別名的名稱。|
|\<AliasValue>|指定別名的值。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   別名會儲存在中繼資料檔案，而且也會載入具有**載入中繼資料**命令。

## <a name="BKMK_examples"></a>範例

若要列出所有的陰影，包括它們的別名，請輸入：
```
list shadows all
```
下列摘錄顯示已被指派預設別名 VSS_SHADOW_x，陰影複製：
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
若要將新的別名名稱 nic:1 指派到這個陰影複製，請輸入：
```
add alias System1 %VSS_SHADOW_1%
```
或者，您可以指派別名使用陰影複製識別碼：
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)