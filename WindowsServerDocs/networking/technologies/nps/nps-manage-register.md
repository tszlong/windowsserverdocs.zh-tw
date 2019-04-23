---
title: 在 Active Directory 網域中登錄 NPS
description: 您可以使用本主題中註冊伺服器，NPS 預設網域中，或另一個網域中的 Windows Server 2016 中執行網路原則伺服器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877649"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>在 Active Directory 網域中登錄 NPS

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題中註冊伺服器，NPS 預設網域中，或另一個網域中的 Windows Server 2016 中執行網路原則伺服器。

## <a name="register-an-nps-in-its-default-domain"></a>在 預設網域中登錄 NPS

您可以在其中伺服器是網域成員的網域中登錄 NPS 使用此程序。 

必須在 Active Directory 中註冊 NPSs，使得它們具有授權程序期間讀取使用者帳戶撥入內容的權限。 註冊 NPS 會將伺服器加入**RAS 與 IAS 伺服器**Active Directory 中的群組。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-its-default-domain"></a>在 預設網域中註冊 NPS


1. 在 NPS 中，在 伺服器管理員 中，按一下 **工具**，然後按一下**網路原則伺服器**。 網路原則伺服器主控台隨即開啟。

2. 以滑鼠右鍵按一下**NPS （本機）**，然後按一下**Active Directory 中註冊的伺服器**。 此時會開啟 [網路原則伺服器] 對話方塊。

3. 在 [網路原則伺服器] 中，按一下 [確定]，再按一次 [確定]。

## <a name="register-an-nps-in-another-domain"></a>在另一個網域中登錄 NPS

若要提供 NPS 具有讀取 Active Directory 中的使用者帳戶的撥入屬性的權限，NPS 必須註冊帳戶所在的網域中。

您可以使用此程序在 NPS 為不是網域成員的網域中登錄 NPS。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-another-domain"></a>若要在另一個網域中登錄 NPS

1. 在網域控制站，在 伺服器管理員 中，按一下 **工具**，然後按一下**Active Directory 使用者和電腦**。 [Active Directory 使用者和電腦] 主控台隨即開啟。

2. 在主控台樹狀目錄中，請在瀏覽至您想要讀取使用者帳戶資訊，NPS 網域，，然後按一下**使用者**資料夾。 

3. 在 [詳細資料] 窗格中，以滑鼠右鍵按一下**RAS and IAS Servers**，然後按一下**屬性**。 **RAS 及 IAS 伺服器內容**對話方塊隨即開啟。

4. 在  **RAS 及 IAS 伺服器內容** 對話方塊中，按一下**成員**索引標籤上，新增每個您想要在網域中註冊，然後按一下 NPSs **確定**。


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>若要在另一個的網域中登錄 NPS 使用 nps 的 Netsh 命令

1. 開啟命令提示字元] 或 [windows PowerShell。 

2. 在命令提示字元輸入下列命令： **netsh nps 新增 registeredserver** &nbsp;*網域* &nbsp; *server*，然後按 ENTER 鍵。

>[!NOTE]
>在上述命令中，*網域*是您要註冊 NPS 中的網域的 DNS 網域名稱並*server* NPS 電腦的名稱。

