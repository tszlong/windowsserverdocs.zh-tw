---
title: serverceipoptin
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0625694053a2d673cd86e12d6f6d54662a6c81d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834677"
---
# <a name="serverceipoptin"></a>serverceipoptin

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您參與客戶經驗改進計畫（CEIP）。
## <a name="syntax"></a>語法
```
serverceipoptin [/query] [/enable] [/disable]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/query|驗證目前的設定。|
|/enable|啟用參與。|
|/disable|停用參與。|
|/?|在命令提示字元顯示說明。|
## <a name="examples"></a><a name=BKMK_Examples></a>典型
若要確認目前的設定，請輸入：
```
serverceipoptin /query
```
若要啟用參與，請輸入：
```
serverceipoptin /enable
```
若要停用參與，請輸入：
```
serverceipoptin /disable
```
## <a name="additional-references"></a>其他參考資料
-   - [命令列語法關鍵](command-line-syntax-key.md)

