---
title: nslookup set retry
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63d72a45c33da099c5936d625b27aa71ef002280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857659"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定重試次數。
## <a name="syntax"></a>語法
```
set retry=<Number>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<Number>|指定的重試次數的新值。 預設的重試次數為 4。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   在一段時間內未收到要求的回覆，逾時期限會加倍，並且在重新傳送要求。 重試值控制放棄之前重新傳送要求時的次數。 您可以變更使用的逾時期限**設定逾時**子命令。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定逾時](nslookup-set-timeout.md)
