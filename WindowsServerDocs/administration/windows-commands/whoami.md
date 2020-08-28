---
title: whoami
description: Whoami 的參考文章，顯示目前登入本機系統之使用者的使用者、群組及許可權資訊。
ms.topic: reference
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3b66a8f8e45edc540745a210ef42b49740052e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031706"
---
# <a name="whoami"></a>whoami



顯示目前登入本機系統之使用者的使用者、群組及許可權資訊。 如果使用時不含參數， **whoami** 會顯示目前的網域和使用者名稱。



## <a name="syntax"></a>語法

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/upn|顯示使用者主體名稱 (UPN) 格式的使用者名稱。|
|/fqdn|以完整功能變數名稱顯示 (FQDN) 格式的使用者名稱。|
|/logonid|顯示目前使用者的登入識別碼。|
|/user|顯示目前的網域和使用者名稱，以及 (SID) 的安全識別碼。|
|/groups|顯示目前使用者所屬的使用者群組。|
|/priv|顯示目前使用者的安全性許可權。|
|/fo \<Format>|指定輸出格式。 有效值包括：</br>**資料表** 在資料表中顯示輸出。 這是預設值。</br>**清單** 顯示清單中的輸出。</br>**csv** 以逗點分隔值顯示 (CSV) 格式的輸出。|
|/all|顯示目前的存取權杖中的所有資訊，包括目前的使用者名稱、安全性識別碼 (SID) 、許可權和目前使用者所屬的群組。|
|/nh|指定不應該在輸出中顯示資料行標題。 這僅適用于資料表和 CSV 格式。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要顯示目前登入此電腦之人員的網域和使用者名稱，請輸入：
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