---
title: ftp 二進位檔
description: Ftp 二進位檔的 Windows 命令主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b2f72517826576cfee643eda0c54063b162c94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843721"
---
# <a name="ftp-binary"></a>ftp： binary

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將檔案傳輸類型設定為 binary。   
## <a name="syntax"></a>語法  
```  
binary  
```  
#### <a name="parameters"></a>參數  
無  
## <a name="remarks-optional-section"></a>備註 <optional section>  
**ftp**同時支援 ASCII 和二進位圖像檔案傳輸類型。 傳輸可執行檔時，請使用 binary。 在二進位模式中，檔案會以一個位元組的單位傳輸。 如需 ASCII 檔案傳輸的詳細資訊，請參閱其他參考中的**ftp： ASCII** 。  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
將 [檔案傳輸類型] 設定為 [二進位]。  
```  
binary  
```  
## <a name="additional-references"></a>其他參考資料  
-   [ftp： ascii](ftp-ascii.md)  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
