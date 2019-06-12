---
title: tscon
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8cac59b2f5524df5a82e9c83424fd781f0ef7c8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440933"
---
# <a name="tscon"></a>tscon

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

連接到遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的另一個工作階段。  
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。  

## <a name="syntax"></a>語法  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>參數  

|參數|描述|  
|-------|--------|  
|\<SessionID>|指定您要連線的工作階段識別碼。 如果您使用選擇性 **/dest:** <*SessionName*> 參數，這是您要連線的工作階段的識別碼。|  
|\<SessionName>|指定您要連線的工作階段的名稱。|  
|/dest:\<SessionName>|指定目前的工作階段的名稱。 當您連接至新的工作階段時，將中斷連接此工作階段。|  
|/password:\<密碼 >|指定擁有您要連線的工作階段之使用者的密碼。 連線的使用者並未擁有工作階段時，才需要此密碼。|  
|/password:*|擁有您要連線的工作階段的使用者密碼的提示。|  
|/v|顯示已執行之動作的相關資訊。|  
|/?|在命令提示字元顯示說明。|  

## <a name="remarks"></a>備註  
-   您必須擁有 完全控制存取權限，或連接來連接到另一個工作階段的特殊存取權限。  
-   **/Dest:** <*SessionName*> 參數可讓您連線到不同的工作階段的另一位使用者的工作階段。  
-   如果您未指定密碼的 <*密碼*> 參數，而目標工作階段不同於目前使用者所屬**tscon**失敗。  
-   您無法連線到主控台工作階段。  

## <a name="BKMK_examples"></a>範例  
- 若要連線到目前的 rd 工作階段主機伺服器上的工作階段 12，並中斷目前工作階段的連線，請輸入：  
  ```  
  tscon 12  
  ```  
- 若要使用密碼 mypass 中，連接到目前的 rd 工作階段主機伺服器上的工作階段 23 並中斷目前工作階段的連線，請輸入：  
  ```  
  tscon 23 /password:mypass  
  ```  
- 若要連線 term03 term05 應用程式，在工作階段的工作階段，並再中斷工作階段 TERM05，如果它已連線，請輸入：  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>其他參考資料  
  [命令列語法關鍵](command-line-syntax-key.md)  
  [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)  
