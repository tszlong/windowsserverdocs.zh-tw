---
title: nslookup set timeout
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8506511fc203f94d395851471f6a981ef0765928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838271"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更等候查閱要求回復的初始秒數。
## <a name="syntax"></a>語法
```
set timeout=<Number>
```
### <a name="parameters"></a>參數

|    參數    |                                           描述                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | 指定等待回復的秒數。 預設等候的秒數為5。 |
| {help &#124; ？} |                      顯示**nslookup**子命令的簡短摘要。                       |

## <a name="remarks"></a>備註
- 在指定的時段內未收到要求的回復時，會將超時時間加倍，並再次傳送要求。 您可以使用 [**設定重試**] 命令來控制重試次數。
  ## <a name="examples"></a><a name=BKMK_examples></a>典型
  下列範例會將取得回應的時間設定為2秒：
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 設定重試](nslookup-set-retry.md)
