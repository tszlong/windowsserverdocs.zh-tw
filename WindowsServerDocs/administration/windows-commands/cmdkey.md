---
title: cmdkey
description: Cmdkey 命令的參考文章，它會建立、列出及刪除已儲存的使用者名稱和密碼或認證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7a5446a692719a55575853df6aebc78733135b8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929838"
---
# <a name="cmdkey"></a>cmdkey

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立、列出和刪除儲存的使用者名稱和密碼或認證。

## <a name="syntax"></a>語法

```
cmdkey [{/add:<targetname>|/generic:<targetname>}] {/smartcard | /user:<username> [/pass:<password>]} [/delete{:<targetname> | /ras}] /list:<targetname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ---------- | ----------- |
| /add`<targetname>` | 將使用者名稱和密碼加入清單中。<p>需要 `<targetname>` 可識別此專案將關聯之電腦或功能變數名稱的參數。 |
| /一般`<targetname>` | 將一般認證加入清單中。<p>需要 `<targetname>` 可識別此專案將關聯之電腦或功能變數名稱的參數。 |
| /smartcard | 從智慧卡抓取認證。 使用此選項時，如果在系統上找到一個以上的智慧卡， **cmdkey**會顯示所有可用智慧卡的相關資訊，然後提示使用者指定要使用哪一個。 |
| /user`<username>` | 指定要與此專案一起儲存的使用者或帳戶名稱。 如果 `<username>` 未提供，則會要求它。 |
|密碼`<password>` | 指定要與此專案一起儲存的密碼。 如果 `<password>` 未提供，則會要求它。 密碼在儲存後不會顯示。 |
| /delete{:`<targetname>` | ra | 刪除清單中的使用者名稱和密碼。 如果 `<targetname>` 指定了，就會刪除該專案。 如果 `/ras` 指定了，就會刪除儲存的遠端存取專案。 |
| /list`<targetname>` | 顯示已儲存的使用者名稱和認證清單。 如果 `<targetname>` 未指定，則會列出所有儲存的使用者名稱和認證。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要顯示所有已儲存的使用者名稱和認證清單，請輸入：

```
cmdkey /list
```

若要新增使用者*Mikedan*的使用者名稱和密碼，以使用密碼*Kleo*來存取電腦*Server01* ，請輸入：

```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```

若要新增使用者*Mikedan*的使用者名稱和密碼以存取電腦*Server01* ，並在每次存取 Server01 時提示輸入密碼，請輸入：

```
cmdkey /add:server01 /user:mikedan
```

若要刪除遠端存取所儲存的認證，請輸入：

```
cmdkey /delete /ras
```

若要刪除*Server01*所儲存的認證，請輸入：

```
cmdkey /delete:server01
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
