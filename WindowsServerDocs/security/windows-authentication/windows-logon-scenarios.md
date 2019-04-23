---
title: Windows 登入案例
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828179"
---
# <a name="windows-logon-scenarios"></a>Windows 登入案例

>適用於：Windows Server （半年通道），Windows Server 2016

常見的 Windows 登入和登入案例，摘要說明這個參考主題適合 IT 專業人員。

Windows 作業系統需要所有使用者登入使用有效的帳戶來存取本機電腦和網路資源。 以 Windows 為基礎的電腦會保護資源，藉由實作登入程序，使用者會進行驗證。 授權和存取控制技術驗證使用者之後，實作保護資源的第二個階段： 判斷已驗證的使用者是否被授權存取的資源。

本主題的內容套用至指定的 Windows 版本**適用於**本主題開頭的清單。

此外，應用程式和服務可以要求使用者登入以存取這些應用程式或服務所提供的資源。 有效的帳戶和正確的認證是必要的但登入資訊儲存在本機電腦上的安全性帳戶管理員 (SAM) 資料庫和 Active Directory 中，適用，登入程序是類似於登入程序。 登入帳戶和認證資訊是由管理應用程式或服務，並 （選擇性） 可以儲存在本機中認證保險箱。

若要了解如何進行驗證，請參閱[Windows 驗證概念](windows-authentication-concepts.md)。

本主題描述下列案例：

