---
title: 轉換在儲存體遷移服務中的運作方式
description: 儲存體遷移服務中切換階段的摘要和詳細資料
author: t-chrche
ms.author: t-chrche
manager: nedpyle
ms.date: 08/31/2020
ms.topic: article
ms.openlocfilehash: 1e886c505435976b6495e0460705821086781a62
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766901"
---
# <a name="how-cutover-works-in-storage-migration-service"></a>轉換在儲存體遷移服務中的運作方式

切換是將來源電腦的網路識別移動到目的地電腦的遷移階段。 轉換之後，來源電腦仍會包含與之前相同的檔案，但不會提供給使用者和應用程式使用。

## <a name="summary"></a>摘要

![轉換設定螢幕擷取畫面 ](media/cutover/cutover_configuration.png)
 __圖1：儲存體遷移服務__轉換設定

在轉換開始之前，您會提供從來源電腦切換至目的地電腦所需的網路設定資訊。 您也可以為來源電腦選擇新的唯一名稱，或讓儲存體遷移服務建立一個隨機名稱。

然後，儲存體遷移服務會採取下列步驟，將來源電腦切換到目的地電腦：

1. 我們會連接到來源和目的地電腦。 它們應該都已啟用下列防火牆規則：
    *  (SMB) 的檔案和印表機共用，TCP 埠445
    * Netlogon 服務 (NP) ，TCP 埠445
    * Windows Management Instrumentation (DCOM) 的 TCP 埠135
    * Windows Management Instrumentation (WMI) 、TCP、任何埠

2. 我們在 Active Directory Domain Services 的目的地電腦上設定安全性許可權，以符合來源電腦的許可權。

3. 我們會在來源電腦上建立暫時的本機使用者帳戶。 如果電腦已加入網域，則帳戶使用者名稱是 "MsftSmsStorMigratSvc"。 我們會停用來源電腦上的 [本機帳戶權杖篩選原則](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows) ，以允許帳戶通過，然後連接到來源電腦。 我們會建立此暫時帳戶，以便在稍後重新開機並從網域移除來源電腦時，仍可存取來源電腦。

4. 我們會在目的地電腦上重複先前的步驟。

5. 我們會將來源電腦從網域中移除，以釋出其 Active Directory 的帳戶，而目的地電腦稍後將接管此帳戶。

6. 我們會對應來源電腦上的網路介面，並重新命名來源電腦。

7. 我們會將來源電腦新增回網域。 來源電腦現在有新的身分識別，可供系統管理員使用，但不適用於使用者和應用程式。

8. 在來源電腦上，我們會移除任何延遲的替代電腦名稱稱，移除我們建立的暫存本機帳戶，然後重新啟用本機帳戶權杖篩選原則。

9. 我們會將目的地電腦從網域中移除。

10. 我們會將目的地電腦上的 IP 位址取代為來源所提供的 IP 資訊，然後將目的地電腦重新命名為來源電腦的原始名稱。

11. 我們將目的地電腦聯結回網域。 加入時，會使用來源電腦的原始 Active Directory 電腦帳戶。 這會保留群組成員資格和安全性 Acl。 目的地電腦現在具有來源電腦的身分識別。

12. 在目的地電腦上，我們會移除任何延遲的替代電腦名稱稱，移除我們建立的暫存本機帳戶，然後重新啟用本機帳戶權杖篩選原則，完成轉換。

在轉換完成之後，目的地電腦已取得來源電腦的身分識別，然後您就可以解除委任來源電腦。

## <a name="detailed-stages"></a>詳細階段

![切換階段描述螢幕擷取畫面 ](media/cutover/cutover_stage_description.png)
 __圖2：顯示轉換階段描述的儲存體遷移服務__

您可以透過如下圖所示的每個階段說明來追蹤轉換進度。 下表顯示每個可能的階段，以及其進度、描述和任何闡明的附注。

