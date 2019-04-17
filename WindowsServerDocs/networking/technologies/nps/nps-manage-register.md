---
title: 在 Active Directory Domain 登記 NPS 伺服器
description: 您可以使用此主題為隊伍登記執行 Windows Server 2016 NPS 伺服器預設網域或其他網域中的網路原則伺服器的伺服器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>在 Active Directory Domain 登記 NPS 伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題為隊伍登記執行 Windows Server 2016 NPS 伺服器預設網域或其他網域中的網路原則伺服器的伺服器。

## <a name="register-an-nps-server-in-its-default-domain"></a>NPS 伺服器登記其預設網域中

您可以使用此程序位於網域中伺服器網域成員登記 NPS 伺服器。 

NPS 伺服器必須登記完畢 Active Directory 中，讓他們使用的是讀取的帳號撥號屬性授權程序期間的權限。 登記 NPS 伺服器新增伺服器**RAS 及 IAS 伺服器]**群組中 Active Directory。

資格在**系統管理員**，或相當於，才能執行這些程序最小值。

### <a name="to-register-an-nps-server-in-its-default-domain"></a>若要在其預設網域登記 NPS 伺服器


1. NPS 伺服器，在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**。 開啟主機的網路原則伺服器。

2. 以滑鼠右鍵按一下**NPS（本機）**，然後按一下 [**登記伺服器 Active Directory**。 **的網路原則伺服器**對話方塊。

3. 在**的網路原則伺服器**，按一下 [ **[確定]**，，然後按一下 [ **[確定]**再試一次。

## <a name="register-an-nps-server-in-another-domain"></a>在另一部網域登記 NPS 伺服器

若要讀取的 Active Directory 中帳號撥號屬性的權限提供 NPS 伺服器、NPS 伺服器必須所在帳號網域中登記完畢。

您可以使用此程序登記 NPS 伺服器 NPS 伺服器位於未加入網域的網域。

資格在**系統管理員**，或相當於，才能執行這些程序最小值。

### <a name="to-register-an-nps-server-in-another-domain"></a>若要在另一部網域登記 NPS 伺服器

1. 網域控制站在伺服器管理員中，按一下 [**工具**，然後按**Active Directory 使用者與電腦**。 Active Directory 使用者和電腦主機開啟。

2. 主控台中瀏覽至您想要讀取使用者 account 資訊、NPS 伺服器網域，然後按一下**使用者**資料夾。 

3. 在詳細資料窗格中，以滑鼠右鍵按一下**RAS 及 IAS 伺服器**，然後按**屬性**。 **RAS 及 IAS 伺服器屬性**對話方塊。

4. 在**RAS 及 IAS 伺服器屬性**對話方塊中，按**成員**索引標籤，新增的每個 NPS 伺服器您想要登記網域中，然後按一下 [ **[確定]**。


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>使用 Netsh 命令 NPS 登記 NPS 伺服器並另一個網域中

1. 命令提示字元」或「windows PowerShell 開放。 

2. 在命令提示字元中輸入下列命令：**netsh nps 新增 registeredserver**&nbsp;*網域*&nbsp;*伺服器*，然後按 ENTER 鍵。

>[!NOTE]
>在前一個命令中，*網域*是您想要登記 NPS 伺服器，網域 DNS 網域名稱和*伺服器*NPS 伺服器電腦的名稱。

