---
title: irftp
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34f912878b97bd00fb1c4ea539c7ea4c1423ea59
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724810"
---
# <a name="irftp"></a>irftp

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過紅外線連結傳送檔案。    
## <a name="syntax"></a>語法  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|磁片磁碟機\|：指定包含您要透過紅外線連結傳送之檔案的磁片磁碟機。|  
|路徑名名稱|指定您想要透過紅外線連結傳送的檔案或一組檔案的位置和名稱。 如果您指定一組檔案，就必須指定每個檔案的完整路徑。|  
|/h|指定隱形模式。 使用隱形模式時，會傳送檔案，而不會顯示 [無線連結] 對話方塊。|  
|/s|開啟 [無線連結] 對話方塊，讓您可以選取要傳送的檔案或一組檔案，而不需使用命令列來指定磁片磁碟機、路徑和檔案名。|  

## <a name="remarks"></a>備註  
-   使用此命令之前，請確認您想要透過紅外線連結進行通訊的裝置已啟用紅外線功能且正常運作，而且裝置之間已建立紅外線連結。  
-   使用時不含參數或搭配 **/s**使用， **irftp**會開啟 [**無線連結**] 對話方塊，您可以在其中選取要傳送的檔案，而不需要使用命令列。  

## <a name="examples"></a>範例  
透過紅外線連結傳送範例 .txt。  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>其他參考  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