-   [互動式登入](#BKMK_InteractiveLogon)

-   [網路登入](#BKMK_NetworkLogon)

-   [智慧卡登入](#BKMK_SmartCardLogon)

-   [生物識別技術登入](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>互動式登入
當使用者輸入認證，在 [認證項目] 對話方塊中，或當使用者插入智慧卡插入智慧卡讀取裝置，或使用生物識別裝置的使用者互動時，就會開始登入程序。 使用者可以使用本機使用者帳戶或網域帳戶來登入電腦，以執行互動式登入。

下圖顯示登入程序與互動式登入項目。

![此圖顯示登入程序與互動式登入項目](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows 用戶端驗證架構**

### <a name="BKMK_LocaDomainLogon"></a>本機和網域登入
網域登入的使用者出示認證，包含所有本機登入，例如帳戶名稱和密碼或憑證，以及 Active Directory 網域資訊的必要項目。 處理程序可確認使用者的本機電腦上的安全性資料庫或 Active Directory 網域的使用者的識別。 此必要的登入程序無法關閉網域中的使用者。

使用者可以在任一種方式中執行互動式登入電腦：

-   在本機，當使用者具有直接實際存取電腦，或在電腦的電腦網路的一部分時。

    本機登入授與使用者存取本機電腦上的 Windows 資源的權限。 本機登入需要使用者具有使用者帳戶中的安全性帳戶管理員 (SAM) 在本機電腦上。 SAM 保護並管理使用者和群組的形式儲存在本機電腦登錄中的安全性帳戶的資訊。 電腦可以存取網路，但並非必要。 本機使用者帳戶和群組成員資格的資訊用來管理本機資源的存取權。

    網路登入授與使用者存取網路的電腦上的任何資源除了在本機電腦上的 Windows 資源認證的存取權杖所定義的權限。 本機登入和登入網路需要使用者具有使用者帳戶中的安全性帳戶管理員 (SAM) 在本機電腦上。 本機使用者帳戶和群組成員資格資訊用來管理本機資源的存取權和使用者的存取權杖可讓您定義可以存取哪些資源，在網路上的電腦上。

    本機登入和登入網路並不足夠使用者和電腦權限授與存取，並將網域資源。

-   遠端電腦上，透過終端機服務或遠端桌面服務 (RDS)，則這個登入是進一步限定為遠端互動。

互動式登入之後，Windows 執行應用程式代表使用者，以及使用者可以與這些應用程式互動。

本機登入授與使用者的權限來存取本機電腦上的資源或網路的電腦上的資源。 如果電腦已加入網域，Winlogon 功能嘗試登入該網域。

使用者的權限來存取本機和網域資源，會授與登入網域。 網域登入要求的使用者具有在 Active Directory 中的使用者帳戶。 電腦必須擁有 Active Directory 網域中的帳戶，並實際連接到網路。 使用者也必須登入本機電腦或網域的使用者權限。 網域使用者帳戶資訊和群組成員資格資訊來管理網域和本機資源存取權。

### <a name="BKMK_RemoteLogon"></a>遠端登入
在 Windows 中，透過遠端登入存取另一部電腦需要在遠端桌面通訊協定 (RDP)。 使用者必須已擁有成功登入到用戶端電腦再嘗試遠端連線，因為互動式登入程序已順利完成。

RDP 會管理使用遠端桌面用戶端的使用者輸入的認證。 這些認證是針對在目標電腦，使用者必須具有該目標電腦上的帳戶。 此外，目標電腦必須設定為接受遠端連線。 目標電腦認證會傳送至嘗試執行驗證程序。 如果驗證成功，使用者會連線到本機和網路資源，都可供使用提供的認證。

## <a name="BKMK_NetworkLogon"></a>網路登入
網路登入僅適用於使用者、 服務或電腦的驗證後隨同。 網路登入時，此程序不使用認證項目對話方塊來收集資料。 相反地，先前建立的認證或使用另一種方法來收集認證。 此程序可確認使用者身分識別與使用者嘗試存取任何網路服務。 此程序是使用者通常不會看到它，除非有提供替代認證。

若要提供這種類型的驗證，安全性系統會包含這些驗證機制：

-   Kerberos 版本 5 通訊協定

-   公開金鑰憑證

-   安全通訊端層/傳輸層安全性 (SSL/TLS)

-   摘要

-   NTLM，與 Microsoft Windows NT 4.0 為基礎的系統相容性

如需詳細資訊的項目和處理程序，互動式登入圖。

## <a name="BKMK_SmartCardLogon"></a>智慧卡登入
智慧卡可以用於網域帳戶，而非本機帳戶只能登入。 智慧卡驗證需要使用 Kerberos 驗證通訊協定。 中導入在 Windows 2000 Server 中，以 Windows 為基礎作業系統的公用金鑰的擴充功能來實作通訊協定的初始驗證要求的 Kerberos。 相較於共用祕密的金鑰密碼編譯公開金鑰密碼編譯為非對稱，也就是需要兩個不同的索引鍵： 一個用來加密，來解密另一個。 在一起，才能執行這兩項作業的索引鍵會構成私密/公開金鑰組。

若要初始化一般的登入工作階段，使用者必須提供資訊只有知道使用者和基礎的 Kerberos 通訊協定基礎結構證明他或她的身分識別。 祕密的資訊是衍生自使用者密碼的密碼編譯的共用的金鑰。 共用祕密金鑰是對稱的也就是說，相同的索引鍵，用於加密和解密。

下圖顯示的項目和所需的智慧卡登入的程序。

![此圖顯示的項目和處理程序所需的智慧卡登入](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智慧卡認證提供者架構**

使用智慧卡時，而不使用密碼，私密/公開金鑰組，儲存在使用者的智慧卡被取代共用祕密金鑰，衍生自使用者的密碼。 私用金鑰只會儲存在智慧卡。 公開金鑰可與其擁有者想要交換的機密資訊的任何人。

如需有關在 Windows 中的智慧卡登入程序的詳細資訊，請參閱[智慧卡登入的運作方式在 Windows 中](https://technet.microsoft.com/library/ff404285.aspx)。

## <a name="BKMK_BioLogon"></a>生物識別技術登入
裝置用來擷取並建立成品，例如指紋數位特性。 範例相同的成品，然後比較此數位的表示法，並當成功地比較這兩個時，可以通過驗證。 執行任何指定的作業系統的電腦**適用於**本主題開頭的清單可以設定為接受這種形式的登入。 不過，如果生物識別技術登入僅設定為本機登入，使用者必須提供網域認證，存取 Active Directory 網域時。

## <a name="additional-resources"></a>其他資源
如需有關 Windows 如何管理認證登入程序期間所提交的資訊，請參閱 <<c0> [ 在 Windows 驗證的認證管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登入及驗證技術概觀](https://technet.microsoft.com/library/dn169029.aspx)


