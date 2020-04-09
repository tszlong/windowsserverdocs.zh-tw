---
title: unlodctr
description: 適用于 unlodctr 的 Windows 命令主題，它會從系統登錄移除效能計數器名稱，並說明服務或設備磁碟機的文字。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe7fc3c9eafefd59a5daab625e3af06b6addd292
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832255"
---
# <a name="unlodctr"></a>unlodctr

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從系統登錄移除**效能計數器**名稱，並**說明**服務或設備磁碟機的文字。   

## <a name="syntax"></a>語法  
```  
Unlodctr <DriverName>   
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|\<DriverName >|從 Windows Server 2003 登錄中移除效能計數器名稱設定，並說明驅動程式或服務 <DriverName> 的文字。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
> [!WARNING]  
> 不當編輯登錄可能會造成系統嚴重受損。 變更登錄之前，您應該先備份電腦所有的重要資料。  

如果您提供的資訊包含空格，請使用引號括住文字（例如 <DriverName>）。  

## <a name="examples"></a><a name=BKMK_Examples></a>典型  
若要移除「簡易郵件傳送通訊協定（SMTP）」服務的目前效能登錄設定和計數器解說文字：  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
  
