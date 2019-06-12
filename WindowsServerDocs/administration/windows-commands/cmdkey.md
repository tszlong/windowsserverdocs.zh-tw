---
title: cmdkey
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5c06a04fa6473bc30c3b354f049a55775d2308a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434309"
---
# <a name="cmdkey"></a>cmdkey

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立、列出和刪除儲存的使用者名稱和密碼或認證。

## <a name="syntax"></a>語法
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>參數

|             參數             |                                                                                    描述                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         / 新增：<TargetName>          | 將清單中的使用者名稱和密碼。<br /><br />需要的參數<TargetName>識別此項目相關聯的電腦或網域名稱。 |
|       / 一般：<TargetName>        |   將清單中的泛用的認證。<br /><br />需要的參數<TargetName>識別此項目相關聯的電腦或網域名稱。    |
|             /smartcard             |                                                                    從智慧卡中擷取的認證。                                                                     |
|          /user:<UserName>          |                                 指定要與這個項目儲存的使用者或帳戶名稱。 如果*UserName*是未提供，它會要求。                                  |
|          /pass:<Password>          |                                       指定要與這個項目儲存的密碼。 如果*密碼*是未提供，它會要求。                                        |
| /delete{:<TargetName> &#124; /ras} |  從清單中刪除的使用者名稱和密碼。 如果*TargetName*指定，將會刪除項目。 如果指定 /ras，就會刪除儲存的遠端存取項目。   |
|         /list:<TargetName>         |                  顯示預存的使用者名稱和認證的清單。 如果*TargetName*不能指定，所有預存使用者名稱和認證將會列出。                   |
|                 /?                 |                                                                        在命令提示字元顯示說明。                                                                        |

## <a name="remarks"></a>備註
- 如果使用 /smartcard 命令列選項時，在系統上找到一個以上的智慧卡**cmdkey**會顯示所有可用的智慧卡的相關資訊，然後提示使用者指定要使用哪一個。
- 一旦儲存之後，不會顯示密碼。
  ## <a name="BKMK_examples"></a>範例
  若要顯示的所有使用者名稱和儲存的認證清單，請輸入：
  ```
  cmdkey /list
  ```
  若要加入使用密碼 Kleo 存取電腦 Server01 的使用者名稱和密碼，使用者 Mikedan，請輸入：
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  若要新增的使用者名稱和 使用者存取電腦 Server01 Mikedan 密碼和密碼的提示字元，每當 Server01 存取時，請輸入：
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  若要刪除遠端存取已儲存的認證，請輸入：
  ```
  cmdkey /delete /ras
  ```
  若要刪除 Server01 為所儲存的認證，請輸入：
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>其他參考資料
  [命令列語法關鍵](command-line-syntax-key.md)
