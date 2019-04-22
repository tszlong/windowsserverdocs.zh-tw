---
title: bitsadmin getnotifycmdline
description: 適用於 Windows 命令主題**bitsadmin getnotifycmdline** -擷取作業完成的傳輸資料時執行的命令列命令。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3ca7b2e67c0b5672733a25465fba89d1bd69d07a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817289"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

擷取作業完成的傳輸資料時要執行的命令列命令。

**1.2 及更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetNotifyCmdLine <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取時，作業命名為服務所使用的命令列命令*myDownloadJob*完成。
```
C:\>bitsadmin /GetNotifyCmdLine myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)