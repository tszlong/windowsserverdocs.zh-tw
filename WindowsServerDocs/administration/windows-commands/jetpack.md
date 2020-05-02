---
title: jetpack
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec29a1fd48fdba72f07fe5d00de7730d93434105
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724791"
---
# <a name="jetpack"></a>jetpack

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

壓縮 Windows 網際網路名稱服務（WINS）或動態主機設定通訊協定（DHCP）資料庫。 Microsoft 建議您在 WINS 資料庫接近 30 MB 時，將其壓縮。 

## <a name="syntax"></a>語法
```
jetpack.EXE <database name> <temp database name>
```

#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<database name>|指定原始資料庫檔案。|
|<temp database name>|指定暫存資料庫檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例
若要壓縮 WINS 資料庫：
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
若要壓縮 DHCP 資料庫：
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
在上述範例中， **Tmp**是 jetpack 所使用的暫存資料庫。 **Wins**是 wins 資料庫。 **Dhcp**是 dhcp 資料庫。
jetpack 會藉由執行下列動作來壓縮 WINS 或 DHCP 資料庫：
1.  將資料庫資訊複製到名為**Tmp**的暫存資料庫檔案。
2.  刪除原始資料庫檔案，**勝出**或**Dhcp**.mdb。
3.  將暫存資料庫檔案重新命名為原始的檔案名。

> [!NOTE]
> 在 compact 程式期間，jetpack 會使用*temp 資料庫名稱*參數所指定的名稱來建立暫存檔案。 當 compact 程式完成時，就會移除暫存檔案。 請確定您的 WINS 或 DHCP 資料夾中沒有已存在的檔案，其名稱與*temp 資料庫名稱*參數中指定的相同。

## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)
