---
title: jetpack
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3bffc29519df139921bdb1de53e67acd558b306
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858009"
---
# <a name="jetpack"></a>jetpack

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

壓縮 Windows 網際網路名稱服務 (WINS) 或動態主機設定通訊協定 (DHCP) 的資料庫。 Microsoft 建議您壓縮 WINS 資料庫，每當它接近 30MB。 

## <a name="syntax"></a>語法
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<database name>|指定原始資料庫檔案。|
|<temp database name>|指定的暫存資料庫檔案。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_Examples"></a>範例
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
在上述範例**Tmp.mdb**是暫存資料庫，可由 jetpack.exe。 **Wins.mdb**是 WINS 資料庫。 **Dhcp.mdb**是 DHCP 資料庫。
jetpack.exe 會壓縮 WINS 或 DHCP 資料庫，執行下列動作：
1.  複製資料庫到稱為的暫存資料庫檔案的資訊**Tmp.mdb**。
2.  刪除原始資料庫檔案**Wins.mdb**或是**Dhcp.mdb**。
3.  將暫存資料庫檔案的重新命名為原始的檔案名稱。

> [!NOTE]
> 在壓縮過程中，jetpack.exe，請建立具有所指定名稱的暫存檔*暫存資料庫名稱*參數。 Compact 的程序完成時，會移除暫存檔案。 請確定您沒有已存在於 WINS 或 DHCP 的檔案與所指定的名稱相同的資料夾*暫存資料庫名稱*參數。

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
