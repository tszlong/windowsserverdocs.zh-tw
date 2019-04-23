---
title: ftp mget
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1160ec742dde318141da720bd35b7d60ab805bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888419"
---
# <a name="ftp-mget"></a>ftp: mget

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

遠端檔案複製到本機電腦使用目前的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<remoteFile>|指定要複製到本機電腦的遠端檔案。|  
## <a name="BKMK_Examples"></a>範例  
將遠端檔案複製**a.exe**並**b.exe**到本機電腦使用目前的檔案傳輸類型。  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
