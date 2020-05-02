---
title: ftp 二進位檔
description: Ftp 二進位檔的參考主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a3e84bf9256dd5c71dd4444b5939980eecc512
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725375"
---
# <a name="ftp-binary"></a>ftp： binary

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 binary。   
## <a name="syntax"></a>語法  
```  
binary  
```  
#### <a name="parameters"></a>參數  
無  
## <a name="remarks-optional-section"></a>標記<optional section>  
**ftp**同時支援 ASCII 和二進位圖像檔案傳輸類型。 傳輸可執行檔時，請使用 binary。 在二進位模式中，檔案會以一個位元組的單位傳輸。 如需 ASCII 檔案傳輸的詳細資訊，請參閱其他參考中的**ftp： ASCII** 。  
## <a name="examples"></a>範例  
將 [檔案傳輸類型] 設定為 [二進位]。  
```  
binary  
```  
## <a name="additional-references"></a>其他參考  
-   [ftp： ascii](ftp-ascii.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
