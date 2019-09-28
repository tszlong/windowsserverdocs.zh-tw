---
title: netcfg
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f8368aaff16592a55cc9def84d593cf323f28ee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373286"
---
# <a name="netcfg"></a>netcfg

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

會安裝 Windows 預先安裝環境（WinPE），這是用來部署工作站的輕量版本 Windows。   
## <a name="syntax"></a>語法  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/v|以詳細資訊（詳細）模式執行|  
|/e|在安裝和卸載期間使用服務環境變數|  
|/winpe|安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境|  
|/l|提供 INF 的位置|  
|/c|提供要安裝之元件的類別;通訊協定、服務或用戶端|  
|/i|提供元件識別碼|  
|/s|提供要顯示的元件類型<br /><br />\ta = 介面卡，n = net 元件|  
|/?|在命令提示字元顯示說明。|  
## <a name="BKMK_Examples"></a>典型  
若要使用 c:\oemdir\example.inf 安裝通訊協定*範例*：  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
若要安裝*MS_Server*服務：  
```  
netcfg /c s /i MS_Server  
```  
安裝 TCP/IP、NetBIOS 和 Microsoft Client for Windows 預先安裝環境  
```  
netcfg /v /winpe  
```  
若要顯示元件*MS_IPX*是否已安裝：  
```  
netcfg /q MS_IPX  
```  
若要卸載元件*MS_IPX*：  
```  
netcfg /u MS_IPX  
```  
若要顯示所有已安裝的 net 元件：  
```  
netcfg /s n  
```  
若要顯示包含*MS_TCPIP*的系結路徑：  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>其他參考  
-   [命令列語法關鍵](command-line-syntax-key.md)  
