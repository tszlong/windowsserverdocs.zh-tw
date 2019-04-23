---
title: getmac
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e356354e63a057201582db0fb74933e1b3ef8d8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851039"
---
# <a name="getmac"></a>getmac

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

傳回媒體存取控制 (MAC) 位址和網路通訊協定相關聯所有的網路卡，在每台電腦，每個地址可能是在本機或網路上的清單。 
## <a name="syntax"></a>語法
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/s <computer>|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。|
|/u <Domain>\\<User>|使用指定之使用者的使用者或網域 \ 使用者帳戶權限執行命令。 預設值是目前登入的使用者發出命令的電腦上的權限。|
|/p <Password>|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/fo { TABLE &#124; list&#124; CSV}|指定要用於查詢的輸出格式。 有效值**表格**，**清單**，並**CSV**。 輸出的預設格式**資料表**。|
|/nh|隱藏在輸出中的資料行標頭。 時才有效 **/fo**參數設為**表格**或是**CSV**。|
|/v|指定輸出會顯示詳細的資訊。|
|/?||
## <a name="remarks"></a>備註
**getmac**可以是很有用，當您想要進入網路分析器中的 MAC 位址或當您需要知道哪些通訊協定目前正在使用中的電腦中的每個網路介面卡上。
## <a name="BKMK_Examples"></a>範例
下列範例示範如何使用**getmac**命令：
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
