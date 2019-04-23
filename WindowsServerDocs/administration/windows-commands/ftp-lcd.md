---
title: ftp lcd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eac028c8aa675e680dedefcfe9f0b8da18ce7179
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826889"
---
# <a name="ftp-lcd"></a>ftp: lcd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更本機電腦上的工作目錄。 根據預設，工作目錄是在其中的目錄**ftp**已啟動。   
## <a name="syntax"></a>語法  
```  
lcd [<directory>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<directory>]|若要變更要在本機電腦上指定的目錄。 如果*directory*未指定，目前的工作目錄變更為預設目錄。|  
## <a name="BKMK_Examples"></a>範例  
變更本機電腦上的工作目錄**C:\dir1**  
```  
lcd C:\dir1  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
