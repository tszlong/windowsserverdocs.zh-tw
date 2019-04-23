---
title: AD 樹系復原-備份完整伺服器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865559"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>重設一方的信任的信任密碼  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 如果出現安全性缺口相關樹系復原，使用下列程序來重設一方的信任的信任密碼。 這包括子系和父系網域，以及此網域 （信任的網域） 和另一個網域 （受信任的網域） 之間的明確信任之間的隱含信任。 
  
 重設密碼，一邊只信任網域的信任，也就是連入信任 （此網域所屬的邊）。 然後，信任，也就是連出信任的受信任的網域端上使用相同的密碼。 當您還原在每一個其他 （信任） 的網域中的第一個 DC 時，重設的連出信任密碼。 
  
 重設信任密碼，可確保 DC 不會使用其網域之外可能不正確的網域控制站複寫。 藉由設定相同的信任密碼還原在每個網域中的第一個 DC 時，您可以確定此 DC 複寫的每個復原網域控制站。 後續網域控制站在網域中安裝 AD DS 來復原的安裝程序期間，會自動複寫這些新的密碼。 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>若要重設一方的信任的信任密碼  
  
1. 在命令提示字元中輸入下列命令，然後按 ENTER：  

   ```  
   netdom experthelp trust  
   ```  
  
2. 使用這個命令會提供的信任密碼重設使用 NetDom 工具的語法。
   比方說，如果樹系中有兩個網域，父系和子系，和您在父系網域中還原的網域控制站上執行此命令，請使用下列命令語法：  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   當您在子網域中執行此命令時，使用下列命令語法：  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT**應該是相同的值在雙方的信任。 此命令僅執行一次 (不同於**netdom resetpwd**命令) 因為它會自動重設密碼兩次。 
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
