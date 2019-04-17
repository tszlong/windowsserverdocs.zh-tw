---
title: "Windows 驗證概念"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 27e124c971926edd33f102fe6009a0c552c6d814
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-concepts"></a>Windows 驗證概念

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這個參考概觀主題描述 Windows 驗證所依據的概念。

驗證是來確認人員或物件的身分處理程序。 當您進行驗證物件時，目標是驗證物件是正版軟體。 當您進行驗證的人員時，目標是來確認人員不詐欺。

網路功能的操作，以驗證是網路的應用程式或資源證明身分的行為。 一般而言，身分是證明密碼編譯作業任一個按鍵僅使用者知道（如同公開加密）該使用或共用的按鍵。 驗證換貨的伺服器端比較簽署的資料與已知驗證嘗試密碼編譯金鑰。

延展性，而且可維護密碼編譯金鑰儲存在安全的中央位置可驗證程序。 Active Directory 建議，預設儲存身分的資訊，包括密碼編譯金鑰的技術按鍵的使用者的認證。 Active Directory 是必要的預設 NTLM 和 Kerberos 實作。

驗證技術範圍從簡單的登入至作業系統或登入服務或使用者將辨識應用程式根據項目僅使用者知道，密碼，例如使用權杖公開金鑰憑證，為使用者 has'such 圖片、的項目越安全機制或：屬性。 在企業環境中，使用者可能會存取許多類型的在單一位置或跨多個位置的伺服器上的多個應用程式。 基於這些原因，驗證必須支援其他平台和其他 Windows 作業系統的環境。

## <a name="authentication-and-authorization-a-travel-analogy"></a>驗證和授權：旅行類比
旅行類比可以解釋驗證的運作方式。 一些準備工作，都是通常是為了開始旅程。 旅行者必須向其主機授權單位證明他們為 true 的身分。 此證明可證明表現、出生地點、個人化的憑證，相片，或任何所需的主機國家/地區的法律的格式。 Passport，這是發行，管理組織-的安全性原則系統 account 類似的發行驗證時旅行的身分。 Passport 預期的目的根據一組規則及法規發出政府授權。

**之旅**

當旅行到達國際外框時，邊境 guard 要求的認證，和旅行者，會顯示她或護照。 此程序是雙重：

-   藉由驗證的發行安全性授權單位本機政府信任（信任，至少發行護照），並藉由驗證 passport 未經過修改，guard 驗證護照。

-   Guard 驗證旅行者確認臉部符合附圖護照之人員的臉孔和其他必要的認證，是很好的順序。

如果確實有效 passport 旅行證明做為其擁有者，驗證成功，且旅行者可以存取允許邊境上。

轉移信任之間安全性授權單位是驗證; 的基礎驗證時國際類型根據信任。 本機政府不知道旅行者，但它信任主機政府，並會。 當主機政府發出護照時，它不知道旅行者架構。 信任的憑證出生或其他文件發出機構。 發行出生憑證，機構，信任醫師簽署的憑證。 醫師見證旅行的生日，並選取憑證直接證明的身分，這種情形下與新生的使用量。 信任傳輸受信任的橋樑，透過這種方式，是轉移。

轉移信任是在 Windows server client 日架構網路安全性基本知識。 建立信任關係整個設定的網域，例如「網域樹狀結構流程和網域和所有的網域信任的網域之間的關係。 例如，網域 A 轉移信任的網域 B，如果，如果 B 網域信任的網域 C 然後網域 A 信任網域 c。

還有驗證與授權之間的人而有所不同。 使用驗證時，系統證明您是您的身分。 授權，系統會確認您具有權限，才能執行您想要的項目。 若要需要邊境類比下一個步驟，只驗證旅行正確的有效 passport 擁有者會不一定授權旅行輸入國家/地區。 輸入其他國家/地區，只要只能在輸入的國家/地區位置授與無限制的權限所有市民該特定國家之輸入情形簡報 passport 允許居民特定國家/地區。

