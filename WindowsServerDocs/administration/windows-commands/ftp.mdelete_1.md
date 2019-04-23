---
title: ftp mdelete_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9390f8e5b0bded92a553b3057d122dace3fc665a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851609"
---
# <a name="ftp-mdelete1"></a>ftp: mdelete_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

刪除遠端電腦上的檔案。   
## <a name="syntax"></a>語法  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<remoteFile>|指定要刪除的遠端檔案。|  
## <a name="BKMK_Examples"></a>範例  
刪除遠端檔案**a.exe**並**b.exe**。  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
