---
title: AD 樹系復原-重設電腦帳戶在網域控制站
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852759"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>AD 樹系復原-重設電腦帳戶在網域控制站

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 您可以使用下列程序來重設 DC 的電腦帳戶密碼。 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>若要重設的網域控制站的電腦帳戶密碼  

1. 在命令提示字元中輸入下列命令，然後按 ENTER：  

   ```
   netdom help resetpwd  
   ```
  
2. 使用這個命令會提供重設電腦帳戶密碼，例如使用 Netdom 命令列工具的語法：  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    何處*網域控制站名稱*是您要復原的本機 DC。 
  
   > [!NOTE]
   > 您應該執行此命令兩次。
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
