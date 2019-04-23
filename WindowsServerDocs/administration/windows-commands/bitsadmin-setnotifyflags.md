---
title: bitsadmin setnotifyflags
description: 適用於 Windows 命令主題**bitsadmin setnotifyflags** -指定作業的通知旗標設定的事件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868789"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

設定事件通知旗標，指定作業。

## <a name="syntax"></a>語法

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|NotifyFlags|請參閱 < 備註 >|

## <a name="remarks"></a>備註

**NotifyFlags**參數可以包含一或多個下列的通知旗標。

|---|---| | 1 |產生事件時已傳送作業中的所有檔案。 || 2 |產生的事件發生錯誤時。 || 4 |停用通知。 |

## <a name="BKMK_examples"></a>範例

下列範例會設定的通知旗標，移轉，和名為的工作作業的錯誤事件*myDownloadJob*。
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)