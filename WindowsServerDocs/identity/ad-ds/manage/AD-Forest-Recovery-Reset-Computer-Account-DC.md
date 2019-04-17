---
title: "廣告樹系修復-重設電腦 account DC 上"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>廣告樹系修復-重設電腦 account DC 上 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 使用下列程序來重設電腦 account DC 的密碼。  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>重設電腦的網域控制站 account 密碼  
  
1.  在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  使用這個命令提供使用 Netdom 命令列工具來重設電腦 account 密碼，例如語法：  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     其中*網域控制站名稱*是您要修復的本機 DC。  
  
    > [!NOTE]
    >  您應該會在兩次執行這個命令。  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
