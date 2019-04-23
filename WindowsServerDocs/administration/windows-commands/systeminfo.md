---
title: systeminfo
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841499"
---
# <a name="systeminfo"></a>systeminfo



顯示詳細的電腦和其作業系統，包括作業系統設定、 安全性資訊、 產品識別碼及硬體屬性 （例如 RAM、 磁碟空間和網路卡） 的組態資訊。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s\<電腦 >|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u\<網域 >\<使用者名稱 >|指定的使用者帳戶的帳戶權限執行命令。 如果 **/u**未指定，此命令會使用目前登入的電腦，會發出命令之使用者的權限。|
|/p \<Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/fo\<格式 >|指定的輸出格式具有下列值之一：</br>資料表：輸出資料表中的顯示。</br>清單：輸出顯示在清單中。</br>CSV:以逗點分隔值格式的顯示輸出。|
|/nh|隱藏在輸出中的資料行標頭。 時才有效 **/fo**參數設定為資料表或 CSV。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要檢視名為 Srvmain 之電腦的組態資訊，請輸入：

**systeminfo /s srvmain**

若要從遠端檢視名為 Srvmain2 位於 Maindom 網域之電腦的組態資訊，請輸入：

**systeminfo /s srvmain2 /u maindom\hiropln**

若要從遠端檢視名為 Srvmain2 位於 Maindom 網域的電腦 （以清單格式） 組態資訊，請輸入：

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)