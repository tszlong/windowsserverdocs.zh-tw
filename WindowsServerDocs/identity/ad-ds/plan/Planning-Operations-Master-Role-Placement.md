---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: 規劃設置操作主機角色
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ad4e89be7eeb6190d27ee0e15e370bcaa1806cb8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959370"
---
# <a name="planning-operations-master-role-placement"></a>規劃設置操作主機角色

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services （AD DS）支援目錄資料的多宿主複寫，這表示任何網域控制站都可以接受目錄變更，並將變更複寫到所有其他網域控制站。 不過，某些變更（例如架構修改）並不實用，無法以多宿主方式執行。 基於此原因，某些網域控制站（稱為「操作主機」）會保留負責接受特定變更之要求的角色。

> [!NOTE]
> 操作主機角色持有者必須能將資訊寫入 Active Directory 資料庫。 由於唯讀網域控制站（RODC）上 Active Directory 資料庫的唯讀性質， **rodc 無法作為操作主機角色持有**者。

每個網域中都有三個操作主機角色（也稱為彈性單一主機操作或 FSMO）：

- 主域控制站（PDC）模擬器操作主機會處理所有密碼更新。

- 相對識別碼（RID）操作主機會維護網域的全域 RID 集區，並將本機 Rid 池配置給所有網域控制站，以確保在網域中建立的所有安全性主體都具有唯一識別碼。
- 指定網域的基礎結構操作主機會維護來自網域內群組成員之其他網域的安全性主體清單。

除了三個網域層級操作主機角色以外，每個樹系中都有兩個操作主機角色：

- 架構操作主機負責控制架構的變更。
- 網域命名操作主機會在樹系中新增和移除網域和其他目錄分割（例如網域名稱系統（DNS）應用程式磁碟分割）。

將裝載這些操作主機角色的網域控制站放在網路可靠性很高的區域中，並確保 PDC 模擬器和 RID 主機一致地可用。

建立特定網域中的第一個網域控制站時，會自動指派操作主機角色持有者。 這兩個樹系層級角色（架構主機和網網域命名主機）會指派給樹系中建立的第一個網域控制站。 此外，這三個網域層級角色（RID 主機、基礎結構主機和 PDC 模擬器）會指派給在網域中建立的第一個網域控制站。

> [!NOTE]
> 自動操作主機角色持有者指派只會在建立新的網域以及降級目前的角色持有者時進行。 角色擁有者的所有其他變更都必須由系統管理員起始。

這些自動操作主機角色指派可能會在樹系或網域中建立的第一個網域控制站上造成非常高的 CPU 使用量。 若要避免這種情況，請將（轉移）操作主機角色指派給樹系或網域中的各個網域控制站。 將主控操作主機角色的網域控制站放在網路可靠的區域，以及樹系中所有其他網域控制站可存取的操作主機。

您也應該為所有操作主機角色指定待命（替代）操作主機。 待命操作主機是網域控制站，您可以在原始角色持有者失敗時轉移操作主機角色。 確定待命操作主機是實際操作主機的直接複寫合作夥伴。

## <a name="planning-the-pdc-emulator-placement"></a>規劃 PDC 模擬器放置

PDC 模擬器會處理用戶端密碼變更。 只有一個網域控制站會作為樹系中每個網域的 PDC 模擬器。

即使所有網域控制站都已升級為 Windows 2000、Windows Server 2003 和 Windows Server 2008，而網域是在 Windows 2000 原生功能等級上運作，PDC 模擬器還是會收到網域中其他網域控制站所執行之密碼變更的優先複寫。 如果最近變更過密碼，該變更會花時間複寫到網域中的每個網域控制站。 如果由於密碼不正確而導致另一個網域控制站的登入驗證失敗，則該網域控制站會將驗證要求轉送至 PDC 模擬器，然後再決定是否要接受或拒絕登入嘗試。

如有需要，請將 PDC 模擬器放在包含來自該網域之大量使用者的位置，以進行密碼轉送作業。 此外，請確定該位置已妥善連接到其他位置，以將複寫延遲降到最低。

