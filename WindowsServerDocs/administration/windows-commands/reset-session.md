---
title: reset session
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5a0991c76ba890bb94b0dcf258df6207ed228e72
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441791"
---
# <a name="reset-session"></a>reset session

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓您重設 （刪除） 的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的工作階段。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。  

## <a name="syntax"></a>語法  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>參數  

|參數|描述|  
|-------|--------|  
|\<SessionName>|指定您想要重設工作階段的名稱。 若要判斷工作階段的名稱，請使用**查詢工作階段**命令。|  
|\<SessionID>|指定要重設工作階段的識別碼。|  
|/server:\<伺服器名稱 >|指定包含您想要重設工作階段的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|  
|/v|顯示已執行之動作的相關資訊。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   您一律可以重設自己的工作階段，但您必須重設其他使用者的工作階段的完整控制存取權限。  
-   請注意，重設使用者工作階段，如果沒有警告使用者可能會導致資料遺失的工作階段。  
-   您應該重設工作階段只當功能有問題或似乎已停止回應。  
-   **/Server**只有當您使用時，才是必要參數**重設工作階段**從遠端伺服器。  

## <a name="BKMK_examples"></a>範例  
- 若要重設指定 rdp tcp #6 的工作階段，請輸入：  
  ```  
  reset session rdp-tcp#6  
  ```  
- 若要重設的工作階段，會使用工作階段 ID 3，請輸入：  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
[遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)  
