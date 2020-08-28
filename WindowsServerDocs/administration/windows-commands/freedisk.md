---
title: freedisk
description: Freedisk 命令的參考文章，此命令會在繼續安裝程式之前，檢查是否有指定的可用磁碟空間量。
ms.topic: reference
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3f8543e6fd2cff9a4e086068155d84526d9377
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027606"
---
# <a name="freedisk"></a>freedisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查是否有指定的可用磁碟空間量，再繼續進行安裝程式。

## <a name="syntax"></a>語法

```
freedisk [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] [/d <drive>] [<value>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 此參數會套用至命令中指定的所有檔案和資料夾。 |
| u `[<domain>\]<user>` | 使用指定之使用者帳戶的許可權來執行腳本。 預設為系統許可權。 |
| /p [ <password> ] | 指定 **/u**中指定之使用者帳戶的密碼。 |
| /d `<drive>` | 指定您想要找出可用空間可用性的磁片磁碟機。 您必須 `<drive>` 為遠端電腦指定。 |
| `<value>` | 檢查是否有特定的可用磁碟空間量。 您可以指定 `<value>` 位元組、KB、MB、GB、TB、PB、EB、ZB 或 YB。 |

#### <a name="remarks"></a>備註

- 使用 **/s**、 **/u**和 **/p** 命令列選項時，只有在您使用 **/s**時才可使用。 您必須使用 **/p** 搭配 **/u**來提供使用者的密碼。

- 若為自動安裝，您可以在安裝批次檔中使用 **freedisk** 來檢查必要條件數量的可用空間，然後再繼續進行安裝。

- 當您在批次檔中使用 **freedisk** 時，如果有足夠的空間，則會傳回 **0** ，如果沒有足夠的空間，則會傳回 **1** 。

### <a name="examples"></a>範例

若要判斷磁片磁碟機 C：上是否至少有 50 MB 的可用空間，請輸入：

```
freedisk 50mb
```

畫面上會出現類似下列的輸出：

```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
