---
title: serverceipoptin
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d4214b9105e04f355bd6e09aeb7bc671ae6007d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721969"
---
# <a name="serverceipoptin"></a>serverceipoptin

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="examples"></a>範例
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
## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)

