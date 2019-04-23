---
title: bitsadmin setnoprogresstimeout
description: 適用於 Windows 命令主題**bitsadmin setnoprogresstimeout** -以秒為單位，服務就會嘗試將檔案傳輸之後就會發生暫時性錯誤, 設定的時間長度。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873769"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

設定時間長度，以秒為單位，BITS 嘗試傳送檔案之後就會發生第一個暫時性的錯誤。

## <a name="syntax"></a>語法

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|TimeOutvalue|表示以秒為單位的數字。|

## <a name="remarks"></a>備註

-   當工作遇到暫時性錯誤時，就會開始沒有進度逾時間隔。
-   逾時間隔會停止，或重設成功傳送資料的位元組。
-   如果沒有進度的逾時間隔超過*TimeOutvalue*，則作業會變成嚴重的錯誤狀態。

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的任何進度逾時值*myDownloadJob*為 20 秒
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)