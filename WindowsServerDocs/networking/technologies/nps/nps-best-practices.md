---
title: 網路原則伺服器最佳做法
description: 本主題提供部署和管理 Windows Server 2016 中的網路原則伺服器的最佳作法。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834229"
---
# <a name="network-policy-server-best-practices"></a>網路原則伺服器最佳做法

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解部署和管理網路原則伺服器的最佳做法\(NPS\)。

下列各節提供 NPS 部署的不同層面的最佳作法。

## <a name="accounting"></a>帳戶處理

以下是 NPS 記錄的最佳作法。

有兩種類型的計量，或記錄，在 NPS 中：

- Nps 事件記錄。 您可以使用將 NPS 事件記錄的事件記錄系統及安全性事件記錄檔中。 這主要用於稽核和疑難排解連線嘗試。

- 使用者驗證與帳戶處理要求記錄。 您可以記錄到以文字格式或資料庫格式記錄檔的使用者驗證和帳戶處理要求，或是記錄中的 SQL Server 2000 資料庫的預存程序。 要求記錄主要用於連線分析及計費用途，而且也做為安全性調查工具，讓您追蹤攻擊活動的方法很有用。

若要讓最有效利用 NPS 記錄：

- 開啟記錄功能\(最初\)進行驗證和帳戶處理記錄。 之後您決定適合您的環境，請修改這些選取項目。

- 請確定事件記錄已即足以維持您的記錄檔的容量。

- 備份定期執行的所有記錄檔，因為它們無法重新建立損毀或刪除時。

- 使用 RADIUS 類別屬性來追蹤使用情況和簡化的部門或使用者的使用量費用的識別。 雖然自動產生的類別屬性是唯一的每個要求，重複的記錄可能會存在到存取伺服器回覆時遺失，而且在重新傳送要求。 您可能需要刪除重複的要求，從您的記錄檔，才能正確地追蹤使用狀況。

- 如果您的網路存取伺服器和 RADIUS proxy 伺服器定期虛構的連線將要求訊息傳送到 NPS 確認 NPS 處於線上狀態，請使用**ping 使用者名稱**登錄設定。 此設定會設定 NPS 自動拒絕這些 false 的連線要求，而不需處理它們。 此外，NPS 不會記錄交易涉及的虛構的使用者名稱中任何記錄檔，讓您更輕鬆地解譯事件記錄檔。

- 停用 NAS 通知轉寄。 您可以停用轉送的開始和停止訊息從網路存取伺服器 (Nas) 的遠端 RADIUS 伺服器的成員群組在 NPS 中設定該 IS。 如需詳細資訊，請參閱 <<c0> [ 停用 NAS 通知轉寄](nps-disable-nas-notifications.md)。

如需詳細資訊，請參閱 <<c0> [ 設定網路原則伺服器 Accounting](nps-accounting-configure.md)。

