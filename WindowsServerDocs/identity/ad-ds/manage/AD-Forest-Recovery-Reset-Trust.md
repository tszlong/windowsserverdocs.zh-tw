---
title: "廣告樹系修復-備份完整伺服器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>將其中一邊的信任信任密碼重設  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 如果樹系復原相關的安全性漏洞，使用下列程序將其中一邊的信任信任密碼重設。 這包括隱含信任之間子女和家長網域，以及明確信任之間這個網域 （信任的網域） 和其他網域 （受信任的網域）。  
  
 重設密碼一端只信任網域信任，也就是連入信任 （一邊所屬這個網域）。 然後，一端受信任的網域信任，也就是傳出信任使用相同的密碼。 還原在每個 （信任） 網域中的第一個 DC 時，請重設密碼的撥出信任。  
  
 信任密碼重設，可確保 DC 不會複寫潛在錯誤 dc 外其網域。 還原在每個網域中的第一個 DC 時設定信任相同的密碼，您確保，與每個復原網域控制站複製這個網域控制站。 後續 Dc 網域中的復原安裝 AD DS，安裝程序期間，會自動複寫這些新的密碼。  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>將其中一邊的信任信任密碼重設  
  
1.  在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  使用語法，此命令提供使用 NetDom 工具信任密碼重設。  
  
     例如，如果有兩個網域森林中-父系和子女-並還原家長網域中的網域控制站在您執行這個命令，使用下列語法命令：  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     當您的子女網域中執行這個命令時，使用下列命令語法：  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT**應該會在兩個側邊信任的相同的值。 一次執行這個命令 (和不同的是**netdom resetpwd**命令) 因為它會自動重設密碼兩次。  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
