---
title: ksetup addkdc
description: '>ksetup addkdc 命令的參考文章，此命令會為指定的 Kerberos 領域 ads 金鑰發佈中心 (KDC) 位址。'
ms.topic: reference
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d86c494f3326f2d1fbc74eb81670b791004669fc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639746"
---
# <a name="ksetup-addkdc"></a>ksetup addkdc

為指定的 Kerberos 領域新增金鑰發佈中心 (KDC) 位址

對應會儲存在登錄中的 **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains** 下，而且必須先重新開機電腦，才會使用新的領域設定。

> [!NOTE]
> 若要將 Kerberos 領域設定資料部署到多部電腦，您必須在個別電腦上明確地使用「 **安全性設定範本** 」嵌入式管理單元和原則散發。 您無法使用此命令。

## <a name="syntax"></a>語法

```
ksetup /addkdc <realmname> [<KDCname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 當執行 **>ksetup** 時，這個值也會顯示為預設領域，而且是您想要新增其他 KDC 的領域。 |
| `<KDCname>` | 指定不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，DNS 會找出 Kdc。 |

### <a name="examples"></a>範例

若要設定非 Windows KDC 伺服器和工作站應使用的領域，請輸入：

```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

若要在與 p@sswrd1 前一個範例相同的電腦上，將本機電腦帳戶密碼設定為%，然後重新開機電腦，請輸入：

```
ksetup /setcomputerpassword p@sswrd1%
```

若要確認電腦的預設領域名稱，或確認此命令是否如預期運作，請輸入：

```
ksetup
```
檢查登錄以確定對應是否如預期執行。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup setcomputerpassword 命令](ksetup-setcomputerpassword.md)
