---
title: reset session
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 67a4e910ba87209c9700f2242f7859a6cc9e725f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384534"
---
# <a name="reset-session"></a>reset session

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您重設（刪除）遠端桌面工作階段主機（rd 工作階段主機）伺服器上的會話。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。  

## <a name="syntax"></a>語法  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>參數  

|參數|描述|  
|-------|--------|  
|\<SessionName >|指定您想要重設的會話名稱。 若要判斷會話的名稱，請使用**查詢會話**命令。|  
|\<SessionID >|指定要重設之會話的識別碼。|  
|/server： \<ServerName >|指定包含您要重設之會話的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|  
|/v|顯示正在執行之動作的相關資訊。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   您一律可以重設自己的會話，但您必須擁有 [完全控制] 存取權限，才能重設其他使用者的會話。  
-   請注意，在不發出警告的情況下重設使用者的會話可能會導致會話遺失資料。  
-   只有當會話發生故障或似乎已停止回應時，您才應該重設會話。  
-   只有當您從遠端伺服器使用 [**重設會話**] 時，才需要 **/server**參數。  

## <a name="BKMK_examples"></a>典型  
- 若要重設指定的 rdp-tcp # 6 會話，請輸入：  
  ```  
  reset session rdp-tcp#6  
  ```  
- 若要重設使用會話識別碼3的會話，請輸入：  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
[遠端桌面服務&#40;終端機&#41;服務命令參考](remote-desktop-services-terminal-services-command-reference.md)  
