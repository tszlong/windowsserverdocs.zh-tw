---
title: cmdkey
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2b12cb53eef930d05c1e291de5574a8ba94306
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379314"
---
# <a name="cmdkey"></a>cmdkey

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立、列出和刪除儲存的使用者名稱和密碼或認證。

## <a name="syntax"></a>語法
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>參數

|             參數             |                                                                                    描述                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /add： <TargetName>          | 將使用者名稱和密碼加入清單中。<br /><br />需要 <TargetName> 的參數，它會識別與此專案相關聯的電腦或功能變數名稱。 |
|       /一般： <TargetName>        |   將一般認證加入清單中。<br /><br />需要 <TargetName> 的參數，它會識別與此專案相關聯的電腦或功能變數名稱。    |
|             /smartcard             |                                                                    從智慧卡抓取認證。                                                                     |
|          /user： <UserName>          |                                 指定要與此專案一起儲存的使用者或帳戶名稱。 如果未提供使用者*名稱*，則會要求使用者。                                  |
|          /pass： <Password>          |                                       指定要與此專案一起儲存的密碼。 如果未提供*密碼*，則會要求它。                                        |
| /delete{： <TargetName> &#124; /ras} |  刪除清單中的使用者名稱和密碼。 如果指定了*TargetName* ，將會刪除該專案。 如果指定/ras，將會刪除儲存的遠端存取專案。   |
|         /list： <TargetName>         |                  顯示已儲存的使用者名稱和認證清單。 如果未指定*TargetName* ，則會列出所有儲存的使用者名稱和認證。                   |
|                 /?                 |                                                                        在命令提示字元顯示說明。                                                                        |

## <a name="remarks"></a>備註
- 當使用/smartcard 命令列選項時，如果在系統上找到一個以上的智慧卡， **cmdkey**將會顯示所有可用智慧卡的相關資訊，然後提示使用者指定要使用哪一個。
- 儲存之後，將不會顯示密碼。
  ## <a name="BKMK_examples"></a>典型
  若要顯示所有已儲存的使用者名稱和認證清單，請輸入：
  ```
  cmdkey /list
  ```
  若要新增使用者 Mikedan 的使用者名稱和密碼，以使用密碼 Kleo 來存取電腦 Server01，請輸入：
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  若要新增使用者 Mikedan 的使用者名稱和密碼以存取電腦 Server01，並在每次存取 Server01 時提示輸入密碼，請輸入：
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  若要刪除遠端存取已儲存的認證，請輸入：
  ```
  cmdkey /delete /ras
  ```
  若要刪除為 Server01 儲存的認證，請輸入：
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>其他參考
  [命令列語法關鍵](command-line-syntax-key.md)
