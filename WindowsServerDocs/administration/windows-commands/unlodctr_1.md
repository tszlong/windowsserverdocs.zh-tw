---
title: unlodctr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886649"
---
# <a name="unlodctr"></a>unlodctr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從系統登錄移除效能計數器名稱和解說文字服務或裝置驅動程式。   

## <a name="syntax"></a>語法  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|\<DriverName>|移除效能計數器名稱設定，並說明驅動程式或服務的文字<DriverName>從 Windows Server 2003 登錄。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
> [!WARNING]  
> 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。  

如果您提供的資訊包含空格，請使用引號括住的文字 (例如，"<DriverName>」)。  

## <a name="BKMK_Examples"></a>範例  
若要移除目前的效能登錄設定和計數器的 Simple Mail Transfer Protocol (SMTP) 服務的說明文字：  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