同樣地，您可以授與所有使用者從存取資源特定網域權限。 所屬的網域擁有的存取權的資源，就像加拿大我們美國市民輸入加拿大所有使用者。 不過，嘗試輸入巴西或印度美國市民找到，他們無法只是透過簡報 passport，因為這兩個這些國家/地區需要有有效的 visa 美國市民瀏覽輸入這些國家/地區。 因此，驗證，並不保證資源或使用資源授權的存取權。

## <a name="credentials"></a>認證
Passport 與 visas 可能相關聯的旅行的接受的認證。 不過，這些認證可能會讓旅客輸入或存取所有資源國家/地區中。 例如，需要額外的認證出席研討會」。 在 Windows 中，可讓您存取網路上的資源，而不必重複提供認證 account 持有可能管理認證。 這種類型的存取可讓使用者一次系統存取 [所有應用程式驗證，資料來源，它們授權使用，而不輸入另一個 account 識別碼或密碼。 Windows 平台將使用在網路上的單一的使用者身分（維護的 Active Directory）本機快取使用者的認證在作業系統的本機安全性授權單位 (LSA) 的功能。 當使用者登入網域時，Windows 驗證套件無障礙使用認證驗證憑證的網路資源時提供單一登入。 如需有關的認證，請查看[在 Windows 驗證認證處理程序](credentials-processes-in-windows-authentication.md)。

一種旅行多因素驗證可能會執行並顯示多個文件，以驗證他的身分，例如 passport 與會議登記資訊的需求。 Windows 會實作表單或智慧卡、virtual 智慧卡，與生物特徵辨識技術透過驗證。 

## <a name="security-principals-and-accounts"></a>安全性主體和帳號
在 Windows 中，任何使用者、服務、群組或電腦，就可以開始行動是安全性原則。 安全性主體有帳號，這可以是本機電腦或網域型。 例如，Windows client 加入網域的電腦可以參與，即使在不人性化使用者登入通訊的網域控制站網路網域。 若要開始進行通訊，電腦必須使用帳號網域中。 之前接受電腦的通訊，網域控制站的本機安全性授權單位會電腦的身分、驗證和人性化安全性主體一樣，然後定義電腦的安全性操作。 本文中的安全性定義特定電腦或使用者、服務、群組中或在網路上的電腦上的身分和使用者或服務的功能。 例如，它會定義共用檔案或印表機，可以存取和的動作，例如朗讀、寫入或修改，使用者、服務，或從資源的電腦可執行的資源。 如需詳細資訊，請查看[安全性主體](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals)。

Account 是一種方法來找出 claimant-人性化的使用者或服務-要求存取或資源。 會真確 passport 旅行擁有 account 主機國家/地區使用。 使用者的使用者、物件和服務群組都能個人帳號或共用帳號。 帳號特定的權利和權限] 可指派，而且可以群組成員。 帳號可以限制本機電腦位於工作群組、網路，或指定成員資格加入網域。

每個版本的 Windows 定義建帳號及安全性群組他們的成員。 使用安全性群組，您可以相同安全性將權限指派給成功驗證，許多使用者的簡化存取管理。 發行護照規則可能需要旅行給 business 或政府旅遊資訊，例如特定群組。 此程序群組的所有成員確保一致的安全性權限。 使用安全性群組將權限指派常且容易管理和稽核，app 會維持表示存取資源的控制項。 新增或移除視需要從安全性群組適當存取權的使用者，您可以將最小化變更存取控制清單 (Acl) 的頻率。

獨立管理服務帳號，並 virtual 帳號帶來 Windows Server 2008 R2 和 Windows 7 中提供所需的應用程式，例如 Microsoft Exchange Server 和網際網路資訊服務 (IIS)，以他們自己的網域帳號的隔離時手動不需要系統管理員可以管理這些帳號認證與服務主體名稱 (SPN)。 群組受管理的服務帳號帶來 Windows Server 2012 和提供相同功能網域中的，但也到多部伺服器擴充功能。 連接到服務，伺服器發電廠，例如網路負載平衡裝載時支援互加好友的驗證，驗證通訊協定要求服務的所有執行個體使用相同的原則。