|  進度 | 描述                                                                                               |  注意 |
|:-----|:--------------------------------------------------------------------------------------------------------------------|:---|
|  0% | 轉換閒置中。 |   |
| 2%  | 正在連接到來源電腦 .。。 |   請確定 [來源和目的地電腦的需求](./overview.md#security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports) 都已完成。|
| 5%  | 正在連接到目的地電腦 .。。 |   |
| 6%  | 正在 Active Directory 中設定電腦物件的安全性許可權 .。。 |   在目的地電腦上複寫來源電腦的 Active Directory 物件安全性許可權。|
| 8%  | 確定已成功在來源電腦上刪除我們建立的暫存帳戶 .。。 |   確定我們可以建立具有相同名稱的暫時帳戶。|
| 11% | 正在來源電腦上建立暫存本機使用者帳戶 .。。 |   如果來源電腦已加入網域，則暫時帳戶的使用者名稱是 "MsftSmsStorMigratSvc"。 密碼包含127個隨機的 unicode 寬字元，其中包含字母、數位、符號和大小寫變更。 如果來源電腦在工作組中，我們會使用原始來源認證。|
| 13% | 正在設定來源電腦上的本機帳戶權杖篩選原則 .。。 |   停用原則，讓我們可以在未加入網域時連接到來源。 請在 [這裡](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows)深入瞭解本機帳戶權杖篩選原則。|
| 16% | 使用暫存本機使用者帳戶連接到來源電腦 .。。 |   |
| 19% | 確定已在目的地電腦上成功刪除我們建立的暫存帳戶 .。。 |   |
| 22% | 正在目的地電腦上建立暫存本機使用者帳戶 .。。 | 如果目的地電腦已加入網域，則暫時帳戶的使用者名稱是 "MsftSmsStorMigratSvc"。 密碼包含127個隨機的 unicode 寬字元，其中包含字母、數位、符號和大小寫變更。 如果目的地電腦位於工作組中，我們會使用原始目的地認證。 |
| 25% | 正在設定目的地電腦上的本機帳戶權杖篩選原則 .。。 | 停用原則，讓我們可以在未加入網域時連接到目的地。 請在 [這裡](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows)深入瞭解本機帳戶權杖篩選原則。   |
| 27% | 使用暫存本機使用者帳戶連接到目的地電腦 .。。 |   |
| 30% | 正在從網域移除來源電腦 .。。 |   |
| 之間 | 收集來源電腦 IP 位址。 |   僅適用于 Linux 來源電腦。 |
| 33% | 正在重新開機來源電腦 ... (1 次重新開機)  |   |
| 36% | 正在等候來源電腦在第一次重新開機後回應 .。。 |   如果來源電腦未涵蓋在 DHCP 子網中，但您在網路設定期間選取了 DHCP，則可能會變成沒有回應。|
| 38% | 對應來源電腦上的網路介面 .。。 |   |
| 41% | 正在重新命名來源電腦 .。。 |   |
| 42% | 正在重新開機來源電腦 ... (1 次重新開機)  |   僅適用于 Linux 來源電腦。|
| 43% | 正在重新開機來源電腦 ... (第2次重新開機)  |   僅適用于已加入網域的 Windows Server 2003 來源電腦。|
| 43% | 正在等候來源電腦在第一次重新開機後回應 .。。 |   |
| 43% | 等候來源電腦在第2次重新開機之後回應 .。。 |   |
| 44% | 正在將來源電腦新增至網域 .。。 |   |
| 47% | 正在重新開機來源電腦 ... (1 次重新開機)  |   |
| 50% | 正在重新開機來源電腦 ... (第2次重新開機)  |   |
| 51% | 正在重新開機來源電腦 ... (第三個重新開機)  |   只適用于 Windows Server 2003 來源電腦。|
| 52% | 正在等候來源電腦回應 .。。 |   |
| 52% | 正在等候來源電腦在第一次重新開機後回應 .。。 |   |
| 55% | 等候來源電腦在第2次重新開機之後回應 .。。 |   |
| 56% | 正在等候來源電腦在第三個重新開機後回應 .。。 |   |
| 57% | 正在移除來源上的替代電腦名稱稱 .。。 |   確定來源無法與其他使用者和應用程式連線。 如需詳細資訊，請參閱 [Netdom computername](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc835082(v=ws.11))。 |
| 58% | 正在移除我們在來源電腦上建立的暫存本機帳戶 .。。 |   |
| 61% | 正在重設來源電腦上的本機帳戶權杖篩選原則 .。。 |   啟用原則。|
| 63% | 正在從網域移除目的地電腦 .。。 |   |
| 66% | 正在重新開機目的地電腦 ... (1 次重新開機)  |   |
| 69% | 在第一次重新開機之後等候目的地電腦回應 .。。 |   |
| 72% | 對應目的地電腦上的網路介面 .。。 |  將來源電腦的每個網路介面卡和 IP 位址對應到目的地電腦，並取代目的地的網路資訊。   |
| 75% | 正在重新命名目的地電腦 .。。 |   |
| 77% | 正在將目的地電腦新增至網域 .。。 |  目的地電腦會接管舊的來源電腦的 Active Directory 物件。 如果目的地使用者不是 Domain Admins 的成員，或沒有來源電腦的系統管理員許可權 Active Directory 物件，則這可能會失敗。 您可以在轉換開始之前，在「輸入認證」步驟中指定替代目的地認證。|
| 80% | 正在重新開機目的地電腦 ... (1 次重新開機)  |   |
| 83% | 正在重新開機目的地電腦 ... (第2次重新開機)  |   |
| 84% | 正在等候目的地電腦回應 .。。 |   |
| 86% | 在第一次重新開機之後等候目的地電腦回應 .。。 |   |
| 88% | 等候目的地電腦在第2次重新開機之後回應 .。。 |   |
| 91% | 正在等候目的地電腦以新名稱回應 .。。 |  可能需要很長的時間，因為 Active Directory 和 DNS 複寫。 |
| 93% | 正在移除目的地上的替代電腦名稱稱 .。。 |   確認目的名稱已被取代。|
| 94% | 正在移除我們在目的地電腦上建立的暫存本機帳戶 .。。|   |
| 97% | 正在重設目的地電腦上的本機帳戶權杖篩選原則 .。。 |   啟用原則。|
|  (100% )  | 成功 |   |

## <a name="faq"></a>常見問題集

### <a name="__is-domain-controller-migration-supported__"></a>__是否支援網域控制站遷移？__

目前不是，但請參閱 [常見問題頁面](./faq.md#is-domain-controller-migration-supported) 以取得解決方法。


## <a name="known-issues"></a>已知問題
>確定您已完成 [儲存體遷移服務](overview.md) 的需求，並已在執行儲存體遷移服務的電腦上安裝最新的 Windows 更新。

如需有關下列問題的詳細資訊，請參閱 [ [已知問題] 頁面](./known-issues.md) 。
* [__儲存體遷移服務轉換驗證失敗，並出現「目的地電腦上的權杖篩選原則拒絕存取」錯誤__](./known-issues.md#storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer)

* [__「針對網路服務資源 CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO 失敗」錯誤，且 Windows Server 2008 R2 叢集轉換失敗__](./known-issues.md#error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails)

* [__切換停止回應來源電腦上的「38% 對應網路介面 ...」使用靜態 Ip 時__](./known-issues.md#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-static-ips)

* [__切換停止回應來源電腦上的「38% 對應網路介面 ...」__](./known-issues.md#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer)

## <a name="additional-references"></a>其他參考資料

- [儲存體遷移服務總覽](overview.md)
- [使用儲存體遷移服務遷移檔案伺服器](migrate-data.md)
- [儲存體遷移服務的常見問題 (常見問題) ](faq.md)
- [儲存體遷移服務的已知問題](known-issues.md)