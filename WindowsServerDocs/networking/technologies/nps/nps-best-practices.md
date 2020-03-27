---
title: 網路原則伺服器最佳做法
description: 本主題提供在 Windows Server 2016 中部署和管理網路原則伺服器的最佳作法。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 278d813aa13ea42b7f597bdbe7eb210f68cee955
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316300"
---
# <a name="network-policy-server-best-practices"></a>網路原則伺服器最佳做法

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解部署和管理網路原則伺服器 \(NPS\)的最佳作法。

下列各節提供 NPS 部署不同層面的最佳作法。

## <a name="accounting"></a>計量

以下是 NPS 記錄的最佳作法。

NPS 中有兩種類帳戶處理或記錄：

- NPS 的事件記錄。 您可以使用事件記錄來記錄系統與安全性事件日誌中的 NPS 事件。 這主要是用來對連線嘗試進行稽核和疑難排解。

- 記錄使用者驗證和帳戶處理要求。 您可以將使用者驗證與帳戶處理要求，記錄到文字格式或資料庫格式的記錄檔中，或是記錄到 SQL Server 2000 資料庫的預存程序中。 要求記錄主要是用來進行連線分析和計費，而且也很適合做為安全性調查工具，提供您追蹤攻擊者活動的方法。

充分有效地利用 NPS 記錄：

- 針對驗證和帳戶處理記錄，開啟記錄 \(一開始\)。 在判斷出適合您環境的項目之後，修改這些選項。

- 確定為事件記錄設定足夠的容量來維護您的記錄。

- 定期備份所有記錄檔，因為它們在損毀或刪除時無法重新建立。

- 使用 RADIUS 類別屬性來追蹤使用狀況，並針對需要付費才能使用的部門或使用者簡化識別作業。 雖然自動產生的類別屬性對每個要求來說是唯一的，但在遺失存取伺服器回覆以及重新傳送要求的狀況下，可能會出現重複的記錄。 您可能需要從記錄刪除重複的要求，才能正確地追蹤使用狀況。

- 如果您的網路存取伺服器和 RADIUS proxy 伺服器定期傳送虛構的連線要求訊息給 NPS，以確認 NPS 已上線，請使用**ping 使用者名稱**登錄設定。 此設定會將 NPS 設定為自動拒絕這些不處理的假連線要求。 此外，NPS 不會在任何記錄檔中記錄涉及虛構使用者名稱的交易，讓事件記錄檔更容易解讀。

- 停用 NAS 通知轉送。 您可以停用從網路存取伺服器（Nas）轉送啟動和停止訊息到 NPS 中設定的遠端 RADIUS 伺服器群組的成員。 如需詳細資訊，請參閱[停用 NAS 通知轉送](nps-disable-nas-notifications.md)。

如需詳細資訊，請參閱[設定網路原則伺服器帳戶](nps-accounting-configure.md)處理。

