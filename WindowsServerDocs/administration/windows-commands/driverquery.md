---
title: driverquery
description: Driverquery 命令的參考文章，可讓系統管理員顯示已安裝的設備磁碟機及其屬性的清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ad0a028217e07d8c15b59dc96e31c8f236dd743
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931465"
---
# <a name="driverquery"></a>driverquery

可讓系統管理員顯示已安裝的設備磁碟機及其內容的清單。 如果在沒有參數的情況下使用， **driverquery**會在本機電腦上執行。

## <a name="syntax"></a>語法

```
driverquery [/s <system> [/u [<domain>\]<username> [/p <password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- |------------ |
| /s`<system>` | 指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。 |
| u`[<domain>]<username>` | 以*user*或*domain\user*所指定的使用者帳號憑證來執行命令。 根據預設， */s*會使用目前登入發出命令之電腦的使用者認證。 除非指定了 **/s** ，否則無法使用 **/u** 。 |
| /p`<password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 除非指定 **/u** ，否則無法使用 **/p** 。 |
| /fo 資料表 | 將輸出格式化為表格。 此為預設值。 |
| /fo 清單 | 將輸出格式化為清單。 |
| /fo csv | 使用逗號分隔值來格式化輸出。 |
| /nh | 省略顯示的驅動程式資訊中的標頭資料列。 如果 **/fo**參數設定為**list**，則無效。 |
| /v | 顯示詳細資訊輸出。 **/v**對已簽署的驅動程式無效。 |
| /si | 提供已簽署驅動程式的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要顯示本機電腦上已安裝的設備磁碟機清單，請輸入：

```
driverquery
```

若要以逗號分隔值（CSV）格式顯示輸出，請輸入：

```
driverquery /fo csv
```

若要隱藏輸出中的標頭資料列，請輸入：

```
driverquery /nh
```

若要在本機電腦上使用目前的認證，在名為*server1*的遠端伺服器上使用**driverquery**命令，請輸入：

```
driverquery /s server1
```

若要在名為*server1*的遠端伺服器上使用**driverquery**命令，並使用 domain *maindom*上*user1*的認證，請輸入：

```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)