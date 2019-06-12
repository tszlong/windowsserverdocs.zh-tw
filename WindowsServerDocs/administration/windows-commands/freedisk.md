---
title: freedisk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac2f38fb1948a26ae55713ac6fb14254851b062a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439165"
---
# <a name="freedisk"></a>freedisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

檢查指定的磁碟空間量是否可用，然後再繼續安裝程序。

## <a name="syntax"></a>語法
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>參數

|       參數       |                                                                                         描述                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     | 指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。 此參數適用於所有檔案和命令中指定的資料夾。 |
| /u [<Domain>\\]<User> |                                            使用指定的使用者帳戶的權限執行指令碼。 預設值是系統權限。                                            |
|    /p [<Password>]    |                                                           指定在指定的使用者帳戶的密碼 **/u**。                                                            |
|      /d <Drive>       |                              指定您要找出剩下的可用空間的磁碟機。 您必須指定<Drive>遠端電腦。                               |
|        <Value>        |                                     一段特定的可用磁碟空間檢查。 您可以指定 <Value>以位元組為單位，KB、 MB、 GB、 TB、 PB、 EB、 ZB 或 YB。                                      |

## <a name="remarks"></a>備註
- 使用 **/s**， **/u**，並 **/p**命令列選項可只當您使用 **/s**。 您必須使用 **/p**具有 **/u**提供使用者的密碼。
- 自動安裝，您可以使用**freedisk**檢查必要數量的可用空間，再繼續進行安裝的安裝批次檔中。
- 當您使用**freedisk**批次檔，它會傳回**0**如果沒有足夠的空間，且**1**如果沒有足夠的空間。
  ## <a name="BKMK_examples"></a>範例
  若要判斷是否至少 50 MB 的可用空間上有磁碟機 c:，請輸入：
  ```
  freedisk 50mb 
  ```
  如下所示的輸出會顯示在螢幕上：
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>其他參考資料
  [命令列語法關鍵](command-line-syntax-key.md)
