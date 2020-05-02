---
title: freedisk
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 102d829535eb6028b2a062234f280fd32ea48943
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725549"
---
# <a name="freedisk"></a>freedisk

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在繼續進行安裝程式之前，請先檢查指定的磁碟空間容量是否可用。

## <a name="syntax"></a>語法
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
### <a name="parameters"></a>參數

|       參數       |                                                                                         描述                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s<computer>     | 指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。 這個參數會套用至命令中指定的所有檔案和資料夾。 |
| /u [<Domain>\\]<User> |                                            以指定之使用者帳戶的許可權執行腳本。 預設值為 [系統許可權]。                                            |
|    /p [<Password>]    |                                                           指定 **/u**中所指定使用者帳戶的密碼。                                                            |
|      /d<Drive>       |                              指定您想要找出可用空間可用性的磁片磁碟機。 您必須指定<Drive>遠端電腦的。                               |
|        <Value>        |                                     檢查是否有特定的可用磁碟空間量。 您可以指定 <Value>以位元組、KB、MB、GB、TB、PB、EB、ZB 或 YB。                                      |

## <a name="remarks"></a>備註
- 只有當您使用 **/s**時，才可以使用 **/s**、 **/u**和 **/p**命令列選項。 您必須使用 **/p**搭配 **/u**來提供使用者的密碼。
- 若為自動安裝，您可以使用安裝批次檔中的**freedisk** ，檢查是否有必要的可用空間量，再繼續進行安裝。
- 當您在批次檔中使用**freedisk**時，如果有足夠的空間，則會傳回**0** ，如果沒有足夠的空間，則會傳回**1** 。
  ## <a name="examples"></a>範例
  若要判斷磁片磁碟機 C：上是否有至少 50 MB 的可用空間，請輸入：
  ```
  freedisk 50mb 
  ```
  畫面上會顯示類似下列的輸出：
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>其他參考
  - [命令列語法關鍵](command-line-syntax-key.md)
