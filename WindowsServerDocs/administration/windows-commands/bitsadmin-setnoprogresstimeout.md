---
title: bitsadmin setnoprogresstimeout
description: '**Bitsadmin setnoprogresstimeout**的 Windows 命令主題-設定服務在發生暫時性錯誤之後，嘗試傳輸檔案的時間長度（以秒為單位）。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380505"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

設定在發生第一個暫時性錯誤之後，BITS 嘗試傳輸檔案的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|TimeOutvalue|以秒數表示的數位。|

## <a name="remarks"></a>備註

-   當作業遇到暫時性錯誤時，不會開始任何進度逾時間隔。
-   當成功傳輸位元組的資料時，會停止或重設逾時間隔。
-   如果沒有進度逾時間隔超過*TimeOutvalue*，則會將工作置於嚴重的錯誤狀態。

## <a name="BKMK_examples"></a>典型

下列範例會將名為*myDownloadJob*之作業的 [無進度超時] 值設定為20秒
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)