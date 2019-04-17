---
title: 網路原則伺服器最佳做法
description: 本主題提供部署及管理 Windows Server 2016 中的網路原則伺服器的最佳做法。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>網路原則伺服器最佳做法

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解部署及管理的網路原則伺服器 \(NPS\) 最佳做法，您可以使用此主題。

下列章節提供最佳做法 NPS 部署的其他方面。

## <a name="accounting"></a>計量

以下是 NPS 登入的最佳做法的規範。

有兩種類型的計量，或在 NPS 登入：

- NPS 的事件登入。 您可以在 [系統及安全性事件登使用使用碼表進行 NPS 事件的事件登入。 這是主要用於稽核和連接嘗試進行疑難排解。

- 登入驗證使用者以及計量要求。 您可以登入的使用者來登入文字的格式或資料庫格式] 底下的 [檔案驗證及計量要求或您可以到儲存程序 SQL Server 2000 資料庫中登入。 要求登入主要用於連接分析及計費用途，並也適用於做為安全性調查工具，為您提供攻擊追蹤活動的方法。

若要使用最有效的 NPS 登入：

- 關閉 [在登入 \(initially\) 會計記錄和驗證。 修改這些選項之後在您判斷適合您的環境。

- 確定事件登入的容量不足以維護您登的設定。

- 因為他們無法重新建立損壞或刪除時備份定期登入的所有檔案。

- 使用 RADIUS 課程屬性追蹤使用和簡化的部門或使用者收取使用的驗證。 雖然唯一的每個要求自動的課程屬性，重複記錄可能存在於的案例位置的存取伺服器回覆遺失，重新傳送要求。 您可能需要從您以精確追蹤使用量登 delete 重複要求。

- 如果您的網路存取伺服器以及 RADIUS proxy 伺服器會定期傳送虛構連接要求訊息給 NPS 確認 online NPS 伺服器，請使用**ping 使用者名稱**登錄設定。 這項設定設定為自動不處理拒絕這些 false 連接要求 NPS。 此外，NPS 記錄交易涉及虛構中的使用者名稱任何登入檔案，讓事件登入變得更容易上尚未取得共識。

- 停用 NAS 通知轉接。 您可以停用的 [開始] 畫面與停止訊息從網路存取伺服器 (Nas) 遠端 RADIUS 伺服器成員群組該 IS NPS 中設定。 如需詳細資訊，請查看[停用 NAS 通知轉寄](nps-disable-nas-notifications.md)。

如需詳細資訊，請查看[設定的網路原則伺服器計量](nps-accounting-configure.md)。

- 若要提供錯誤移轉及冗餘 SQL Server 登入，放置執行 SQL Server 不同子網路上的兩部電腦。 使用 SQL Server**建立發行精靈**來設定資料庫複製兩部伺服器。 如需詳細資訊，請查看[SQL Server 技術文件](https://msdn.microsoft.com/library/ms130214.aspx)和[SQL Server 複寫](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>驗證

以下是進行驗證的最佳做法。

- 使用穩固驗證保護延伸驗證通訊協定 \(PEAP\) 和延伸驗證通訊協定 \(EAP\) 憑證為基礎的驗證方法。 請勿使用僅輸入密碼的驗證方法，因為它們可以很容易各種不同的攻擊，並不安全。 Wireless 安全驗證，使用 PEAP\-MS\-CHAP v2 建議，因為 NPS 伺服器使用者證明他們的身分使用者名稱與密碼時使用伺服器的憑證，以 wireless 戶端證明的身分。  如需有關如何使用 NPS wireless 部署的詳細資訊，請查看[架構部署密碼 802.1 X 驗證 Wireless 存取](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 部署自己憑證授權單位 \(CA\) Active Directory 使用&reg;當您使用穩固憑證式驗證方法、PEAP 和 EAP，例如，需要的伺服器上的憑證 NPS 伺服器使用憑證服務 \(AD CS\)。 您也可以使用您的 CA 憑證電腦和使用者憑證註冊。 如需有關伺服器的憑證部署至 NPS 及遠端存取伺服器，查看[適用於 802.1 X 的有線和無線部署部署伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="client-computer-configuration"></a>Client 電腦設定

以下是 client 電腦設定為最佳做法的規範。

- 自動使用群組原則設定您網域成員 802.1 X client 電腦的所有。 如需詳細資訊，請查看」設定無線 (IEEE 802.11) 的網路原則」主題中的區段[無線存取部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="installation-suggestions"></a>安裝建議

以下是安裝 NPS 的最佳做法。

- 安裝之前 NPS，安裝和使用本機的驗證方法將它們設定為在 NPS RADIUS 戶端之前您的網路存取伺服器的每個測試。

- 您安裝和設定 NPS 之後，將設定儲存使用 Windows PowerShell 命令[匯出-NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx)。 儲存 NPS 組態的每次您重新設定 NPS 伺服器這個命令。

>[!CAUTION]
>- 匯出的 NPS 設定檔包含加密共用的密碼 RADIUS 戶端和遠端 RADIUS 伺服器群組成員。 因此，請確定您將檔案儲存在安全的位置。
>- 匯出程序不包含匯出之檔案的 Microsoft SQL Server 的登入設定。 如果您匯入到另一個 NPS 伺服器匯出的檔案，您必須手動設定 SQL Server 登入新的伺服器上。

## <a name="performance-tuning-nps"></a>調整 NPS 效能

以下是效能調整 NPS 的最佳做法。

- 若要最佳化 NPS 驗證和授權回應時間和網路流量最小化，安裝 NPS 網域控制站。

- 萬用主體名稱 \(UPNs\) 或 Windows Server 2008 和 Windows Server 2003 網域使用時，NPS 使用通用驗證使用者。 最小化時所需執行此動作，請安裝 NPS 通用伺服器或相同通用伺服器子網路上的伺服器上。

- 當您有遠端設定 RADIUS 伺服器群組，NPS 連接要求原則，在您清除**在下列遠端 RADIUS 伺服器群組錄製計量伺服器上的資訊**核取方塊，這些群組仍會傳送網路存取伺服器 \(NAS\) 開始和停止訊息通知。 這會建立不必要的網路流量。 若要排除此流量，來停用 NAS 個人伺服器每個遠端 RADIUS 伺服器群組中的通知轉接清除**向前網路 [開始] 畫面與停止此伺服器通知**核取方塊。

## <a name="using-nps-in-large-organizations"></a>使用大型的組織 NPS

以下是使用大型的組織 NPS 最佳做法。

- 如果您正在使用的網路原則限制存取所有但某些群組，會建立萬用適用於所有使用者您要允許存取權限的群組，並建立授與此通用群組的存取權的網路原則。 不要將所有使用者的通用群組中，直接將尤其是當您有許多人在您的網路。 不過，建立通用群組中，並將使用者新增這些群組到不同群組。

- 使用參照使用者盡可能使用者主體名稱。 使用者可能無論網域成員資格相同的使用者主體名稱。 這個做法提供延展性，可能需要在組織中有大量的網域。

- 如果您的網域控制站以外的電腦上安裝的網路原則伺服器 \(NPS\) NPS 伺服器接收大量驗證要求秒，您可以增加允許 NPS 伺服器之間的網域控制站同時驗證的數目改善 NPS 效能。 如需詳細資訊，請查看 

## <a name="security-issues"></a>安全性問題

以下是最佳做法減少安全性問題。

當您的遠端管理 NPS 伺服器時，請不要純文字在網路上傳送敏感或機密資料（例如，共用的密碼或密碼）。 有兩個 NPS 伺服器的遠端管理建議的方法：

- 使用遠端桌面服務存取 NPS 伺服器。 當您使用遠端桌面服務時，伺服器 client 之間將不會傳送資料。 遠端桌面服務 client，稱為 windows 遠端桌面連接到傳送只使用者介面（例如，桌面作業系統和 NPS 主機映像）伺服器的&reg;10。 Client 傳送鍵盤和滑鼠輸入，這在本機伺服器遠端桌面服務功能的處理。 當遠端桌面服務使用者登入時，他們就可以檢視只他們個人 client 工作階段，這由伺服器，各自獨立。 此外，遠端桌面連接提供 client 之間伺服器 128 元加密。

- 用於機密資料加密網際網路通訊協定的安全性 (IPsec)。 您可以使用 IPsec 加密 NPS 伺服器和您使用管理 NPS client 遠端電腦之間的通訊。 若要從遠端管理的伺服器，您可以安裝[遠端伺服器管理工具適用於 Windows 10 的](https://www.microsoft.com/download/details.aspx?id=45520)上。 安裝之後，請使用 Microsoft Management Console (MMC) NPS 伺服器嵌入式管理單元新增至主機。

>[!IMPORTANT]
>您可以安裝遠端伺服器管理工具適用於 Windows 10 完整版本的 Windows 10 專業版或 Windows 10 企業版只在。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。

