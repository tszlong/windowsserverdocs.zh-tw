---
title: ftp ascii
description: 'Windows 命令主題適用於 ftp ascii '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7bcca3f29cec8ff5c30256dfd123acc7fbb804d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849189"
---
# <a name="ftp-ascii"></a>ftp: ascii

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將檔案傳輸類型設定為 ASCII。   
## <a name="syntax"></a>語法  
```  
ascii  
```  
### <a name="parameters"></a>參數  
無  
## <a name="remarks"></a>備註  
-   預設的檔案傳輸類型為 ASCII。  
-   在 ASCII 模式中，會執行的網路標準字元集的字元轉換。 比方說，行尾字元會轉換必要時，根據目標作業系統。  
-   **ftp**支援 ASCII 和二進位檔映像檔案傳輸類型。 傳送文字檔案時，請使用 ASCII。 如需有關傳送二進位檔案的詳細資訊，請參閱**ftp： 二進位**中其他參考資料。  
## <a name="BKMK_Examples"></a>範例  
您可以設定檔案傳輸類型為 ASCII。  
```  
ascii  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp: binary](ftp-binary.md)  
-   [命令列語法關鍵](command-line-syntax-key.md)  
