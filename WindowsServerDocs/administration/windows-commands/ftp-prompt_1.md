---
title: ftp prompt_1
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08c7f9c14f4168bb5d3aa874711669eede8d0d87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376196"
---
# <a name="ftp-prompt_1"></a>ftp： prompt_1

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在**提示**模式的開啟和關閉之間切換。   
## <a name="syntax"></a>語法  
```  
prompt  
```  
### <a name="parameters"></a>參數  
無  
## <a name="remarks"></a>備註  
- 預設會開啟 [**提示**]。  
- 在多個檔案傳輸期間的**ftp**提示可讓您選擇性地取出或儲存檔案。  如果**prompt**已關閉， **Mget**和**mput**會傳輸所有檔案。  
  ## <a name="BKMK_Examples"></a>典型  
  切換開啟和關閉提示模式。  
  ```  
  prompt  
  ```  
  ## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
