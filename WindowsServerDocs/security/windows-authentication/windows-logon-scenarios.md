---
title: Windows 登入案例
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 07bfb538e1b43fc0c734b3c59b906c027ef985c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403297"
---
# <a name="windows-logon-scenarios"></a>Windows 登入案例

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用于 IT 專業人員的參考主題會摘要說明常見的 Windows 登入和登入案例。

Windows 作業系統要求所有使用者都必須使用有效的帳戶登入電腦，才能存取本機和網路資源。 以 Windows 為基礎的電腦會藉由執行登入程式（在其中驗證使用者）來保護資源。 驗證使用者之後，授權和存取控制技術會實行保護資源的第二個階段：判斷已驗證的使用者是否已獲授權可存取資源。

本主題的內容適用于本主題開頭的**適用**物件清單中指定的 Windows 版本。

此外，應用程式和服務可能會要求使用者登入，以存取應用程式或服務所提供的資源。 登入程式類似于登入程式，因為需要有效的帳戶和正確的認證，但登入資訊會儲存在本機電腦上的安全性帳戶管理員（SAM）資料庫中，以及適用的 Active Directory 中。 登入帳戶和認證資訊是由應用程式或服務所管理，並可選擇性地儲存在本機認證保險箱中。

若要瞭解驗證的運作方式，請參閱[Windows 驗證概念](windows-authentication-concepts.md)。

本主題說明下列案例：