- 若要使用 SQL Server 記錄提供容錯移轉與備援，請將兩台執行 SQL Server 的電腦放在不同的子網路上。 使用 SQL Server**建立發行集** ，在兩部伺服器之間設定資料庫複寫。 如需詳細資訊，請參閱[SQL Server 技術檔](https://msdn.microsoft.com/library/ms130214.aspx)和[SQL Server 複寫](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>驗證

以下是驗證的最佳作法。

- 使用以憑證為基礎的驗證方法，例如受保護的可延伸驗證通訊協定 \(PEAP\) 和可延伸的驗證通訊協定 \(EAP\) 進行增強式驗證。 請勿使用僅限密碼的驗證方法，因為它們很容易遭受各種攻擊，而且並不安全。 針對安全的無線驗證，建議使用 PEAP\-MS\-CHAP v2，因為 NPS 會使用伺服器憑證向無線用戶端證明其身分識別，而使用者以其使用者名稱和密碼證明其身分識別。  如需在無線部署中使用 NPS 的詳細資訊，請參閱[部署以密碼為基礎的 802.1 x 驗證無線存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 當您使用需要在 Nps 上使用伺服器憑證的強式憑證驗證方法（例如 PEAP 和 EAP）時，您可以使用 Active Directory&reg; 憑證服務，將您自己的憑證授權單位單位 \(CA\) 部署到 \(AD CS\)。 您也可以使用自己的 CA 來註冊電腦憑證與使用者憑證。 如需將伺服器憑證部署至 NPS 和遠端存取服務器的詳細資訊，請參閱[部署 802.1 x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

> [!IMPORTANT]
> 網路原則伺服器（NPS）不支援在密碼中使用延伸的 ASCII 字元。

## <a name="client-computer-configuration"></a>用戶端電腦設定

以下是設定用戶端電腦時的最佳作法。

- 使用群組原則自動設定所有網域成員 802.1 X 用戶端電腦。 如需詳細資訊，請參閱[無線存取部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)主題中的「設定無線網路（IEEE 802.11）原則」一節。

## <a name="installation-suggestions"></a>安裝建議

以下是安裝 NPS 的最佳作法。

- 安裝 NPS 之前，請先使用本機驗證方法來安裝和測試每個網路存取伺服器，再將它們設定為 NPS 中的 RADIUS 用戶端。

- 安裝和設定 NPS 之後，請使用 Windows PowerShell 命令[匯出-import-npsconfiguration](https://technet.microsoft.com/library/jj872749.aspx)來儲存設定。 每次您重新設定 NPS 時，請使用此命令來儲存 NPS 設定。

>[!CAUTION]
>- 針對 RADIUS 用戶端和遠端 RADIUS 伺服器群組的成員，匯出的 NPS 設定檔案包含未加密的共用密碼。 因此，請確定您將檔案儲存到安全的位置。
>- 匯出程式不會在匯出的檔案中包含 Microsoft SQL Server 的記錄設定。 如果您將匯出的檔案匯入到另一個 NPS，您必須在新的伺服器上手動設定 SQL Server 記錄。

## <a name="performance-tuning-nps"></a>效能微調 NPS

以下是效能調整 NPS 的最佳作法。

- 若要將 NPS 驗證與授權回應時間最佳化，並將網路流量降至最低，請在網域控制站上安裝 NPS。

- 當使用 \(Upn 的通用主體名稱\) 或 Windows Server 2008 和 Windows Server 2003 網域時，NPS 會使用通用類別目錄來驗證使用者。 若要將執行此動作所花費的時間降到最低，請在通用類別目錄伺服器或與通用類別目錄伺服器位於相同子網的伺服器上安裝 NPS。

- 當您設定遠端 RADIUS 伺服器群組，並在 NPS 連線要求原則中，清除 [在**下列遠端 radius 伺服器群組中的伺服器上記錄帳戶處理資訊**] 核取方塊時，這些群組仍會傳送網路存取伺服器 \(NAS\) 啟動和停止通知訊息。 這會產生不必要的網路流量。 若要排除此流量，請清除 [**轉寄網路啟動和停止通知到這部伺服器**] 核取方塊，以停用每個遠端 RADIUS 伺服器群組中個別伺服器的 NAS 通知轉送。

## <a name="using-nps-in-large-organizations"></a>在大型組織中的使用 NPS

以下是在大型組織中使用 NPS 的最佳作法。

- 如果您使用網路原則來限制所有特定群組的存取權，請為您想要允許存取的所有使用者建立萬用群組，然後建立網路原則來授與此萬用群組的存取權。 請勿將所有的使用者直接放入萬用群組中，尤其是當在網路上有大量使用者的時候。 請改為建立屬於萬用群組成員的不同群組，然後將使用者新增至這些群組。

- 請儘可能使用使用者主要名稱來參照使用者。 使用者可以有相同的使用者主要名稱，不論成員資格為何。 這項作法提供具有大量網域的組織可能所需的延展性。

- 如果您已在網域控制站以外的電腦上安裝網路原則伺服器 \(NPS\)，而 NPS 每秒都會收到大量的驗證要求，您可以增加 NPS 和網域控制站之間允許的並行驗證次數，藉此改善 NPS 效能。 如需詳細資訊，請參閱 

## <a name="security-issues"></a>安全性問題

以下是減少安全性問題的最佳作法。

當您從遠端系統管理 NPS 時，請勿透過網路以純文字傳送敏感或機密資料（例如共用密碼或密碼）。 Nps 的遠端系統管理有兩種建議的方法：

- 使用遠端桌面服務來存取 NPS。 當您使用遠端桌面服務時，不會在用戶端與伺服器之間傳送資料。 只有伺服器的使用者介面（例如作業系統桌面和 NPS 主控台映射）會傳送至 Windows&reg; 10 中名為遠端桌面連線的遠端桌面服務用戶端。 用戶端會傳送鍵盤和滑鼠輸入，這會由已啟用遠端桌面服務的伺服器在本機處理。 當遠端桌面服務使用者登入時，他們只能查看由伺服器管理且彼此獨立的個別用戶端會話。 此外，遠端桌面連線在用戶端與伺服器之間還提供 128 位元的加密。

- 使用網際網路通訊協定安全性 (IPSec) 來加密機密資料。 您可以使用 IPsec 來加密 NPS 與您用來管理 NPS 的遠端用戶端電腦之間的通訊。 若要從遠端管理伺服器，您可以在用戶端電腦上安裝[適用于 Windows 10 的遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。 安裝之後，請使用 Microsoft Management Console （MMC）將 NPS 嵌入式管理單元新增至主控台。

>[!IMPORTANT]
>您只能在完整版的 Windows 10 Professional 或 Windows 10 企業版上安裝適用于 Windows 10 的遠端伺服器管理工具。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。

