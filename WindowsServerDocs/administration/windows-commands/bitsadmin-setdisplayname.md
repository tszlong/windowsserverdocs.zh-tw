---
title: bitsadmin setdisplayname
description: 適用於 Windows 命令主題**bitsadmin setdisplayname** -設定指定工作的顯示名稱。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d50cd2785e42b554cee340abc97fe4e4b53adcfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843669"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



設定指定工作的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|DisplayName|文字用來指定工作的顯示名稱。|

## <a name="BKMK_examples"></a>範例

下列範例會設定名為工作的顯示名稱*myDownloadJob*要*myDownloadJob2*。
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)