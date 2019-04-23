---
title: nslookup set timeout
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7a35a4a8e9e9cc10ea5548875f710fb5a86036
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837809"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更初始的等候回覆的查閱要求的秒數。
## <a name="syntax"></a>語法
```
set timeout=<Number>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<Number>|指定等候回覆的秒數。 要等候的秒數的預設數目為 5。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   在指定的時間週期內未收到要求的回覆，逾時增加一倍，並要求就會再次傳送。 您可以使用**集重試**命令控制重試次數。
## <a name="BKMK_examples"></a>範例
下列範例會設定取得回應為 2 秒的逾時：
```
set timeout=2
```
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定重試](nslookup-set-retry.md)
