---
title: 在 Active Directory 網域中登錄 NPS
description: 您可以使用本主題，在 NPS 預設網域或另一個網域中的 Windows Server 2016 中註冊執行網路原則伺服器的伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 871e1f2563564e1c85287393cd4b587692a44db6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952145"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>在 Active Directory 網域中登錄 NPS

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 NPS 預設網域或另一個網域中的 Windows Server 2016 中註冊執行網路原則伺服器的伺服器。

## <a name="register-an-nps-in-its-default-domain"></a>在預設網域中註冊 NPS

您可以使用此程式在伺服器為網域成員的網域中註冊 NPS。

Nps 必須在 Active Directory 中註冊，才能讓他們在授權程式期間擁有讀取使用者帳戶撥入內容的許可權。 註冊 NPS 會將伺服器新增至 Active Directory 中的 [ **RAS 和 資訊存取伺服器**] 群組。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-its-default-domain"></a>在預設網域中註冊 NPS


1. 在 NPS 的伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**]。 [網路原則伺服器] 主控台隨即開啟。

2. 以滑鼠右鍵按一下 [ **NPS (本機) **]，然後按一下 [**在 Active Directory 中註冊伺服器**]。 此時會開啟 [網路原則伺服器]**** 對話方塊。

3. 在 [網路原則伺服器]**** 中，按一下 [確定]****，再按一次 [確定]****。

## <a name="register-an-nps-in-another-domain"></a>在另一個網域中註冊 NPS

若要為 NPS 提供在 Active Directory 中讀取使用者帳戶撥入內容的許可權，NPS 必須在帳戶所在的網域中註冊。

您可以使用這個程式，在 NPS 不是網域成員的網域中註冊 NPS。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-another-domain"></a>在另一個網域中註冊 NPS

1. 在網域控制站的伺服器管理員中，按一下 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 [Active Directory 使用者和電腦] 主控台隨即開啟。

2. 在主控台樹中，流覽至您想要 NPS 讀取使用者帳戶資訊的網域，然後按一下 [**使用者**] 資料夾。

3. 在詳細資料窗格中，以滑鼠**按右鍵 [** **RAS 和 資訊存取伺服器**]，然後按一下 [內容]。 [ **RAS 和 資訊存取伺服器**內容] 對話方塊隨即開啟。

4. 在 [ **RAS 和 資訊存取伺服器**內容] 對話方塊中，按一下 [**成員**] 索引標籤，新增您要在網域中註冊的每個 Nps，然後按一下 **[確定]**。


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>若要使用 NPS 的 Netsh 命令在另一個網域中註冊 NPS

1. 開啟 [命令提示字元] 或 [windows PowerShell]。

2. 在命令提示字元中輸入下列內容： **netsh nps add registeredserver** &nbsp; *domain* &nbsp; *server*，然後按 enter。

>[!NOTE]
>在上述命令中， *domain*是您想要在其中註冊 nps 之網域的 DNS 功能變數名稱，而*SERVER*則是 NPS 電腦的名稱。

