---
title: jetpack
description: Jetpack 命令的參考文章，此命令會將 Windows 網際網路名稱服務壓縮 (WINS) 或動態主機設定通訊協定 (DHCP) database。
ms.topic: reference
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 300218e6eb98e2cf5ab7adeda1b31bc0da0b026b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634401"
---
# <a name="jetpack"></a>jetpack

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 (DHCP) 資料庫，將 Windows 網際網路名稱服務 (WINS) 或動態主機設定通訊協定壓縮。 建議您在接近 30 MB 的情況時壓縮 WINS 資料庫。

Jetpack.exe 藉由下列方法壓縮資料庫：

1. 將資料庫資訊複製到暫存資料庫檔案。

2. 正在刪除原始資料庫檔案（WINS 或 DHCP）。

3. 將暫存資料庫檔案重新命名為原始檔案名。

## <a name="syntax"></a>語法

```
jetpack.exe <database_name> <temp_database_name>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| `<database_name>` | 指定原始資料庫檔案的名稱。 |
| `<temp_database_name>` | 指定 jetpack.exe 所要建立的暫存資料庫檔案名。<p>注意：壓縮程式完成時，將會移除此暫存檔案。 若要讓此命令正常運作，您必須確定您的暫存檔案名稱是唯一的，而且具有該名稱的檔案尚未存在。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要壓縮 WINS 資料庫，其中 **Tmp** 是暫存資料庫，而 **wins** 是 wins 資料庫，請輸入：

```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack Wins.mdb Tmp.mdb
NET start WINS
```

若要壓縮 DHCP 資料庫，其中 **Tmp** 是暫存資料庫，而 **DHCP** 則為 dhcp 資料庫，請輸入：

```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERVER
jetpack Dhcp.mdb Tmp.mdb
NET start DHCPSERVER
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
