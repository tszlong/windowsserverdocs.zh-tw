---
description: 深入瞭解： AD 樹系復原-重設 DC 上的電腦帳戶
title: AD 樹系復原-重設 DC 上的電腦帳戶
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.openlocfilehash: 21d36989ff6174e5b59eed0b6b63e8aea0a9d921
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041566"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>AD 樹系復原-重設 DC 上的電腦帳戶

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用下列程式來重設 DC 的電腦帳戶密碼。

## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>重設網域控制站的電腦帳戶密碼

1. 在命令提示字元，輸入下列命令，然後按 ENTER：

   ```
   netdom help resetpwd
   ```

2. 使用這個命令提供的語法，以使用 Netdom 命令列工具來重設電腦帳戶密碼，例如：

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*
   ```

    其中的 *網域控制站名稱* 是您要復原的本機 DC。

   > [!NOTE]
   > 您應該執行此命令兩次。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