- 若要使用 SQL Server 記錄提供容錯移轉和備援能力，將放在不同的子網路上執行 SQL Server 的兩部電腦。 使用 SQL Server**建立發行集精靈**設定兩部伺服器之間的資料庫複寫。 如需詳細資訊，請參閱 < [SQL Server 技術文件](https://msdn.microsoft.com/library/ms130214.aspx)並[SQL Server 複寫](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>驗證

以下是驗證的最佳作法。

- 使用憑證型驗證方法，例如受保護的可延伸驗證通訊協定\(PEAP\)和可延伸驗證通訊協定\(EAP\)增強式驗證。 請勿使用僅需密碼的驗證方法，因為它們是容易受到各式各樣的攻擊，而且不安全。 為安全的無線驗證，使用 PEAP\-MS\-MS-CHAP v2 建議，因為 NPS 給無線用戶端證明其身分識別，使用伺服器憑證，而使用者證明其身分識別與使用者名稱和密碼。  如需無線部署中使用 NPS 的詳細資訊，請參閱[部署密碼型 802.1x 驗證無線存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 部署您自己的憑證授權單位\(CA\)與 Active Directory&reg;憑證服務\(AD CS\)當您使用強式憑證為基礎的驗證方法，例如 PEAP 與 EAP 時，需要伺服器憑證 NPSs 上使用。 您也可以使用您的 CA 註冊電腦憑證和使用者憑證。 如需有關如何將伺服器憑證部署至 NPS 及遠端存取伺服器的詳細資訊，請參閱 <<c0> [ 部署 802.1x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="client-computer-configuration"></a>用戶端電腦設定

以下是用戶端電腦設定的最佳作法。

- 自動使用群組原則來設定所有的網域成員 802.1x 用戶端電腦。 如需詳細資訊，請參閱 「 設定無線網路 (IEEE 802.11) 原則 」 主題中的一節[無線存取部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="installation-suggestions"></a>安裝建議

以下是安裝 NPS 的最佳作法。

- 在安裝 NPS 之前, 安裝，並測試每個網路存取伺服器設定為 NPS 中的 RADIUS 用戶端之前，請使用本機的驗證方法。

- 在安裝和設定 NPS 之後，儲存設定使用 Windows PowerShell 命令[匯出 NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。 每次您重新設定 NPS 使用此命令儲存 NPS 設定。

>[!CAUTION]
>- 匯出的 NPS 的組態檔包含未加密共用的密碼 RADIUS 用戶端和遠端 RADIUS 伺服器群組的成員。 基於這個原因，請確定您將檔案儲存到安全的位置。
>- 匯出程序不在匯出的檔案包含 Microsoft SQL Server 記錄設定。 如果您將匯出的檔案匯入另一部 NPS，您必須手動設定 SQL Server 記錄新的伺服器上。

## <a name="performance-tuning-nps"></a>效能微調 NPS

以下是效能微調 NPS 的最佳作法。

- 若要最佳化 NPS 驗證和授權回應時間，並將網路流量降到最低，請在網域控制站上安裝 NPS。

- 當通用主體名稱\(Upn\)或使用 Windows Server 2008 和 Windows Server 2003 的網域，NPS 會使用通用類別目錄來驗證使用者。 若要執行這項操作所花費的時間降到最低，請在通用類別目錄伺服器或與通用類別目錄伺服器位於相同子網路上的伺服器上安裝 NPS。

- 當您擁有設定的遠端 RADIUS 伺服器群組，而且 NPS 連線要求原則，在您清除**記錄下列遠端 RADIUS 伺服器群組中的伺服器上的帳戶處理資訊**核取方塊，這些群組仍是傳送的網路存取伺服器\(NAS\)啟動和停止通知訊息。 這會建立不必要的網路流量。 若要排除這個流量，清除停用每個遠端 RADIUS 伺服器群組中的個別伺服器的 NAS 通知轉寄**轉送網路開始和停止通知到這個伺服器**核取方塊。

## <a name="using-nps-in-large-organizations"></a>在大型組織中使用 NPS

以下是在大型組織中使用 NPS 的最佳作法。

- 如果您使用網路原則來限制存取的所有但特定群組，就會建立所有您要允許存取時，使用者的萬用群組，然後再建立這個萬用群組的存取權的網路原則。 請勿將放所有的使用者直接將萬用群組中，尤其是如果您有大量的網路上。 相反地，建立屬於萬用群組中，並將使用者新增至這些群組的個別群組。

- 使用使用者主體名稱來參照使用者盡可能。 使用者可以擁有相同使用者主體名稱，而不論網域成員資格。 這項作法提供具有大量網域的組織中所需的延展性。

- 如果您安裝網路原則伺服器\(NPS\)網域以外的電腦上控制站與 NPS 伺服器正在接收大量每秒的驗證要求，您可以增加的數目，以改善 NPS 效能NPS 與網域控制站之間允許的同時驗證。 如需詳細資訊，請參閱本主題中的 

## <a name="security-issues"></a>安全性問題

以下是減少安全性問題的最佳作法。

當您從遠端管理 NPS 時，請不要透過網路以純文字傳送敏感或機密資料 （例如，共用的密碼或密碼）。 有兩種 NPSs 的遠端系統管理的建議的方法：

- 您可以使用遠端桌面服務來存取 NPS。 當您使用遠端桌面服務時，並不會將資料傳送用戶端與伺服器之間。 僅使用者介面 （例如，作業系統桌面和 NPS 主控台映像） 的伺服器會傳送至名為在 Windows 中的遠端桌面連線的遠端桌面服務用戶端&reg;10。 用戶端會傳送鍵盤和滑鼠輸入，其中已啟用遠端桌面服務的伺服器在本機處理。 遠端桌面服務使用者登入時，他們可以檢視只有其個別的用戶端工作階段，伺服器所管理而且彼此獨立。 此外，遠端桌面連線會提供用戶端與伺服器之間的 128 位元加密。

- 您可以使用網際網路通訊協定安全性 (IPsec) 來加密機密資料。 您可以使用 IPsec 來加密 NPS 與您用來管理 NPS 的遠端用戶端電腦之間的通訊。 若要從遠端管理伺服器，您可以安裝[遠端伺服器管理工具適用於 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)用戶端電腦上。 安裝完成後，請將 NPS 嵌入式管理單元到主控台中使用 Microsoft Management Console (MMC)。

>[!IMPORTANT]
>您可以只在 Windows 10 專業版或 Windows 10 企業版的完整版本上安裝遠端伺服器管理工具適用於 Windows 10。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。

