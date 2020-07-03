---
title: whoami
description: Whoami 的參考文章，它會顯示目前登入本機系統之使用者的使用者、群組和許可權資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a8ab5b02ab8670145887bcbf1ecfaa5efac95ad
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936578"
---
# <a name="whoami"></a>whoami



顯示目前登入本機系統之使用者的使用者、群組和許可權資訊。 如果使用時不含參數， **whoami**會顯示目前的網域和使用者名稱。



## <a name="syntax"></a>語法

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/upn|顯示使用者主體名稱（UPN）格式的使用者名稱。|
|/fqdn|以完整功能變數名稱（FQDN）格式顯示使用者名稱。|
|/logonid|顯示目前使用者的登入識別碼。|
|/user|顯示目前的網域和使用者名稱以及安全識別碼（SID）。|
|/groups|顯示目前使用者所屬的使用者群組。|
|/priv|顯示目前使用者的安全性許可權。|
|/fo\<Format>|指定輸出格式。 有效值包括：</br>**資料表**在資料表中顯示輸出。 這是預設值。</br>**清單**顯示清單中的輸出。</br>**csv**以逗號分隔值（CSV）格式顯示輸出。|
|/all|顯示目前存取權杖中的所有資訊，包括目前使用者的名稱、安全識別碼（SID）、許可權，以及目前使用者所屬的群組。|
|/nh|指定不應該在輸出中顯示資料行標題。 這只對資料表和 CSV 格式有效。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要顯示目前登入這部電腦之人員的網域和使用者名稱，請輸入：
```
whoami
```
您會看到類似以下的輸出：
```
DOMAIN1\administrator
```
若要顯示目前存取權杖中的所有資訊，請輸入：
```
whoami /all
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)