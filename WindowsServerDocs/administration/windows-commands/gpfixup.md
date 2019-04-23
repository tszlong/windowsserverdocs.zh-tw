---
title: gpfixup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbe9873cc15866037c4688aaac89095e4a85dec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889419"
---
# <a name="gpfixup"></a>gpfixup



網域重新命名作業之後，請修正群組原則物件 和 群組原則 連結中的網域名稱相依性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/v|顯示詳細狀態訊息。</br>如果未使用此參數，僅錯誤訊息或摘要的狀態訊息**成功**或是**失敗**隨即出現。|
|/olddns:\<OLDDNSNAME >|指定舊的 DNS 名稱，與位於已重新命名網域 *\<OLDDNSNAME >* 網域重新命名作業時變更網域的 DNS 名稱。 您可以使用這個參數，您也使用時，才 **/newdns**參數來指定新的網域 DNS 名稱。|
|/newdns:\<NEWDNSNAME >|指定新的 DNS 名稱，與位於已重新命名網域 *\<NEWDNSNAME >* 網域重新命名作業時變更網域的 DNS 名稱。 您可以使用這個參數，您也使用時，才 **/olddns**參數來指定舊的網域 DNS 名稱。|
|/oldnb:\<OLDFLATNAME >|指定舊的 NetBIOS 名稱，與位於已重新命名網域 *\<OLDFLATNAME >* 網域重新命名作業時變更網域的 NetBIOS 名稱。 您可以使用這個參數，只有當您使用 **/newnb**參數來指定新的 網域 NetBIOS 名稱。|
|/newnb:\<NEWFLATNAME >|指定新的 NetBIOS 名稱，與位於已重新命名網域 *\<NEWFLATNAME >* 網域重新命名作業時變更網域的 NetBIOS 名稱。 您可以使用這個參數，只有當您使用 **/oldnb**參數來指定舊的 網域 NetBIOS 名稱。|
|/dc:\<DCNAME>|連接到名為的網域控制站 *\<DCNAME >* （DNS 名稱或 NetBIOS 名稱）。 *\<DCNAME >* 必須裝載可寫入的網域目錄磁碟分割複本，由下列其中一個步驟所示：</br>-DNS 名稱 *\<NEWDNSNAME >* 使用 **/newdns**</br>-的 NetBIOS 名稱 *\<NEWFLATNAME >* 使用 **/newnb**</br>如果未使用此參數，連接到已重新命名所指定的網域中任何網域控制站 *\<NEWDNSNAME >* 或是 *\<NEWFLATNAME >*。|
|/sionly|執行僅與受管理的軟體安裝 （群組原則軟體安裝延伸模組） 相關的群組原則修正。 略過動作修復 Gpo 中的 群組原則連結和 SYSVOL 路徑。|
|/user:\<使用者名稱 >|在使用者的安全性內容中執行此命令*\<使用者名稱 >*，其中*\<使用者名稱 >* 處於 網域 \ 使用者格式。</br>如果未使用此參數，請登入的使用者身分執行此命令。|
|/pwd: {\<密碼 >|*}|指定的密碼，以使用其他安全性內容 **/user**。 如果 **&#42;** 指定而不使用密碼，提示您輸入密碼。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   **Gpfixup**命令適用於 Windows Server 2008 R2 和 Windows Server 2008，除非在 Server Core 安裝上。
-   雖然群組原則管理主控台 (GPMC) 隨附於 Windows Server 2008 R2 和 Windows Server 2008，您必須安裝群組原則管理，為透過 伺服器管理員的功能。

## <a name="BKMK_Examples"></a>範例

這個範例假設您已經執行網域重新命名作業，您已變更的 DNS 名稱**MyOldDnsName**要**MyNewDnsName**，和中的 NetBIOS 名稱**MyOldNetBIOSName**要**MyNewNetBIOSName**。 在此範例中，您可以使用**gpfixup**命令以連線至名為的網域控制站**MyDcDnsName**和修復 Gpo 和群組原則連結，藉由更新舊的網域名稱內嵌在 Gpo 及連結。 狀態和錯誤輸出會儲存到檔案，稱為**gpfixup.log**。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
此範例與上一個相同，不同之處在於它會假設網域期間未變更網域的 NetBIOS 名稱重新命名作業。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>其他參考資料

-   [管理 Active Directory 網域重新命名](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [群組原則 TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令列語法關鍵](command-line-syntax-key.md)