---
title: ftp ascii
description: Ftp ascii 的 Windows 命令主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4930d0d726acaa222f802d7aaef59578030db5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843781"
---
# <a name="ftp-ascii"></a>ftp： ascii

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 ASCII。   
## <a name="syntax"></a>語法  
```  
ascii  
```  
#### <a name="parameters"></a>參數  
無  
## <a name="remarks"></a>備註  
- 預設的檔案傳輸類型為 ASCII。  
- 在 ASCII 模式中，會執行與網路標準字元集之間的字元轉換。 例如，根據目標作業系統，會視需要轉換行尾字元。  
- **ftp**同時支援 ASCII 和二進位圖像檔案傳輸類型。 傳送文字檔時，請使用 ASCII。 如需二進位檔案傳輸的詳細資訊，請參閱其他參考中的**ftp： binary** 。  
  ## <a name="examples"></a><a name=BKMK_Examples></a>典型  
  將檔案傳輸類型設定為 ASCII。  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>其他參考資料  
- [ftp： binary](ftp-binary.md)  
- - [命令列語法關鍵](command-line-syntax-key.md)  
