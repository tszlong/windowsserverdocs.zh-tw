---
title: bitsadmin setdescription
description: 適用於 Windows 命令主題**bitsadmin setdescription** -設定指定工作的描述。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830749"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



設定指定工作的描述。

## <a name="syntax"></a>語法

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|描述|文字用來描述工作。|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的描述*myDownloadJob*。
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)