---
title: gpfixup
description: Gpfixup 命令的參考文章，可修正群組原則物件中的功能變數名稱相依性，以及網域重新命名作業之後的群組原則連結。
ms.topic: reference
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b11ae09bcc345c9fb656a8945e0febe5f4098ac
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025692"
---
# <a name="gpfixup"></a>gpfixup

修正群組原則物件中的功能變數名稱相依性，以及網域重新命名作業之後群組原則連結。 若要使用此命令，您必須透過伺服器管理員將群組原則管理安裝為功能。

## <a name="syntax"></a>語法

```
gpfixup [/v]
[/olddns:<olddnsname> /newdns:<newdnsname>]
[/oldnb:<oldflatname> /newnb:<newflatname>]
[/dc:<dcname>] [/sionly]
[/user:<username> [/pwd:{<password>|*}]] [/?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| /v | 顯示詳細的狀態訊息。 如果未使用此參數，則只會顯示錯誤訊息或摘要狀態訊息，指出 [ **成功** ] 或 [ **失敗** ]。 |
| /olddns:`<olddnsname>` | 指定 `<olddnsname>` 當網域重新命名作業變更網域的 dns 名稱時，重新命名之網域的舊 DNS 名稱。 只有當您也使用 **/newdns** 參數來指定新的網域 DNS 名稱時，才能使用此參數。 |
| /newdns:`<newdnsname>` | 指定 `<newdnsname>` 當網域重新命名作業變更網域的 dns 名稱時，重新命名之網域的新 dns 名稱。 只有當您也使用 **/olddns** 參數來指定舊的網域 DNS 名稱時，才能使用此參數。 |
| /oldnb:`<oldflatname>` | 指定 `<oldflatname>` 當網域重新命名作業變更網域的 netbios 名稱時，重新命名之網域的舊 NetBIOS 名稱。 只有當您使用 **/newnb** 參數來指定新的網域 NetBIOS 名稱時，才能使用此參數。 |
| /newnb:`<newflatname>` | 指定 `<newflatname>` 當網域重新命名作業變更網域的 netbios 名稱時，重新命名之網域的新 netbios 名稱。 只有當您使用 **/oldnb** 參數來指定舊的網域 NetBIOS 名稱時，才能使用此參數。 |
| 電源`<dcname>` | 連接到名為 `<dcname>` (DNS 名稱或 NetBIOS 名稱) 的網域控制站。 `<dcname>` 必須裝載網域目錄分割的可寫入複本，如下列其中一項所示：<ul><li>`<newdnsname>`使用 **/NEWDNS**的 DNS 名稱</li><li>`<newflatname>`使用 **/Newnb**的 NetBIOS 名稱</br>如果未使用此參數，您可以連接到或所指定之重新命名網域中的任何網域控制站 `<newdnsname>` `<newflatname>` 。</li></ul> |
| /sionly | 只會執行與受管理軟體安裝相關的群組原則修正 (群組原則) 的軟體安裝延伸模組。 略過可修正 Gpo 中群組原則連結和 SYSVOL 路徑的動作。 |
| /user`<username>` |在使用者的安全性內容中執行此命令 `<username>` ，其中 `<username>` 的格式為 domain\user。 如果未使用此參數，此命令會以登入的使用者身分執行。 |
| /pwd`{<password> | *}` | 指定使用者的密碼。 |
| /? | 在命令提示字元顯示 [說明]。 |

### <a name="examples"></a>範例

此範例假設您已執行網域重新命名作業，其中已將 DNS 名稱從 **MyOldDnsName** 變更為 **MyNewDnsName**，並將 NetBIOS 名稱從 **MyOldNetBIOSName** 變更為 **MyNewNetBIOSName**。

在此範例中，您會使用 **gpfixup** 命令來連接到名為 **MyDcDnsName** 的網域控制站，並藉由更新內嵌在 gpo 和連結中的舊功能變數名稱來修復 gpo 和群組原則連結。 狀態和錯誤輸出會儲存至名為 **gpfixup**的檔案。

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

這個範例與上一個範例相同，不同之處在于它會假設網域重新命名作業期間未變更網域的 NetBIOS 名稱。

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [管理 Active Directory 網域重新命名](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794869(v=ws.10))
