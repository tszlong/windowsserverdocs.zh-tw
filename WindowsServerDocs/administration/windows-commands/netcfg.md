---
title: netcfg
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871319"
---
# <a name="netcfg"></a>netcfg

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

安裝 Windows 預先安裝環境 (WinPE)，輕量版的 Windows 用來部署工作站。   
## <a name="syntax"></a>語法  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/v|在 詳細資訊 （詳細） 模式中執行|  
|/e|在安裝期間使用服務的環境變數，並解除安裝|  
|/winpe|TCP/IP NetBIOS，Microsoft 用戶端安裝 Windows 預先安裝環境|  
|/l|提供 INF 的位置|  
|/c|提供安裝; 元件的類別通訊協定、 服務或用戶端|  
|/i|提供的元件識別碼|  
|/s|提供要顯示元件的類型<br /><br />\ta = 配接器，n = net 的元件|  
|/?|在命令提示字元顯示說明。|  
## <a name="BKMK_Examples"></a>範例  
若要安裝之通訊協定*範例*使用 c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
若要安裝*MS_Server*服務：  
```  
netcfg /c s /i MS_Server  
```  
若要安裝 Windows 預先安裝環境的 TCP/IP NetBIOS，Microsoft 用戶端  
```  
netcfg /v /winpe  
```  
若要顯示元件*MS_IPX*安裝：  
```  
netcfg /q MS_IPX  
```  
若要解除安裝元件*MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
若要顯示所有安裝 net 的元件：  
```  
netcfg /s n  
```  
要繫結包含路徑的顯示*MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
