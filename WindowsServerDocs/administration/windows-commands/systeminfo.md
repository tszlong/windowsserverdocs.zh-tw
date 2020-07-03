---
title: systeminfo
description: Systeminfo 的參考文章，其中會顯示有關電腦及其作業系統的詳細設定資訊，包括作業系統設定、安全性資訊、產品識別碼及硬體內容（例如 RAM、磁碟空間和網路卡）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 670fdb94a2ccb10476faccab8f265a5e4db060dd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932576"
---
# <a name="systeminfo"></a>systeminfo

顯示電腦及其作業系統的詳細設定資訊，包括作業系統設定、安全性資訊、產品識別碼和硬體內容（例如 RAM、磁碟空間和網路卡）。



## <a name="syntax"></a>語法

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/s\<Computer>|指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。|
|u\<Domain>\<UserName>|使用指定之使用者帳戶的帳戶許可權來執行命令。 如果未指定 **/u** ，此命令會使用目前登入正在發出命令之電腦的使用者許可權。|
|/p\<Password>|指定 **/u**參數中指定之使用者帳戶的密碼。|
|/fo\<Format>|使用下列其中一個值來指定輸出格式：</br>TABLE：在資料表中顯示輸出。</br>LIST：在清單中顯示輸出。</br>CSV：以逗號分隔值格式顯示輸出。|
|/nh|隱藏輸出中的資料行標頭。 當 **/fo**參數設定為 TABLE 或 CSV 時有效。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

若要查看名為 Srvmain 之電腦的設定資訊，請輸入：

**systeminfo/s srvmain**

若要從遠端查看位於 Maindom 網域上名為 Srvmain2 之電腦的設定資訊，請輸入：

**systeminfo/s srvmain2/u maindom\hiropln**

若要從遠端查看位於 Maindom 網域且名為 Srvmain2 之電腦的設定資訊（清單格式），請輸入：

**systeminfo/s srvmain2/u maindom\hiropln/p p@ssW23 /fo list**

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)