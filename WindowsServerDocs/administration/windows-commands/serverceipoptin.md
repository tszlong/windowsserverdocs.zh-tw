---
title: serverceipoptin
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25a7b116387af973a8c7894edbd0daed0dc59f9a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024962"
---
# <a name="serverceipoptin"></a>serverceipoptin

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您參與客戶經驗改進計畫 (CEIP) 。
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
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

