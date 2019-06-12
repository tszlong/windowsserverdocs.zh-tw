---
title: AD FS 密碼攻擊保護
description: 本文件說明如何從密碼破解攻擊保護 AD FS 的使用者
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5666943138070cfa8cfe62f1ba932c2793daa003
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501596"
---
# <a name="ad-fs-password-attack-protection"></a>AD FS 密碼攻擊保護

## <a name="what-is-a-password-attack"></a>什麼是密碼攻擊？

同盟單一登入的需求是透過網際網路進行驗證的端點的可用性。 在網際網路上的驗證端點的可用性可讓使用者存取應用程式，即使它們不在公司網路上。 

不過，這也表示一些不良執行者可以利用網際網路上可用的聯合端點，並使用這些端點，請嘗試並判斷密碼，或建立阻絕服務攻擊。 一種變得更為普遍這類攻擊會呼叫**密碼攻擊**。 

有 2 種類型的常見密碼破解攻擊。 密碼噴灑攻擊和暴力密碼破解強制密碼攻擊。 

### <a name="password-spray-attack"></a>密碼噴灑攻擊
在密碼噴灑攻擊中，這些不良執行者會跨許多不同的帳戶和服務，以便讓他們能夠找到任何受密碼保護的資產的存取嘗試的最常見的密碼。 通常這些跨許多不同的組織和身分識別提供者。 比方說，攻擊者會使用常用的工具組來列舉所有的數個組織中的使用者，然後再次嘗試 「 P@$$w0rd"和"Password1 」，針對所有這些帳戶。 若要讓您了解，攻擊可能會看起來像：


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

此攻擊模式漏網之魚大部分偵測技術，因為從有利點個別使用者或公司的攻擊只看起來像隔離失敗的登入。

攻擊者，它是數字遊戲： 他們知道有某些密碼有非常常見。  攻擊者會取得一些成功的攻擊時，每個數千個帳戶，而言就已足夠生效。 使用的帳戶，可從電子郵件中取得資料、 蒐集連絡資訊，然後傳送網路釣魚的連結，或者只展開密碼噴灑目標群組。 攻擊者不太在意這些初始的目標是人員 — 只要他們有它們可以利用一些成功。

但是，藉由採取一些步驟來設定 AD FS 和正確的網路，您可以保護 AD FS 端點針對這些類型的攻擊。 本文章涵蓋需要正確設定，來協助保護這些攻擊的 3 個區域。

### <a name="brute-force-password-attack"></a>暴力密碼破解攻擊 
在這種形式的攻擊，攻擊者會嘗試多次密碼嘗試，對一組目標的帳戶。 在許多情況下這些帳戶會有較高層級的組織內的存取權的使用者設為目標。 這些可能是組織內部或是系統管理員管理重要的基礎結構的主管。  

此類型的攻擊也可能導致 DOS 模式。 這可能是無法處理要求，因為伺服器沒有足夠數目的大型 # ADFS 所在的服務層級，或可能是在使用者層級使用者鎖定其帳戶的位置。  

## <a name="securing-ad-fs-against-password-attacks"></a>保護對密碼破解攻擊的 AD FS 

但是，藉由採取一些步驟來設定 AD FS 和正確的網路，您可以保護 AD FS 端點針對這些類型的攻擊。 本文章涵蓋需要正確設定，來協助保護這些攻擊的 3 個區域。 


- 基準層級 1:這些是必須確保不良執行者無法暴力密碼破解攻擊的同盟使用者的 AD FS 伺服器設定的基本設定。 
- 保護外部網路的層級 2:這些是必須以確保外部網路存取設定為使用安全的通訊協定、 驗證原則和適當的應用程式設定的設定。 
- 外部網路存取的層級 3，移至 無密碼：這些都被進階設定和指導方針，以存取同盟資源更安全的認證，而非密碼，也就是容易遭到攻擊。 

## <a name="level-1-baseline"></a>等級 1:基準

