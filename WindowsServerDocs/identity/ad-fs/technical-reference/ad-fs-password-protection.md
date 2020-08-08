---
title: AD FS 密碼攻擊保護
description: 本檔說明如何保護 AD FS 使用者免于密碼攻擊
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.author: billmath
ms.openlocfilehash: c17ef0524ed873c8a3d506df75542848e2c4bca7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956155"
---
# <a name="ad-fs-password-attack-protection"></a>AD FS 密碼攻擊保護

## <a name="what-is-a-password-attack"></a>什麼是密碼攻擊？

同盟單一登入的需求是透過網際網路驗證端點的可用性。 網際網路上的驗證端點可用性可讓使用者存取應用程式，即使它們不在公司網路上也一樣。

不過，這也表示有些不良的執行者可以利用網際網路上提供的同盟端點，並使用這些端點來嘗試和判斷密碼，或建立阻絕服務攻擊。 其中一個變得較常見的攻擊稱為**密碼攻擊**。

常見的密碼攻擊有2種類型。 密碼噴灑攻擊 & 暴力密碼破解攻擊。

### <a name="password-spray-attack"></a>密碼噴灑攻擊
在密碼噴灑攻擊中，這些不良的執行者會嘗試跨許多不同帳戶和服務的最常見密碼，以存取他們可以找到的任何受密碼保護的資產。 這些通常會跨越許多不同的組織和身分識別提供者。 例如，攻擊者會使用常用的工具組來列舉數個組織中的所有使用者，然後針對所有這些帳戶嘗試「P@ $ $w 0rd」和「Password1」。 為了提供您的想法，攻擊可能如下所示：


|  目標使用者   | 目標密碼 |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P@$$w0rd     |
| User2@org1.com |    P@$$w0rd     |
| User1@org2.com |    P@$$w0rd     |
| User2@org2.com |    P@$$w0rd     |

這種攻擊模式會 evades 大部分的偵測技術，因為從個別使用者或公司的有利點，攻擊就好像是隔離的失敗登入。

對於攻擊者而言，這是一個數位的遊戲：他們知道有一些密碼是很常見的。  攻擊者會在每一千個帳戶遭受攻擊的成功，這就足以有效。 他們會使用這些帳戶從電子郵件取得資料、搜集連絡人資訊，以及傳送網路釣魚連結，或直接展開密碼噴灑目標群組。 攻擊者並不在意這些初始目標的物件，只是他們有一些成功可以利用的經驗。

但只要採取幾個步驟，就能正確設定 AD FS 和網路，AD FS 的端點就可以受到保護，以免受到這類攻擊的威脅。 本文涵蓋3個需要適當設定的區域，以協助保護這些攻擊。

### <a name="brute-force-password-attack"></a>暴力密碼破解攻擊
在這種攻擊形式中，攻擊者會嘗試對一組目標帳戶進行多次密碼嘗試。 在許多情況下，這些帳戶會以組織內具有較高層級存取權的使用者為目標。 這些可能是組織內的主管，或是管理關鍵基礎結構的系統管理員。

這種類型的攻擊也可能導致 DOS 模式。 這可能是在服務層級上，ADFS 無法處理大量要求，因為伺服器數目不足，或可能是使用者已鎖定其帳戶的使用者層級。

## <a name="securing-ad-fs-against-password-attacks"></a>保護 AD FS 不受密碼攻擊

但只要採取幾個步驟，就能正確設定 AD FS 和網路，AD FS 的端點就可以受到保護，以免受到這些類型的攻擊。 本文涵蓋3個需要適當設定的區域，以協助保護這些攻擊。


- 層級1，基準：這些是必須在 AD FS 伺服器上設定的基本設定，以確保不良的執行者無法暴力攻擊同盟的使用者。
- 層級2，保護外部網路：這些是必須設定的設定，以確保外部網路存取設定為使用安全通訊協定、驗證原則和適當的應用程式。
- 層級3，移至不含密碼的外部網路存取權：這些是先進的設定和指導方針，可讓您以更安全的認證（而不是容易遭受攻擊的密碼）存取同盟資源。

## <a name="level-1-baseline"></a>層級1：基準