如需有關帳號:

-   [Active Directory 帳號](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Active Directory 安全性群組](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [本機帳號](https://technet.microsoft.com/itpro/windows/keep-secure/local-accounts)

-   [Microsoft 帳號](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [服務帳號](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [特殊身分](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>委派的驗證
若要使用的旅行類比，國家可能會發出相同的存取權官方政府委派的所有成員只要代理人是已知。 這個委派我們一個成員處理的其他成員的授權。 在 Windows 中，委派的驗證時發生網路服務接受要求驗證使用者以及假設的使用者身分，以開始新的連接到第二個網路的服務。 若要支援委派的驗證，您必須建立前端或第一層伺服器，例如 web 伺服器，是負責處理 client 驗證要求和後端或 n 層伺服器，例如大型資料庫，是負責儲存資訊。 您可以委派委派驗證使用者在組織中設定降低負載管理您的系統管理員權限。

透過建立服務或電腦做為委派受信任，您可以讓該服務或電腦完成委派的驗證、收到提出要求，使用者的票證，然後存取的使用者的資訊。 此模型限制後端的伺服器上的資料的存取權的使用者或服務來具有正確的存取控制權杖該出現認證。 此外，它可讓存取稽核那些後端資源。 藉由要求的認證委派伺服器代表 client 使用透過存取所有的資料，就可以確保伺服器無法受到和，您可以存取敏感資訊的已儲存在其他伺服器上。 委派的驗證非常適合多設計用來在多部電腦之間，使用單一登入功能的應用程式。

### <a name="authentication-in-trust-relationships-between-domains"></a>在信任的網域之間的關係驗證
有一個以上的網域有合法大部分組織必須使用者存取不同的網域中的共用的資源，就像旅行允許搭乘國家/地區不同地區。 控制此存取需要也可以 authenticated 和授權使用另一部網域中的資源一個網域中的使用者。 若要提供戶端之間不同網域中的伺服器的驗證和授權功能，必須有信任在兩個網域。 信任的基礎技術安全 Active Directory 通訊發生和是 Windows Server 網路架構不可或缺的安全性元件。

信任的網域兩個之間時，每個網域驗證機制信任來自其他網域驗證。 信任協助提供資源網域-信任的網域-中共用資源存取控制藉由驗證連入驗證要求都來自信任的授權單位-受信任的網域。 如此一來，信任做為只讓橋接器已驗證網域之間驗證要求出差。

如何特定信任通過驗證要求的設定方式而定。 藉由提供給其他網域中的資源的存取權的每個網域可以單向提供存取來自受信任的網域信任的網域中的資源或雙向，信任關係。 也是個非轉移，案例信任存在只之間的兩個信任合作夥伴網域，或轉移，此時信任自動延伸到其他合作夥伴之一信任的網域信任。

信任的運作方式的相關資訊，請查看[如何網域和信任的樹系工作](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx)。

### <a name="protocol-transition"></a>通訊協定轉換
通訊協定轉換協助設計的應用程式的工具，可讓應用程式支援不同的驗證，驗證使用者層的機制並切換到 Kerberos 通訊協定的安全性功能，例如互加好友的驗證並限制委派，在 [應用程式後續層級。

如需通訊協定轉換，查看[Kerberos 通訊協定轉換和限制委派](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx)。

### <a name="constrained-delegation"></a>限制的委派
限制的委派提供系統管理員指定並執行的應用程式信任邊界藉由限制位置服務的應用程式可以做代表使用者範圍的功能。 您可以指定委派受信任的電腦，可以要求資源特定的服務。 處於限制授權服務的權限來協助改進的應用程式安全性設計服務未減少危害的機會。

如需委派限制的詳細資訊，請查看[Kerberos 限制委派概觀](../kerberos/kerberos-constrained-delegation-overview.md)。

## <a name="see-also"></a>也了
[Windows 登入和驗證的技術概觀](https://technet.microsoft.com/library/dn269029.aspx)


