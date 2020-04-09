---
title: ftp prompt_1
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d16f70226e91e2e845480be8d83481fd76af173
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843151"
---
# <a name="ftp-prompt_1"></a>ftp： prompt_1

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在**提示**模式的開啟和關閉之間切換。   
## <a name="syntax"></a>語法  
```  
prompt  
```  
#### <a name="parameters"></a>參數  
無  
## <a name="remarks"></a>備註  
- 預設會開啟 [**提示**]。  
- 在多個檔案傳輸期間的**ftp**提示可讓您選擇性地取出或儲存檔案。  如果**prompt**已關閉， **Mget**和**mput**會傳輸所有檔案。  
  ## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  切換開啟和關閉提示模式。  
  ```  
  prompt  
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- - [命令列語法關鍵](command-line-syntax-key.md)  
