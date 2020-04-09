---
title: ftp lcd
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d7e2e6fc9f6af7655381bfb802dc190e79365bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843401"
---
# <a name="ftp-lcd"></a>ftp： lcd

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更本機電腦上的工作目錄。 根據預設，工作目錄是**ftp**啟動所在的目錄。   
## <a name="syntax"></a>語法  
```  
lcd [<directory>]  
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<directory>]|指定要變更之本機電腦上的目錄。 如果未指定*目錄*，則目前的工作目錄會變更為預設目錄。|  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
將本機電腦上的工作目錄變更為**C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
