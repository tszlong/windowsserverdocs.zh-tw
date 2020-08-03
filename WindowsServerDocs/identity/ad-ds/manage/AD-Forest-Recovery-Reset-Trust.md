---
title: AD 樹系復原-重設信任密碼
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 66b0e772a1d656551811f4f9ded0a8d80627e485
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519005"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>在信任的一端重設信任密碼

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 如果樹系復原與安全性缺口有關，請使用下列程式在信任的一端重設信任密碼。 這包括子域和父系網域之間的隱含信任，以及此網域（信任網域）和另一個網域（受信任的網域）之間的明確信任。

 僅重設信任網域端的密碼，也稱為連入信任（此網域所屬的端）。 然後，在信任的受信任網域端使用相同的密碼，也稱為連出信任。 當您還原每個其他（受信任的）網域中的第一個 DC 時，重設外寄信任的密碼。

 重設信任密碼可確保 DC 不會在其網域外部複寫可能不良的 Dc。 在還原每個網域中的第一個 DC 時，您可以設定相同的信任密碼，確保此 DC 會與每個復原的 Dc 進行複寫。 透過安裝 AD DS 復原的網域中的後續 Dc，會在安裝過程中自動複寫這些新密碼。

## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>在信任的一端重設信任密碼

1. 在命令提示字元，輸入下列命令，然後按 ENTER：

   ```
   netdom experthelp trust
   ```

2. 使用此命令為使用 NetDom 工具重設信任密碼所提供的語法。
   例如，如果樹系中有兩個網域（父系和子系），而您在父系網域中還原的 DC 上執行這個命令，請使用下列命令語法：

   ```
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*
   ```

   當您在子域中執行此命令時，請使用下列命令語法：

   ```
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*
   ```

   > [!NOTE]
   > 在信任的兩端， **passwordT**應該是相同的值。 只執行此命令一次（不同于**netdom resetpwd**命令），因為它會自動重設密碼兩次。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
