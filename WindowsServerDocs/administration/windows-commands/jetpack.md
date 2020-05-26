---
title: jetpack
description: Jetpack 命令的參考主題，它會壓縮 Windows 網際網路名稱服務（WINS）或動態主機設定通訊協定（DHCP）資料庫。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d77f9c964f5820fc7a44b803bb765e94cb35637
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818248"
---
# <a name="jetpack"></a>jetpack

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

壓縮 Windows 網際網路名稱服務（WINS）或動態主機設定通訊協定（DHCP）資料庫。 我們建議您在 WINS 資料庫接近 30 MB 時，將其壓縮。

Jetpack 會壓縮資料庫，方法如下：

1. 將資料庫資訊複製到暫存資料庫檔案。

2. 正在刪除原始資料庫檔案，即 WINS 或 DHCP。

3. 將暫存資料庫檔案重新命名為原始的檔案名。

## <a name="syntax"></a>語法

```
jetpack.exe <database_name> <temp_database_name>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ------- | -------- |
| `<database_name>` | 指定原始資料庫檔案的名稱。 |
| `<temp_database_name>` | 指定 jetpack 要建立之暫存資料庫檔案的名稱。<p>注意： compact 程式完成時，會移除此暫存檔案。 若要讓此命令正常運作，您必須確定暫存檔案名稱是唯一的，而且具有該名稱的檔案尚未存在。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要壓縮 WINS 資料庫，其中**Tmp**是暫存資料庫，而**WINS**則是 wins 資料庫，請輸入：

```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack Wins.mdb Tmp.mdb
NET start WINS
```

若要壓縮 DHCP 資料庫，其中**Tmp**為暫存資料庫，而**dhcp**為 dhcp 資料庫，請輸入：

```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERVER
jetpack Dhcp.mdb Tmp.mdb
NET start DHCPSERVER
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
