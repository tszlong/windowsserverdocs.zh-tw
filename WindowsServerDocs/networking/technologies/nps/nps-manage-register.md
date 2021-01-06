---
title: 在 Active Directory 網域中登錄 NPS
description: 您可以使用本主題，在 NPS 預設網域或其他網域中的 Windows Server 2016 中，註冊執行網路原則伺服器的伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 15cdd18b6b6e5baffc47e1f4ecac7daaf580602a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947084"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>在 Active Directory 網域中登錄 NPS

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，在 NPS 預設網域或其他網域中的 Windows Server 2016 中，註冊執行網路原則伺服器的伺服器。

## <a name="register-an-nps-in-its-default-domain"></a>在其預設網域中註冊 NPS

您可以使用此程式在伺服器是網域成員的網域中註冊 NPS。

NPSs 必須在 Active Directory 中註冊，才能在授權程式期間有權讀取使用者帳戶的撥入屬性。 註冊 NPS 會將伺服器新增至 Active Directory 中的 [ **RAS 和 資訊存取伺服器** ] 群組。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-its-default-domain"></a>在其預設網域中註冊 NPS


1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 [網路原則伺服器] 主控台隨即開啟。

2. 以滑鼠右鍵按一下 [ **NPS (本機)**]，然後按一下 [ **Active Directory 中的 [註冊伺服器**]。 此時會開啟 [網路原則伺服器] 對話方塊。

3. 在 [網路原則伺服器] 中，按一下 [確定]，再按一次 [確定]。

## <a name="register-an-nps-in-another-domain"></a>在另一個網域中註冊 NPS

若要提供 NPS 具有在 Active Directory 中讀取使用者帳戶撥入內容的許可權，NPS 必須在帳戶所在的網域中註冊。

您可以使用此程式在 NPS 不是網域成員的網域中註冊 NPS。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

### <a name="to-register-an-nps-in-another-domain"></a>註冊另一個網域中的 NPS

1. 在網域控制站上的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **Active Directory 消費者和電腦**]。 Active Directory 消費者和電腦主控台隨即開啟。

2. 在主控台樹中，流覽至您要 NPS 讀取使用者帳戶資訊的網域，然後按一下 [ **使用者** ] 資料夾。

3. 在詳細資料窗格中，以滑鼠 **按右鍵 [** **RAS 和 資訊存取伺服器**]，然後按一下 [內容]。 [ **RAS 和 資訊存取伺服器屬性** ] 對話方塊隨即開啟。

4. 在 [ **RAS 和 資訊存取伺服器** 內容] 對話方塊中，按一下 [ **成員** ] 索引標籤，新增您想要在網域中註冊的每個 NPSs，然後按一下 **[確定]**。


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>使用 NPS 的 Netsh 命令在另一個網域中註冊 NPS

1. 開啟命令提示字元或 windows PowerShell。

2. 在命令提示字元中輸入下列命令： **netsh nps add registeredserver** &nbsp; *domain* &nbsp; *server*，然後按 enter。

>[!NOTE]
>在上述命令中， *domain* 是您要在其中註冊 nps 之網域的 DNS 功能變數名稱，而 *SERVER* 則是 nps 電腦的名稱。

