---
title: irftp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848419"
---
# <a name="irftp"></a>irftp

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

透過紅外線連結，會傳送檔案。    
## <a name="syntax"></a>語法  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|磁碟機：\|指定包含您想要透過紅外線連結傳送之檔案的磁碟機。|  
|[path]FileName|指定的位置和檔案名稱或一組您想要透過紅外線連結傳送的檔案。 如果您指定一組檔案，您必須指定每個檔案的完整路徑。|  
|/h|指定隱藏的模式。 使用隱藏的模式時，就會將檔案傳送而不會顯示 [無線連結] 對話方塊。|  
|/s|讓您可以選取檔案或一組您想要傳送，而不使用命令列來指定磁碟機、 路徑和檔案名稱的檔案，請開啟無線連結 對話方塊中。|  

## <a name="remarks"></a>備註  
-   之前使用此命令，確認您想要透過紅外線連結進行通訊的裝置是否已啟用紅外線功能並正常運作，請建立紅外線連結時，會在裝置之間。  
-   不含參數或搭配 **/s**， **irftp**開啟**無線連結**對話方塊中，您可以在其中選取您想要傳送，而不使用命令列的檔案。  

## <a name="BKMK_Examples"></a>範例  
將 Example.txt 傳送透過紅外線連結。  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
