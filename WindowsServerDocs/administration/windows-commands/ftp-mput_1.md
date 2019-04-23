---
title: ftp mput_1
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99b938618deb2d1e779fd20c504c01a13a2d3f8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868159"
---
# <a name="ftp-mput1"></a>ftp: mput_1

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本機檔案複製到遠端電腦使用目前的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<LocalFile>|指定要複製到遠端電腦的本機檔案。|  
## <a name="BKMK_Examples"></a>範例  
複製**Program1.exe**並**Program2.exe**到遠端電腦使用目前的檔案傳輸類型。  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
