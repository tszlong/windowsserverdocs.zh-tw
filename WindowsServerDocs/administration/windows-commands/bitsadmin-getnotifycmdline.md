---
title: bitsadmin getnotifycmdline
description: 適用于**bitsadmin getnotifycmdline**的 Windows 命令主題-抓取作業完成傳輸資料時所執行的命令列命令。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b91d2c71ad4bedaac65e23041ca78a70ade99977
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381494"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

當作業完成傳輸資料時，抓取要執行的命令列命令。

**BITS 1.2 和更早版本**： 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetNotifyCmdLine <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會在名為*myDownloadJob*的作業完成時，捕獲服務所使用的命令列命令。
```
C:\>bitsadmin /GetNotifyCmdLine myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)