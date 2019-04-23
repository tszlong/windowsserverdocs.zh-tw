---
title: bootcfg
description: 適用於 Windows 命令主題**bootcfg** -設定、 查詢，或變更 Boot.ini 檔案設定。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79a1c0e22a3b162ba9492c80d114b2d5b943c744
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867019"
---
# <a name="bootcfg"></a>bootcfg

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定、查詢或變更 Boot.ini 檔案設定。  
## <a name="syntax"></a>語法  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|新增作業系統載入選項，可針對指定的作業系統項目。|  
|[bootcfg copy](bootcfg-copy.md)|會建立一份現有的開機項目，您可以新增命令列選項。|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|設定 1394年連接埠的偵錯指定的作業系統項目。|  
|[bootcfg debug](bootcfg-debug.md)|加入或變更偵錯設定為指定的作業系統項目。|  
|[bootcfg default](bootcfg-default.md)|指定將指定為預設的作業系統項目。|  
|[bootcfg delete](bootcfg-delete.md)|刪除中的作業系統項目 **[operating systems]** Boot.ini 檔案區段。|  
|[bootcfg ems](bootcfg-ems.md)|可讓使用者加入或變更遠端電腦的 Emergency Management Services 主控台重新導向的設定。|  
|[bootcfg 查詢](bootcfg-query.md)|查詢，並顯示 [開機載入器] 並 **[operating systems]** 區段 Boot.ini 中的項目。|  
|[bootcfg raw](bootcfg-raw.md)|新增作業系統載入選項，可指定為字串中的作業系統項目 **[operating systems]** Boot.ini 檔案區段。|  
|[bootcfg rmsw](bootcfg-rmsw.md)|移除作業系統載入選項，針對指定的作業系統項目。|  
|[bootcfg timeout](bootcfg-timeout.md)|變更作業系統逾時值。|  