如需協助您記錄規劃放置 PDC 模擬器之位置的相關資訊，以及每個位置所代表每個網域的使用者數目，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟網域控制站放置（DSSTOPO_4.doc）。

當您部署地區網域時，您必須參考需要放置 PDC 模擬器之位置的相關資訊。 如需部署地區網域的詳細資訊，請參閱[部署 Windows Server 2008 地區網域](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10))。

## <a name="requirements-for-infrastructure-master-placement"></a>基礎結構主機位置的需求

基礎結構主機會從新增至自己網域中群組的其他網域，更新安全性主體的名稱。 例如，如果某個網域的使用者是第二個網域中群組的成員，而第一個網域中的使用者名稱變更，則第二個網域不會收到通知，指出使用者的名稱必須在群組的成員資格清單中更新。 因為一個網域中的網域控制站不會將安全性主體複寫到另一個網域中的網域控制站，所以第二個網域永遠不會察覺到沒有結構主機的變更。

基礎結構主機會持續監視群組成員資格，以尋找來自其他網域的安全性主體。 如果找到一個，它會檢查安全性主體的網域，以確認資訊是否已更新。 如果資訊已過期，則基礎結構主機會執行更新，然後將變更複寫到其網域中的其他網域控制站。

有兩個例外狀況適用于此規則。 首先，如果所有網域控制站都是通用類別目錄伺服器，裝載結構主機角色的網域控制站就很重要，因為通用類別目錄會複寫更新的資訊，而不論其所屬的網域為何。 第二，如果樹系只有一個網域，裝載結構主機角色的網域控制站就很重要，因為來自其他網域的安全性主體不存在。

請勿將基礎結構主機放在同時也是通用類別目錄伺服器的網域控制站上。 如果基礎結構主機和通用類別目錄位於相同的網域控制站上，則基礎結構主機將無法運作。 基礎結構主機永遠不會找到已過期的資料;因此，它永遠不會將任何變更複寫到網域中的其他網域控制站。

## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>連線的操作主機位置具有有限的連線能力

請注意，如果您的環境具有可放置操作主機角色持有者的中央位置或中樞網站，則相依于這些操作主機角色持有者之可用性的特定網域控制站作業可能會受到影響。

例如，假設組織會建立 A、B、C 和 D 等網站連結。介於 A 和 B 之間、B 和 C 之間，以及 C 和 D 之間。網路連線完全反映了網站連結的網路連線能力。 在此範例中，所有操作主機角色都會放在網站 A 中，而且不會選取 [**橋接所有網站連結**] 選項。

雖然此設定會導致所有網站之間的複寫成功，但操作主機角色功能有下列限制：

- 網站 C 和 D 中的網域控制站無法存取網站 A 中的 PDC 模擬器，以更新密碼或檢查是否有最近更新過的密碼。
- 網站 C 和 D 中的網域控制站無法存取網站 A 中的 RID 主機，以便在 Active Directory 安裝之後取得初始 RID 集區，並在它們被耗盡時重新整理 RID 集區。
- 網站 C 和 D 中的網域控制站無法新增或移除目錄、DNS 或自訂的應用程式磁碟分割。
- 網站 C 和 D 中的網域控制站無法進行架構變更。

如需協助您規劃操作主機角色位置的工作表，請參閱[Windows Server 2003 部署套件的工作輔助工具](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟網域控制站位置（DSSTOPO_4.doc）。

當您建立樹系根域和地區網域時，您將需要參考這則資訊。 如需部署樹系根域的詳細資訊，請參閱部署[部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10))。 如需部署地區網域的詳細資訊，請參閱[部署 Windows Server 2008 地區網域](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10))。

## <a name="next-steps"></a>接下來的步驟

如需有關 FSMO 角色位置的詳細資訊，請參閱[Active Directory 網域控制站上的 fsmo 位置和優化](https://support.microsoft.com/help/223346)支援主題
