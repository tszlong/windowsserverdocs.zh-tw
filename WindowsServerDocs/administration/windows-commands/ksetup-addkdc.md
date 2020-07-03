---
title: ksetup addkdc
description: Ksetup addkdc 命令的參考文章，其會為指定的 Kerberos 領域廣告金鑰發佈中心（KDC）位址。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32839c0f8c1e408cfa6ab1e067c250551ee7b490
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925564"
---
# <a name="ksetup-addkdc"></a>ksetup addkdc

為指定的 Kerberos 領域新增金鑰發佈中心（KDC）位址

對應會儲存在登錄中的**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**底下，而且必須重新開機電腦，才會使用新的領域設定。

> [!NOTE]
> 若要將 Kerberos 領域設定資料部署到多部電腦，您必須在個別電腦上明確使用 [**安全性設定範本**] 嵌入式管理單元和原則發佈。 您無法使用此命令。

## <a name="syntax"></a>語法

```
ksetup /addkdc <realmname> [<KDCname>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 當**ksetup**執行時，這個值也會顯示為預設領域，而是您想要將其他 KDC 加入其中的領域。 |
| `<KDCname>` | 指定不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，DNS 將會找出 Kdc。 |

### <a name="examples"></a>範例

若要設定非 Windows KDC 伺服器和工作站應使用的領域，請輸入：

```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

若要在與 p@sswrd1 上一個範例相同的電腦上，將本機電腦帳戶密碼設定為%，然後重新開機電腦，請輸入：

```
ksetup /setcomputerpassword p@sswrd1%
```

若要驗證電腦的預設領域名稱，或確認此命令是否如預期運作，請輸入：

```
ksetup
```
請檢查登錄，確定對應是否如預期發生。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup setcomputerpassword 命令](ksetup-setcomputerpassword.md)
