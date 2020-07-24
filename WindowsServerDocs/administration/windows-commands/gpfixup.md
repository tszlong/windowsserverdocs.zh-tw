---
title: gpfixup
description: Gpfixup 命令的參考文章，它會在網域重新命名作業之後，修正群組原則物件和群組原則連結中的功能變數名稱相依性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15bef10afa49fafebfad485836bd6f9cdd5f496e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957170"
---
# <a name="gpfixup"></a>gpfixup

修正網域重新命名作業之後，群組原則物件和群組原則連結中的功能變數名稱相依性。 若要使用此命令，您必須透過伺服器管理員，將群組原則管理安裝為功能。

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
| /v | 顯示詳細的狀態訊息。 如果未使用此參數，則只會顯示錯誤訊息或摘要狀態訊息，指出、**成功**或**失敗**。 |
| /olddns:`<olddnsname>` | 指定網域 `<olddnsname>` 重新命名作業變更網域的 dns 名稱時，已重新命名之網域的舊 DNS 名稱。 只有當您也使用 **/newdns**參數指定新的網域 DNS 名稱時，才可以使用此參數。 |
| /newdns:`<newdnsname>` | `<newdnsname>`當網域重新命名作業變更網域的 DNS 名稱時，指定重新命名之網域的新 DNS 名稱。 只有當您同時使用 **/olddns**參數來指定舊網域 DNS 名稱時，才能使用此參數。 |
| /oldnb:`<oldflatname>` | 指定網域 `<oldflatname>` 重新命名作業變更網域的 netbios 名稱時，已重新命名之網域的舊 netbios 名稱。 只有當您使用 **/newnb**參數指定新的網域 NetBIOS 名稱時，才可以使用此參數。 |
| /newnb:`<newflatname>` | 指定網域 `<newflatname>` 重新命名作業變更網域的 netbios 名稱時，已重新命名之網域的新 netbios 名稱。 只有當您使用 **/oldnb**參數指定舊網域 NetBIOS 名稱時，才可以使用此參數。 |
| /dc`<dcname>` | 連接到名為的網域控制站 `<dcname>` （DNS 名稱或 NetBIOS 名稱）。 `<dcname>`必須裝載網域目錄磁碟分割的可寫入複本，如下列其中一項所示：<ul><li>`<newdnsname>`使用 **/NEWDNS**的 DNS 名稱</li><li>`<newflatname>`使用 **/Newnb**的 NetBIOS 名稱</br>如果未使用此參數，您可以連接到或所指示的已重新命名網域中的任何網域控制站 `<newdnsname>` `<newflatname>` 。</li></ul> |
| /sionly | 僅執行與受管理軟體安裝相關的群組原則修正（適用于群組原則的軟體安裝延伸模組）。 略過修正群組原則連結的動作，以及 Gpo 中的 SYSVOL 路徑。 |
| /user`<username>` |在使用者的安全性內容中執行此命令 `<username>` ，其中的 `<username>` 格式為 domain\user 如果未使用此參數，則會以登入的使用者身分執行此命令。 |
| /pwd`{<password> | *}` | 指定使用者的密碼。 |
| /? | 在命令提示字元顯示 [說明]。 |

### <a name="examples"></a>範例

這個範例假設您已執行網域重新命名作業，其中將 DNS 名稱從**MyOldDnsName**變更為**MyNewDnsName**，並將 NetBIOS 名稱從**MyOldNetBIOSName**變更為**MyNewNetBIOSName**。

在此範例中，您會使用**gpfixup**命令來連線到名為**MyDcDnsName**的網域控制站，並藉由更新內嵌在 gpo 和連結中的舊功能變數名稱來修復 gpo 和群組原則連結。 狀態和錯誤輸出會儲存到名為**gpfixup**的檔案中。

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

這個範例與前一個範例相同，不同之處在于它會假設網域的 NetBIOS 名稱在網域重新命名作業期間不會變更。

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [管理 Active Directory 網域重新命名](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794869(v=ws.10))
