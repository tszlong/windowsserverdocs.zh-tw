---
title: ksetup： addkdc
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bb31cbc8ba7920c4ba609f86202e2e62a705078
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841831"
---
# <a name="ksetupaddkdc"></a>ksetup： addkdc



為指定的 Kerberos 領域新增金鑰發佈中心（KDC）位址。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，在執行**ksetup**時，它會列為預設領域。 這是您嘗試新增其他 KDC 的領域。|
|\<KDCName >|KDC 名稱會指定為不區分大小寫的完整功能變數名稱，例如 mitkdc.microsoft.com。 如果省略 KDC 名稱，DNS 將會找出 Kdc。|

## <a name="remarks"></a>備註

這些對應會儲存在登錄中**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**底下。 若要將 Kerberos 領域設定資料部署到多部電腦，請使用 [安全性設定範本] 嵌入式管理單元和原則發佈，而不是在個別電腦上明確使用**ksetup** 。

電腦必須重新開機，才會使用新的領域設定。

若要驗證電腦的預設領域名稱，或確認此命令是否如預期運作，請在命令提示字元中執行**ksetup** ，並確認已新增 KDC 的輸出。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

設定非 Windows KDC 伺服器和工作站應使用的領域：
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
在與前一個命令相同的電腦命令列中執行 Ksetup 工具，將本機電腦帳戶密碼設定為 p@sswrd1%。 然後重新開機電腦。
```
Ksetup /setcomputerpassword p@sswrd1%
```

## <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)