-   [互動式登入](#BKMK_InteractiveLogon)

-   [網路登入](#BKMK_NetworkLogon)

-   [智慧卡登入](#BKMK_SmartCardLogon)

-   [生物識別登入](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>互動式登入
登入程式會在使用者于 [認證專案] 對話方塊中輸入認證時，或當使用者將智慧卡插入智慧卡讀卡機，或使用者與生物識別單元互動時開始。 使用者可以使用本機使用者帳戶或網域帳戶來登入電腦，以執行互動式登入。

下圖顯示互動式登入元素和登入程式。

![顯示互動式登入元素和登入程式的圖表](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows 用戶端驗證架構**

### <a name="BKMK_LocaDomainLogon"></a>本機和網域登入
使用者為網域登入所提供的認證包含本機登入所需的所有元素，例如帳戶名稱和密碼或憑證，以及 Active Directory 網域資訊。 此程式會確認使用者的本機電腦或 Active Directory 網域的安全性資料庫識別。 網域中的使用者無法關閉此強制登入程式。

使用者可以使用下列兩種方式之一，在電腦上執行互動式登入：

-   當使用者具有電腦的直接實體存取權，或電腦是電腦網路的一部分時，在本機上。

    本機登入會授與使用者存取本機電腦上 Windows 資源的許可權。 本機登入需要使用者在本機電腦的安全性帳戶管理員（SAM）中具有使用者帳戶。 SAM 會以儲存在本機電腦登錄中的安全性帳戶形式，來保護和管理使用者和群組資訊。 電腦可以擁有網路存取權，但不是必要的。 本機使用者帳戶和群組成員資格資訊可用來管理本機資源的存取權。

    除了認證的存取權杖所定義的網路電腦上的任何資源以外，網路登入也會授與使用者存取本機電腦上 Windows 資源的許可權。 本機登入和網路登入時，都需要使用者在本機電腦的安全性帳戶管理員（SAM）中具有使用者帳戶。 本機使用者帳戶和群組成員資格資訊可用來管理本機資源的存取權，而使用者的存取權杖會定義可在網路電腦上存取哪些資源。

    本機登入和網路登入不足以授與使用者和電腦存取和使用網域資源的許可權。

-   透過終端機服務或遠端桌面服務（RDS）遠端執行，在此情況下，登入會進一步限定為遠端互動式。

在互動式登入之後，Windows 會代表使用者執行應用程式，而使用者可以與這些應用程式互動。

本機登入會授與使用者許可權，以存取本機電腦上的資源或網路電腦上的資源。 如果電腦已加入網域，則 Winlogon 功能會嘗試登入該網域。

網域登入會授與使用者存取本機和網域資源的許可權。 網域登入需要使用者在 Active Directory 中具有使用者帳戶。 電腦必須具有 Active Directory 網域中的帳戶，且實際連線到網路。 使用者也必須具有登入本機電腦或網域的使用者權限。 網域使用者帳戶資訊和群組成員資格資訊可用來管理網域和本機資源的存取權。

### <a name="BKMK_RemoteLogon"></a>遠端登入
在 Windows 中，透過遠端登入存取另一部電腦需依賴遠端桌面通訊協定（RDP）。 因為使用者在嘗試遠端連線之前必須已經成功登入用戶端電腦，所以互動式登入程式已順利完成。

RDP 會管理使用者使用遠端桌面用戶端輸入的認證。 這些認證適用于目的電腦，而且使用者必須擁有該目的電腦上的帳戶。 此外，目的電腦必須設定為接受遠端連線。 系統會傳送目的電腦認證以嘗試執行驗證程式。 如果驗證成功，則使用者會連接到可使用提供的認證來存取的本機和網路資源。

## <a name="BKMK_NetworkLogon"></a>網路登入
只有在進行使用者、服務或電腦驗證之後，才可以使用網路登入。 在網路登入時，進程不會使用 [認證專案] 對話方塊來收集資料。 相反地，會使用先前建立的認證或用來收集認證的另一個方法。 此程式會向使用者嘗試存取的任何網路服務確認使用者的身分識別。 除非必須提供替代認證，否則使用者通常不會看到此程式。

為了提供這種類型的驗證，安全性系統包含下列驗證機制：

-   Kerberos 第5版通訊協定

-   公開金鑰憑證

-   安全通訊端層/傳輸層安全性（SSL/TLS）

-   摘要

-   NTLM，用於與 Microsoft Windows NT 4.0 型系統的相容性

如需元素和程式的詳細資訊，請參閱上述的互動式登入圖表。

## <a name="BKMK_SmartCardLogon"></a>智慧卡登入
智慧卡只能用來登入網域帳戶，而不是本機帳戶。 智慧卡驗證需要使用 Kerberos 驗證通訊協定。 在 Windows 2000 Server 中引進的 windows 作業系統中，會執行 Kerberos 通訊協定的初始驗證要求的公開金鑰延伸。 相較于共用秘密金鑰加密，公開金鑰加密是非對稱的，也就是說，需要兩個不同的金鑰：一個用來加密，另一個則是解密。 同時，執行這兩項作業所需的金鑰會構成私用/公開金鑰組。

若要起始一般登入會話，使用者必須藉由提供只有使用者和基礎 Kerberos 通訊協定基礎結構所知的資訊，來證明他的身分識別。 秘密資訊是從使用者的密碼衍生而來的加密共用金鑰。 共用秘密金鑰是對稱的，這表示會同時使用相同的金鑰進行加密和解密。

下圖顯示智慧卡登入所需的元素和處理常式。

![此圖顯示智慧卡登入所需的元素和進程](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智慧卡認證提供者架構**

使用智慧卡而非密碼時，儲存在使用者智慧卡上的私用/公開金鑰組，會取代為衍生自使用者密碼的共用秘密金鑰。 私密金鑰只會儲存在智慧卡上。 公開金鑰可供擁有者想要交換機密資訊的任何人使用。

如需 Windows 中智慧卡登入程式的詳細資訊，請參閱[windows 中的智慧卡登入運作](https://technet.microsoft.com/library/ff404285.aspx)方式。

## <a name="BKMK_BioLogon"></a>生物識別登入
裝置可用來捕捉和建立成品的數位特性，例如指紋。 這個數位標記法接著會與相同成品的範例進行比較，而當兩者成功比較時，就會發生驗證。 執行本主題開頭的 [**適用**于] 清單中指定的任何作業系統的電腦，都可以設定為接受這種形式的登入。 不過，如果僅針對本機登入設定生物識別登入，則使用者在存取 Active Directory 網域時，必須提供網域認證。

## <a name="additional-resources"></a>其他資源
如需 Windows 如何管理在登入程式期間所提交之認證的相關資訊，請參閱[Windows 驗證中的認證管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登入和驗證技術總覽](https://technet.microsoft.com/library/dn169029.aspx)


