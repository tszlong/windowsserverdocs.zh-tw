---
title: whoami
description: Whoami 的參考文章，它會顯示目前登入本機系統之使用者的使用者、群組和許可權資訊。
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdcef4072fe692f2717fe79814af926a2c151636
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896493"
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

|參數|描述|
|---------|-----------|
|/upn|顯示使用者主體名稱中的使用者名稱 (UPN) 格式。|
|/fqdn|以 (FQDN) 格式，顯示完整功能變數名稱中的使用者名稱。|
|/logonid|顯示目前使用者的登入識別碼。|
|/user|顯示目前的網域和使用者名稱，以及 (SID) 的安全識別碼。|
|/groups|顯示目前使用者所屬的使用者群組。|
|/priv|顯示目前使用者的安全性許可權。|
|/fo\<Format>|指定輸出格式。 有效值包括：</br>**資料表**在資料表中顯示輸出。 這是預設值。</br>**清單**顯示清單中的輸出。</br>**csv**以逗號分隔值顯示 (CSV) 格式的輸出。|
|/all|顯示目前的存取權杖中的所有資訊，包括目前的使用者名稱、安全識別碼 (SID) 、許可權，以及目前使用者所屬的群組。|
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