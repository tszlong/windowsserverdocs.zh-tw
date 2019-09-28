---
title: ftp 類型
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb254d1c9b17ac6baadf6b84702d2812f1117a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375904"
---
# <a name="ftp-type"></a>ftp：類型

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定或顯示檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>參數  

|  參數   |            描述            |
|--------------|-----------------------------------|
| [<typeName>] | 指定檔案傳輸類型。 |

## <a name="remarks"></a>備註  
- 如果未指定*typeName* ，則會顯示目前的類型。  
- **ftp**支援兩種檔案傳輸類型： ASCII 和二進位檔。  
  預設的檔案傳輸類型為 ASCII。  傳送文字檔時，應該使用**ascii**命令。 在 ASCII 模式中，會執行與網路標準字元集之間的字元轉換。 例如，會根據目的地的作業系統，將行尾字元轉換為必要。  
  傳輸可執行檔時，應該使用**二進位**命令。 在二進位模式中，檔案會以一個位元組的單位移動。  
  ## <a name="BKMK_Examples"></a>典型  
  將檔案傳輸類型設定為 ASCII。  
  ```  
  type ascii  
  ```  
  將 [傳輸檔案類型] 設定為 [二進位]。  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>其他參考  
- [命令列語法關鍵](command-line-syntax-key.md)  
