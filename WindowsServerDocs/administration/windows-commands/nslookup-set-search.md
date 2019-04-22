---
title: nslookup set search
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf952a0337e23c0426265c6c0a4a8387a6ab45e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816989"
---
# <a name="nslookup-set-search"></a>nslookup set search



將 DNS 網域搜尋清單中的網域名稱系統 (DNS) 網域名稱附加至要求，直到收到回應為止。 這適用於包含至少一個句號，設定及搜尋要求時，但不是以句點結尾。

## <a name="syntax"></a>語法

```
set [no]search
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|**nosearch**|停止將 DNS 網域搜尋清單中的網域名稱系統 (DNS) 網域名稱附加至要求。|
|**search**|將 DNS 網域搜尋清單中的網域名稱系統 (DNS) 網域名稱附加至要求，直到收到回應為止。 預設語法是**搜尋**。|
|{說明 | ?}|顯示的簡短摘要**nslookup**子命令。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)