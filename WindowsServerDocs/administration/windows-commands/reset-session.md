---
title: reset session
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b83aa67e0d90be9ddc679b32c8ffa4270efec41f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835791"
---
# <a name="reset-session"></a>reset session

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

可讓您重設（刪除）遠端桌面工作階段主機（rd 工作階段主機）伺服器上的會話。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。  

## <a name="syntax"></a>語法  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

### <a name="parameters"></a>參數  

|參數|描述|  
|-------|--------|  
|\<SessionName >|指定您想要重設的會話名稱。 若要判斷會話的名稱，請使用**查詢會話**命令。|  
|\<SessionID >|指定要重設之會話的識別碼。|  
|/server：\<ServerName >|指定包含您要重設之會話的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|  
|/v|顯示正在執行之動作的相關資訊。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   您一律可以重設自己的會話，但您必須擁有 [完全控制] 存取權限，才能重設其他使用者的會話。  
-   請注意，在不發出警告的情況下重設使用者的會話可能會導致會話遺失資料。  
-   只有在工作階段的功能有問題或似乎已停止回應時，才需要重設工作階段。  
-   只有當您從遠端伺服器使用 [**重設會話**] 時，才需要 **/server**參數。  

## <a name="examples"></a><a name=BKMK_examples></a>典型  
- 若要重設指定的 rdp-tcp # 6 會話，請輸入：  
  ```  
  reset session rdp-tcp#6  
  ```  
- 若要重設使用會話識別碼3的會話，請輸入：  
  ```  
  reset session 3  
  ```  

## <a name="additional-references"></a>其他參考資料  
- [命令列語法關鍵](command-line-syntax-key.md)  
[遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)  
