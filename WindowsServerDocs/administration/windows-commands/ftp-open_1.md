---
title: ftp open_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45de8b3c210fe0925ac3cc43c41d3e092d5dfe16
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438500"
---
# <a name="ftp-open1"></a>ftp: open_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

連接到指定的 ftp 伺服器。   
## <a name="syntax"></a>語法  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>參數  

| 參數  |                                           描述                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                指定您嘗試連接的遠端電腦。                 |
|  [<Port>]  | 指定要用來連線到 ftp 伺服器的 TCP 通訊埠編號。 根據預設，會使用 TCP 通訊埠 21。 |

## <a name="remarks"></a>備註  
您可以使用 IP 位址或電腦名稱 （在此情況下的 DNS 伺服器或主機檔案必須是可用） 來指定**電腦**。  
## <a name="BKMK_Examples"></a>範例  
在連接到 ftp 伺服器**ftp.microsoft.com**。  
```  
Open ftp.microsoft.com  
```  
在連接到 ftp 伺服器**ftp.microsoft.com** 755 的 TCP 連接埠上接聽。  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
