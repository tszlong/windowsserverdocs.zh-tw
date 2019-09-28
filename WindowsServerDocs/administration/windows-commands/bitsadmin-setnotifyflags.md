---
title: bitsadmin setnotifyflags
description: '**Bitsadmin setnotifyflags**的 Windows 命令主題-設定指定之作業的事件通知旗標。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380446"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

設定指定之作業的事件通知旗標。

## <a name="syntax"></a>語法

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|NotifyFlags|請參閱備註|

## <a name="remarks"></a>備註

**NotifyFlags**參數可以包含下列一個或多個通知旗標。

|-----|-----| | 1 |當作業中的所有檔案都已轉移時，產生事件。 || 2 |發生錯誤時產生事件。 || 4 |停用通知。 |

## <a name="BKMK_examples"></a>典型

下列範例會針對名為*myDownloadJob*的作業，設定已傳送和錯誤事件作業的通知旗標。
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)