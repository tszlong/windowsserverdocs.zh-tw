---
title: bitsadmin getnotifyflags
description: 適用於 Windows 命令主題**bitsadmin getnotifyflags** -擷取指定作業的通知旗標。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889279"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



擷取指定作業的通知旗標。

## <a name="syntax"></a>語法

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

工作可以包含一或多個下列的通知旗標。

|---|---| | 0x001 |產生事件時已傳送作業中的所有檔案。 || 定義 0x002 |產生的事件發生錯誤時。 || 0x004 |停用通知。 || 0x008 |產生時修改作業時，或項目傳輸進度事件。 |

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的通知旗標*myDownloadJob*。
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)