1. 如果 ADFS 2016，請執行[外部網路智慧鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)外部網路智慧鎖定會追蹤熟悉的位置，如果他們先前已成功從該位置登入，就會允許有效的使用者通過。 藉由使用外部網路智慧鎖定，您可以確保不良的執行者無法暴力破解攻擊使用者，同時讓合法的使用者有生產力。
    - 如果您不是在 AD FS 2016，我們強烈建議您[升級](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)至 AD FS 2016。 這是來自 AD FS 2012 R2 的簡單升級路徑。 如果您是在 AD FS 2012 R2，請執行[外部網路鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。 這種方法的其中一個缺點是，如果您採用暴力密碼破解模式，有效的使用者可能會被封鎖外部網路存取。 伺服器2016上的 AD FS 沒有這個缺點。

2. 監視 & 封鎖可疑的 IP 位址
    - 如果您已 Azure AD Premium，請執行 ADFS 的 Connect Health，並使用它所提供的具[風險 IP 報告](/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview)通知。

        a. 授權並非適用于所有使用者，而且需要25個授權/ADFS/WAP 伺服器，客戶可能會很容易。

        b. 您現在可以調查正在產生大量失敗登入的 IP

        c. 這會要求您在 ADFS 伺服器上啟用審核。

3.  封鎖可疑 IP 的。  這可能會封鎖 DOS 攻擊。

    a. 如果在2016，請使用[*外部網路禁用 IP 位址*](../../ad-fs/operations/configure-ad-fs-banned-ip.md)功能，以封鎖 #3 (或手動分析) 的任何來自 IP 的要求。

    b. 如果您是在 AD FS 2012 R2 或更低版本，請直接在 Exchange Online 和您的防火牆上封鎖 IP 位址。

4. 如果您有 Azure AD Premium，請使用[Azure AD 密碼保護](/azure/active-directory/authentication/concept-password-ban-bad-on-premises)來防止猜到密碼進入 Azure AD

    a. 請注意，如果您有猜到的密碼，只要1-3 嘗試就可以破解。 這項功能可防止這些設定取得。

    b. 從我們的預覽統計資料中，幾乎20-50% 的新密碼遭到封鎖而無法設定。 這表示有% 的使用者很容易被猜到密碼。

## <a name="level-2-protect-your-extranet"></a>層級2：保護您的外部網路

5. 針對從外部網路存取的任何用戶端移至新式驗證。 Mail 用戶端是這個很重要的一部分。

    a. 您將需要使用行動裝置的 Outlook Mobile。 新的 iOS 原生郵件應用程式也支援新式驗證。

    b. 您將需要搭配最新的 CU 修補程式) 或 Outlook 2016 使用 Outlook 2013 (。

6. 針對所有外部網路存取啟用 MFA。 這可讓您針對任何外部網路存取提供額外的保護。

   a.  如果您有 Azure AD premium，請使用[Azure AD 條件式存取原則](/azure/active-directory/conditional-access/overview)來控制這種情況。  這比在 AD FS 中執行規則的效果更好。  這是因為現代化的用戶端應用程式會以更頻繁的方式來強制執行。  這會在 Azure AD，要求新的存取權杖時，通常每隔一小時 () 使用重新整理權杖。

   b.  如果您沒有 Azure AD premium 或在 AD FS 上有其他應用程式允許以網際網路為基礎的存取，則在 AD FS 2016) 上執行 MFA (也可以是 Azure MFA，並針對所有外部網路存取執行[全域 MFA 原則](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally)。

## <a name="level-3-move-to-password-less-for-extranet-access"></a>層級3：針對外部網路存取，移至較少的密碼

7. 移至 [Window 10] 並使用 [ [Hello 企業版](/windows/security/identity-protection/hello-for-business/hello-identity-verification)]。

8. 針對其他裝置，如果在 AD FS 2016 上，您可以使用[AZURE MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md)作為第二個因素，做為第一個因素和密碼。

9. 對於行動裝置，如果您只允許受 MDM 管理的裝置，您可以使用[憑證](../../ad-fs/operations/configure-user-certificate-authentication.md)來登入使用者。

## <a name="urgent-handling"></a>緊急處理

如果 AD FS 環境受到作用中的攻擊，則應該儘早執行下列步驟：

 - 停用 ADFS 中的 U/P 端點，並要求每個人都可以存取或在您的網路內。 這會要求您必須先完成步驟**層級 2 #1a** 。 否則，所有內部 Outlook 要求仍然會透過雲端透過 EXO proxy 驗證來路由傳送。
 - 如果攻擊只是透過 EXO，您可以停用 Exchange 通訊協定的基本驗證 (POP、IMAP、SMTP、EWS 等等) 使用驗證原則，這些通訊協定和驗證方法會在大部分的攻擊中使用。 此外，EXO 和每個信箱通訊協定啟用的用戶端存取規則會在驗證後進行評估，而且不會協助減輕攻擊。
 - 選擇性地使用層級 3 #1-3 提供外部網路存取。

## <a name="next-steps"></a>後續步驟

- [升級至 AD FS server 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)
- [AD FS 2016 中的外部網路智慧鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [設定條件式存取原則](/azure/active-directory/conditional-access/overview)
- [Azure AD 密碼保護](/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
