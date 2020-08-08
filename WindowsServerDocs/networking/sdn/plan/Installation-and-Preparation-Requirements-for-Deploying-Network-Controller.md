---
title: 部署網路控制站的需求
description: 準備您的資料中心以進行網路控制站部署，這需要一或多部電腦或 vm，以及一部電腦或 VM。 在您可以部署網路控制站之前，您必須先設定安全性群組、記錄檔位置 (視需要) 和動態 DNS 註冊。
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: 051518873bd028e8b1253b9bf7cb17dcff344d0d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996600"
---
# <a name="requirements-for-deploying-network-controller"></a>部署網路控制站的需求

>適用於：Windows Server (半年度管道)、Windows Server 2016

準備您的資料中心以進行網路控制站部署，這需要一或多部電腦或 vm，以及一部電腦或 VM。 在您可以部署網路控制站之前，您必須先設定安全性群組、記錄檔位置 (視需要) 和動態 DNS 註冊。


## <a name="network-controller-requirements"></a>網路控制站需求

網路控制站部署需要一或多部做為網路控制站的電腦或 Vm，以及一部電腦或 VM 作為網路控制站的管理用戶端。

- 規劃為網路控制卡節點的所有 Vm 和電腦都必須執行 Windows Server 2016 Datacenter edition。
- 任何安裝網路控制卡的電腦或虛擬機器 (VM) 都必須執行 Windows Server 2016 Datacenter edition。
- 網路控制卡的管理用戶端電腦或 VM 必須執行 Windows 10。


## <a name="configuration-requirements"></a>組態需求

部署網路控制站之前，您必須先設定安全性群組、記錄檔位置 (視需要) 和動態 DNS 註冊。

### <a name="step-1-configure-your-security-groups"></a>步驟 1： 設定安全性群組

您想要做的第一件事是建立兩個安全性群組來進行 Kerberos 驗證。

您可以為具有下列許可權的使用者建立群組：

1. 設定網路控制站<p>例如，您可以將此群組命名為網路控制站系統管理員。
2.  使用網路控制卡設定和管理網路<p>例如，您可以將此群組命名為網路控制卡使用者。 使用具像狀態傳輸 (REST) 設定和管理網路控制卡。

>[!NOTE]
>您新增的所有使用者都必須是 Active Directory [使用者和電腦] 中的 [網域使用者] 群組的成員。

### <a name="step-2-configure-log-file-locations-if-needed"></a>步驟 2： 視需要設定記錄檔位置

您要做的下一件事，就是將檔案位置設定為在網路控制站電腦或 VM 上，或是在遠端檔案共用上儲存網路控制站的 debug 記錄檔。

>[!NOTE]
>如果您將記錄儲存在遠端檔案共用中，請確定可以從網路控制站存取該共用。


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>步驟 3： 設定網路控制站的動態 DNS 註冊

最後，您要做的下一件事，就是在相同的子網或不同的子網上部署網路控制站叢集節點。


|         如果...         |                                                                                                                                                         則...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  在相同的子網上，  |                                                                                                                                您必須提供網路控制卡的 REST IP 位址。                                                                                                                                 |
| 在不同的子網上， | 您必須提供在部署過程中建立的網路控制站 REST DNS 名稱。 您也必須執行下列動作：<ul><li>在 DNS 伺服器上設定網路控制站 DNS 名稱的 DNS 動態更新。</li><li>僅限制網路控制站節點的 DNS 動態更新。</li></ul> |

---

> [!NOTE]
> 若要執行這些程式，至少需要**Domain Admins**的成員資格或同等許可權。

1. 允許區域的 DNS 動態更新。

   a. 開啟 [DNS 管理員]，然後在主控台樹中，以滑鼠右鍵按一下適用的區域，**然後按一下 [** 內容]。

   b. 在 [**一般**] 索引標籤上，確認區欄位型別為 [**主要**] 或 [ **Active Directory 整合**]。

   c. 在 [**動態更新**] 中，確認已選取 [**僅安全**]，然後按一下 **[確定]**。

2. 設定網路控制站節點的 DNS 區域安全性許可權

   a.  按一下 [安全性]**** 索引標籤，然後按一下 [進階]****。

   b. 在 [**高級安全性設定**] 中，按一下 [**新增**]。

   c. 按一下 [選取主體]****。

   d. 在 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊中，按一下 [**物件類型**]。

   e. 在 [**物件類型**] 中，選取 [**電腦**]，然後按一下 **[確定]**。

   f. 在 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊中，輸入您的部署中其中一個網路控制卡節點的 NetBIOS 名稱，然後按一下 **[確定]**。

   g. 在 [**許可權專案**] 中，確認下列值：

      - **類型**= 允許
      - **適用于**= 此物件和所有子系物件

   h. 在 [**許可權**] 中，選取 [**寫入所有屬性**] 和 [**刪除**]，然後按一下 **[確定]**。

3. 針對網路控制卡叢集中的所有電腦和 Vm 重複此動作。

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>步驟 4： 設定服務主體名稱（如果使用 Kerberos 型驗證）

如果網路控制卡使用 Kerberos 驗證來與管理用戶端通訊，您必須為 Active Directory 中的網路控制站) 設定 (SPN 的服務主體名稱。 網路控制站會自動設定 SPN。 您只需要提供網路控制站電腦的許可權，就可以註冊和修改 SPN。 如需詳細資訊，請參閱[ (SPN) 設定服務主體名稱](../security/kerberos-with-spn.md#configure-service-principal-names-spn)。

## <a name="deployment-options"></a>部署選項

### <a name="network-controller-deployment"></a>網路控制站部署

此設定在虛擬機器上設定了三個網路控制站節點，具備高可用性。 同時也顯示兩個租使用者2的虛擬網路分成兩個虛擬子網，以模擬 web 層和資料庫層。

![SDN NC 規劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>網路控制卡和軟體負載平衡器部署

為了達到高可用性，有兩個或多個 SLB/MUX 節點。

![SDN NC 規劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>網路控制站、軟體 Load Balancer 和 RAS 閘道部署

有三個閘道虛擬機器;兩個作用中，一個是多餘的。

![SDN NC 規劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)

>[!IMPORTANT]
>如果您使用 VMM 進行部署，請確定您的基礎結構虛擬機器 (VMM 伺服器、AD/DNS、SQL Server ) 等，而不是裝載于圖表中所示的四部主機上。


## <a name="next-steps"></a>後續步驟
[規劃軟體定義的網路基礎結構](./plan-a-software-defined-network-infrastructure.md)。

## <a name="related-topics"></a>相關主題
- [網路控制卡](../technologies/network-controller/Network-Controller.md)
- [網路控制站高可用性](../technologies/network-controller/network-controller-high-availability.md)
- [使用 Windows PowerShell 部署網路控制卡](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
- [使用伺服器管理員安裝網路控制卡伺服器角色](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)