1. 如果 ADFS 2016 中，實作[外部網路的智慧鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)外部網路的智慧鎖定會追蹤熟悉的位置，而且允許有效的使用者，如果使用者先前登入已成功從該位置通過。 藉由使用外部網路的智慧鎖定，您可以確保不良執行者將無法再以暴力密碼破解攻擊的使用者，並在同一時間可讓合法使用者提高產能。
    - 如果您不需 AD FS 2016 中，我們強烈建議您[升級](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)至 AD FS 2016。 這是簡單的升級路徑從 AD FS 2012 R2。 如果您是在 AD FS 2012 R2 上，實作[外部網路鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。 這種方法的缺點是從外部網路存取可能會封鎖的有效使用者，如果您是在暴力密碼破解模式。 在 Server 2016 上的 AD FS 並沒有這個缺點。

2. 監視及封鎖可疑的 IP 位址 
    - 如果您有 Azure AD Premium，則在 ADFS 與使用實作 Connect Health[有風險的 IP 報告](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview)它所提供的通知。

        a. 授權的所有使用者並不需要 25 的授權/ADFS/WAP 伺服器可能會讓客戶輕鬆。

        b. 您現在可以調查 IP 的產生失敗的登入的大數目

        c. 這需要您在您的 ADFS 伺服器上啟用稽核。

3.  封鎖可疑的 IP。  這可能會封鎖 DOS 攻擊。

    a. 如果在 2016 年，則使用[*禁止的外部網路 IP 位址*](../../ad-fs/operations/configure-ad-fs-banned-ip.md) #3 （或手動分析） 設定來封鎖從 IP 的任何要求的功能旗標。

    b. 如果您是在 AD FS 2012 R2 或更低，禁止直接在 Exchange Online，並選擇性地在防火牆上的 IP 位址。

4. 如果您有 Azure AD Premium，使用[Azure AD 密碼保護](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises)以放入 Azure AD 時，防止猜到的密碼  

    a. 請注意，是否您已經猜到的密碼時，您便可以打開它們與只是 1 到 3 的嘗試。 這項功能可防止從取得。 

    b. 幾乎我們預覽統計資料，從 20 到 50%的新密碼取得封鎖所設定。 這表示在 %的使用者還是可能輕易猜到的密碼。

## <a name="level-2-protect-your-extranet"></a>等級 2:保護您的外部網路

5. 移至從外部網路存取的任何用戶端的新式驗證。 郵件用戶端是重要的這部分。 

    a. 您必須使用 Outlook Mobile 為行動裝置。 新的 iOS 原生郵件應用程式支援以及新式驗證。 

    b. 您必須使用 Outlook 2013 （含最新 CU 的修補程式） 或 Outlook 2016。

6. 啟用 MFA 的所有外部網路存取。 這讓您加入的任何外部網路存取保護中。

   a.  如果您有 Azure AD premium，使用[Azure AD 條件式存取原則](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)來控制這點。  這是優於實作 AD FS 的規則。  這是因為現代化的用戶端應用程式會更頻繁地強制執行。  發生這種情況，在 Azure AD 中，要求新存取權杖時 （通常每小時） 使用重新整理權杖。  

   b.  如果您沒有 Azure AD premium 或已在 AD FS 可讓網際網路上的其他應用程式為基礎的存取，實作的 MFA （可以是 Azure MFA 也在 AD FS 2016） 執行一項[全域的 MFA 原則](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally)所有外部網路存取。

## <a name="level-3-move-to-password-less-for-extranet-access"></a>等級 3:移至密碼較少的外部網路存取

7. 移至 Window 10，並用[Hello For Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)。

8. 針對其他裝置，如果在 AD FS 2016，您可以使用[Azure MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md)的第一個因素和密碼做為第 2 個因素。 

9. 針對行動裝置，如果您只允許受管理的 MDM 裝置，您可以使用[憑證](../../ad-fs/operations/configure-user-certificate-authentication.md)登入的使用者。 

## <a name="urgent-handling"></a>緊急的處理

如果 AD FS 環境在作用中的攻擊之下，應該盡早實作的下列步驟：

 - 停用在 ADFS 中的 U/P 端點，而且需要每個人以取得存取或在您的網路內的 VPN。 這必須要有步驟**層級 2 #1a**完成。 否則，所有內部 Outlook 將仍將要求路由傳送透過雲端透過 EXO proxy 驗證
 - 如果透過 EXO 即將只到攻擊，您可以停用 Exchange 通訊協定 （POP、 IMAP、 SMTP、 EWS 等等） 的基本驗證使用驗證原則，這些通訊協定和驗證方法上使用大部分的這些攻擊。 此外，在 EXO 和每個信箱的通訊協定啟用的用戶端存取規則會評估的後續驗證，並不會協助上緩和攻擊。 
 - 選擇性地提供外部網路存取使用層級 3 #1-3。

## <a name="next-steps"></a>後續步驟

- [升級至 2016年的 AD FS 伺服器](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [外部網路 AD FS 2016 中的智慧鎖定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [設定條件式存取原則](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Azure AD 密碼保護](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
