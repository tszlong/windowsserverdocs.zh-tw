---
title: "Windows 登入案例"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Windows 登入案例

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這適用於 IT 專業人員的參考主題摘要通用 Windows 登入並登入案例。

Windows 作業系統需要所有使用者登入來存取本機有效 account 的電腦及網路資源。 Windows 的電腦安全資源實作驗證使用者登入程序。 授權及存取控制技術驗證使用者後，實作保護資源的第二個階段：判斷驗證的使用者是否獲得授權存取資源。

版本的 Windows 中指定到本主題適用於**適用於**清單中的開頭本主題。

此外，應用程式與服務可以要求使用者登入來存取提供的應用程式或服務的資源。 登入程序很類似登入程序，在於有效的帳號，並正確的認證，但登入資訊儲存在本機電腦上的安全性 Account 管理員（坡）資料庫和 Active Directory 適用。 登入 account 和認證資訊的應用程式或服務由，也可以儲存在本機 Credential 購物服務區中。

若要了解驗證方式，請查看[Windows 驗證概念](windows-authentication-concepts.md)。

本主題描述如下：

-   [互動式登入](#BKMK_InteractiveLogon)

-   [網路登入](#BKMK_NetworkLogon)

-   [智慧卡登入](#BKMK_SmartCardLogon)

-   [生物特徵辨識登入](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>互動式登入
登入程序開始，當使用者的認證項對話方塊中，輸入認證或使用者智慧卡插入智慧卡讀卡機或時使用的生物特徵辨識裝置的使用者互動。 使用者可以使用當地帳號或核對登入電腦執行互動式登入。

下圖顯示互動式登入的項目和登入程序。

![圖表顯示互動式登入的項目和登入程序](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Windows Client 驗證架構**

### <a name="BKMK_LocaDomainLogon"></a>本機和網域登入
網域登入的使用者提供的認證包含的所有項目所需的本機登入，例如 account 名稱與密碼，或憑證，以及 Active Directory domain 資訊。 此程序確認使用者的本機電腦上的安全性資料庫或 Active Directory domain 使用者的驗證。 這個管轄登入程序無法網域中的使用者關閉。

使用者可以執行互動式登入電腦中的兩種方式：

-   本機，當使用者可以直接存取實體存取到電腦，或電腦的網路的電腦屬於。

    登入本機授與的使用者存取本機電腦上的 Windows 資源的權限。 本機登入需要使用者有帳號中安全性帳號 Manager（坡）本機電腦上。 薩姆保護及管理使用者和群組資訊儲存在本機電腦登錄安全性帳號的格式。 電腦都可以有網路存取權，而不需要。 本機使用者 account 與群組成員資格資訊用來管理本機資源的存取權。

    網路登入授與使用者的權限存取網路的電腦上的任何資源除了本機電腦上的 Windows 資源所定義的憑證存取預付碼。 本機登入和網路登入需要使用者有帳號中安全性帳號 Manager（坡）本機電腦上。 本機使用者 account 與群組成員資格資訊用來管理存取本機資源，以及存取的使用者權杖定義資源可以存取網路的電腦上。

    本機登入和網路登入並非足以授與的使用者及電腦權限存取並使用網域資源。

-   遠端電腦上，到車票服務或遠端桌面服務 (RDS)，其中案例登入是進一步限定為遠端互動。

後互動式登入，Windows 執行的應用程式的使用者，代表，使用者可以使用這些應用程式互動。

登入本機授與的本機電腦上的存取資源或網路的電腦上的資源使用者權限。 如果電腦已經加入網域，Winlogon 功能嘗試登入的網域。

登入網域授與使用者的權限存取本機和網域資源。 登入網域需要使用者有帳號 Active Directory 中。 電腦在 Active Directory domain 必須帳號，並實際連接到網路。 使用者必須也有使用者登入本機電腦或網域的權限。 網域使用者 account 群組成員資格資訊和可用來管理存取網域和本機資源。

### <a name="BKMK_RemoteLogon"></a>遠端登入
在 Windows 中，透過登入遠端存取其他電腦需要在 [遠端桌面通訊協定 (RDP)。 使用者必須已經有成功登入 client 的電腦之前，請先嘗試遠端連接，因為已成功完成登入互動式處理程序。

RDP 管理使用者透過遠端桌面 Client 輸入認證。 這些認證供的目標電腦上，以及使用者必須帳號的目標電腦上。 此外，必須接受連接遠端設定目標電腦。 傳送目標電腦認證嘗試執行驗證程序。 如果成功驗證，到本機連接使用者及網路資源，都可供使用提供的認證。

## <a name="BKMK_NetworkLogon"></a>網路登入
驗證使用者、服務或電腦發生之後，可以只使用網路登入。 網路登入時程序不會收集資料使用對話方塊認證項目。 而是先前建立的認證或收集認證另一個方法使用。 此程序確認使用者的身分，以使用者想要存取的任何網路的服務。 這個程序會向使用者通常看不到，除非替代憑證有提供。

若要提供這種類型的驗證，安全性系統包含這些驗證機制：

-   Kerberos 5 版本通訊協定

-   公開憑證

-   安全通訊端層日 Tls (SSL 日 TLS)

-   摘要

-   NTLM，適用於 Microsoft Windows nt4.0 型系統的相容性

資訊的項目和處理程序，會看到上互動式登入圖。

## <a name="BKMK_SmartCardLogon"></a>智慧卡登入
登入，才能網域帳號，而不是本機帳號，可以使用智慧卡。 智慧卡驗證要求 Kerberos 驗證通訊協定的使用。 引進 Windows 2000 Server、中的 Windows 架構作業系統公開金鑰延伸實作通訊協定的初始驗證要求 Kerberos。 相較於共用密碼加密，公用加密非對稱式，也就是需要兩個不同的金鑰：加密解密另一個。 在一起，按鍵，才能執行兩項作業組成公開私密金鑰日的金鑰。

若要初始化一般工作階段，使用者必須證明對方可用自己的身分提供只有知道使用者和基礎 Kerberos 通訊協定基礎結構的資訊。 秘密資訊是密碼編譯共用的按鍵推斷的使用者的密碼。 共用的私密金鑰是對稱，這表示相同的按鍵用於加密與解密。

下圖顯示的項目和所需的智慧卡登入的處理程序。

![圖表顯示的項目和處理程序所需的智慧卡登入](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**智慧卡認證提供者架構**

而非密碼使用智慧卡時，會取代公開私密金鑰日的金鑰儲存在使用者的智慧卡上共用金鑰，從使用者的密碼。 私密金鑰儲存只能在智慧卡上。 公開鍵可提供給任何人收件者的擁有者想要交換機密資訊。

如需 Windows 中的智慧卡登入程序，請查看[智慧卡登入方式 Windows 在](https://technet.microsoft.com/library/ff404285.aspx)。

## <a name="BKMK_BioLogon"></a>生物特徵辨識登入
使用擷取和組建數位特性成品，例如指紋的裝置。 這個數位表示然後相較於樣本的相同成品，和這兩個成功相較於，當發生驗證。 電腦執行的作業系統中指定的任何**適用於**清單中的開頭本主題，可以設定接受這種登入。 不過，如果生物特徵辨識登入只本機登入的設定，使用者必須存取 Active Directory domain 時顯示網域認證。

## <a name="additional-resources"></a>其他資源
適用於 Windows 管理提交在登入時憑證的方式的相關資訊，請查看[在 Windows 驗證認證管理](https://technet.microsoft.com/library/dn169014.aspx)。

[Windows 登入和驗證的技術概觀](https://technet.microsoft.com/library/dn169029.aspx)


