---
title: driverquery
description: Driverquery 命令的參考文章，可讓系統管理員顯示已安裝的設備磁碟機及其屬性清單。
ms.topic: reference
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbd8ca2de7f15a5b5fb8682dae3a3aa2e105d7cd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030796"
---
# <a name="driverquery"></a>driverquery

可讓系統管理員顯示已安裝的設備磁碟機及其屬性清單。 如果使用時不含參數， **driverquery** 會在本機電腦上執行。

## <a name="syntax"></a>語法

```
driverquery [/s <system> [/u [<domain>\]<username> [/p <password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| /s `<system>` | 指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。 |
| u `[<domain>]<username>` | 使用 *user* 或 *domain\user*所指定之使用者帳戶的認證來執行命令。 根據預設， */s* 會使用目前登入發出命令之電腦的使用者認證。 除非指定了 **/s** ，否則無法使用 **/u** 。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 除非指定 **/u** ，否則無法使用 **/p** 。 |
| /fo 資料表 | 將輸出格式化為表格。 這是預設值。 |
| /fo 清單 | 將輸出格式化為清單。 |
| /fo csv | 使用逗點分隔值將輸出格式化。 |
| /nh | 從顯示的驅動程式資訊中省略標頭資料列。 如果 **/fo** 參數設定為 **list**，則無效。 |
| /v | 顯示詳細資訊輸出。 **/v** 簽署的驅動程式無效。 |
| /si | 提供簽署驅動程式的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要顯示本機電腦上已安裝的設備磁碟機清單，請輸入：

```
driverquery
```

若要以逗點分隔值顯示輸出 (CSV) 格式，請輸入：

```
driverquery /fo csv
```

若要隱藏輸出中的標頭資料列，請輸入：

```
driverquery /nh
```

若要使用本機電腦上目前的認證，在名為*server1*的遠端伺服器上使用**driverquery**命令，請輸入：

```
driverquery /s server1
```

若要在名為*server1*的遠端伺服器上使用**driverquery**命令，請在網域*maindom*上使用*user1*的認證，輸入：

```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)