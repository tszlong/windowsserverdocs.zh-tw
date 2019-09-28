---
title: gpfixup
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e32427369f1664476c81a81353ae8869ec0c2ff3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375666"
---
# <a name="gpfixup"></a>gpfixup



修正網域重新命名作業之後，群組原則物件和群組原則連結中的功能變數名稱相依性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>參數

|       參數       |                                                                                                                                                                                                                               描述                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      顯示詳細的狀態訊息。</br>如果未使用此參數，則只會顯示錯誤訊息，或出現 [**成功**] 或 [**失敗**] 的摘要狀態訊息。                                                                                                                                                       |
| /olddns： \<OLDDNSNAME > |                                                                                                           當網域重新命名作業變更網域的 DNS 名稱時，將已重新命名之網域的舊 DNS 名稱指定為 *@no__t 1OLDDNSNAME >* 。 只有當您也使用 **/newdns**參數指定新的網域 DNS 名稱時，才可以使用此參數。                                                                                                            |
| /newdns： \<NEWDNSNAME > |                                                                                                          當網域重新命名作業變更網域的 DNS 名稱時，將已重新命名之網域的新 DNS 名稱指定為 *@no__t 1NEWDNSNAME >* 。 只有當您同時使用 **/olddns**參數來指定舊網域 DNS 名稱時，才能使用此參數。                                                                                                           |
| /oldnb： \<OLDFLATNAME > |                                                                                                        當網域重新命名作業變更網域的 NetBIOS 名稱時，將已重新命名之網域的舊 NetBIOS 名稱指定為 *@no__t 1OLDFLATNAME >* 。 只有當您使用 **/newnb**參數指定新的網域 NetBIOS 名稱時，才可以使用此參數。                                                                                                        |
| /newnb： \<NEWFLATNAME > |                                                                                                       當網域重新命名作業變更網域的 NetBIOS 名稱時，將已重新命名之網域的新 NetBIOS 名稱指定為 *@no__t 1NEWFLATNAME >* 。 只有當您使用 **/oldnb**參數指定舊網域 NetBIOS 名稱時，才可以使用此參數。                                                                                                       |
|     /dc： \<DCNAME >     | 連接到名為 *@no__t 1DCNAME*的網域控制站 > （DNS 名稱或 NetBIOS 名稱）。 *@no__t 1DCNAME >* 必須裝載網域目錄磁碟分割的可寫入複本，如下列其中一項所示：</br>-使用 **/newdns** *@no__t 1NEWDNSNAME >* 的 DNS 名稱</br>-NetBIOS 名稱 *@no__t 1NEWFLATNAME >* 使用 **/newnb**</br>如果未使用此參數，請連接到已重新命名的網域中的任何網域控制站 *\<NEWDNSNAME >* 或 *\<NEWFLATNAME >* 。 |
|        /sionly        |                                                                                                                           僅執行與受管理軟體安裝相關的群組原則修正（適用于群組原則的軟體安裝延伸模組）。 略過修正群組原則連結的動作，以及 Gpo 中的 SYSVOL 路徑。                                                                                                                           |
|   /user： \<USERNAME >   |                                                                                                                                   在使用者 *\<USERNAME >* 的安全性內容中執行此命令，其中 *@no__t 3USERNAME >* 的格式為 domain\user</br>如果未使用此參數，會以登入的使用者身分執行此命令。                                                                                                                                    |
|   /pwd： {\<PASSWORD >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  在命令提示字元顯示 [說明]。                                                                                                                                                                                                                   |

## <a name="remarks"></a>備註

-   **Gpfixup**命令可在 windows Server 2008 R2 和 windows server 2008 中使用，但 Server Core 安裝除外。
-   雖然群組原則管理主控台（GPMC）是與 Windows Server 2008 R2 和 Windows Server 2008 一起散發，但您必須透過伺服器管理員將群組原則管理安裝為功能。

## <a name="BKMK_Examples"></a>典型

這個範例假設您已執行網域重新命名作業，其中將 DNS 名稱從**MyOldDnsName**變更為**MyNewDnsName**，並將 NetBIOS 名稱從**MyOldNetBIOSName**變更為**MyNewNetBIOSName**。 在此範例中，您會使用**gpfixup**命令來連線到名為**MyDcDnsName**的網域控制站，並藉由更新內嵌在 gpo 和連結中的舊功能變數名稱來修復 gpo 和群組原則連結。 狀態和錯誤輸出會儲存到名為**gpfixup**的檔案中。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
這個範例與前一個範例相同，不同之處在于它會假設網域的 NetBIOS 名稱在網域重新命名作業期間不會變更。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>其他參考資料

-   [管理 Active Directory 網域重新命名](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [群組原則技術中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令列語法關鍵](command-line-syntax-key.md)