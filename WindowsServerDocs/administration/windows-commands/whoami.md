---
title: whoami
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840149"
---
# <a name="whoami"></a>whoami



顯示目前登入本機系統的使用者的使用者、 群組和權限資訊。 如果未指定參數，使用**whoami**會顯示目前的網域和使用者名稱。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/upn|在 使用者主體名稱 (UPN) 格式顯示的使用者名稱。|
|/fqdn|完整的網域名稱 (FQDN) 格式顯示的使用者名稱。|
|/logonid|顯示目前使用者的登入識別碼。|
|/user|顯示目前的網域和使用者名稱和安全性識別碼 (SID)。|
|/groups|顯示目前使用者所屬的使用者群組。|
|/priv|顯示目前使用者的安全性權限。|
|/fo\<格式 >|指定輸出格式。 有效值包括：</br>**資料表**以表格顯示輸出。 這是預設值。</br>**清單**會以清單顯示輸出。</br>**csv**以逗號分隔值 (CSV) 格式顯示輸出。|
|/all|顯示目前的存取權杖，包括目前的使用者名稱、 安全性識別元 (SID)、 權限，以及目前使用者所屬的群組中所有的資訊。|
|/nh|指定不應該在輸出中顯示資料行標頭。 這是僅適用於資料表和 CSV 格式。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要顯示目前登入此電腦之人員的網域和使用者名稱，請輸入：
```
whoami
```
此時會出現如下所示的輸出：
```
DOMAIN1\administrator
```
若要在目前的存取權杖中顯示的所有資訊，請輸入：
```
whoami /all
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)