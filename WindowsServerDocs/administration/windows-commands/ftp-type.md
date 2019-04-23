---
title: ftp 類型
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3524c7c772cdcd54a131d8a7e8c8714fad9ce563
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858899"
---
# <a name="ftp-type"></a>ftp： 型別

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定或顯示的檔案傳輸類型。   
## <a name="syntax"></a>語法  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|[<typeName>]|指定檔案傳輸類型。|  
## <a name="remarks"></a>備註  
-   如果*typeName*未指定，會顯示目前的類型。  
-   **ftp**支援兩個檔案傳輸類型、 ASCII 和二進位檔。  
    預設的檔案傳輸類型為 ASCII。  **Ascii**傳送文字檔案時應該使用的命令。 在 ASCII 模式中，會執行的網路標準字元集的字元轉換。 例如，將行尾字元轉換為必要，請根據目的地的作業系統。  
    **二進位**傳輸可執行檔時，應該使用命令。 在二進位模式中，會移動檔案，以位元組為單位。  
## <a name="BKMK_Examples"></a>範例  
您可以設定檔案傳輸類型為 ASCII。  
```  
type ascii  
```  
將傳輸的檔案類型設定為二進位檔。  
```  
type binary  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
