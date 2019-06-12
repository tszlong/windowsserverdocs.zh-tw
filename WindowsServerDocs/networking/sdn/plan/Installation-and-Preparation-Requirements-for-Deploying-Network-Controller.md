---
title: 部署網路控制站需求
description: 準備您的資料中心網路控制卡部署，需要一或多個電腦或 Vm 和一部電腦或 VM。 您可以部署網路控制站之前，您必須設定安全性群組、 記錄檔位置 （如有需要），以及動態 DNS 註冊。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 51ba991397a7c35ee0198f8e75c67b2f99b7c7bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446314"
---
# <a name="requirements-for-deploying-network-controller"></a>部署網路控制站需求

>適用於：Windows Server （半年通道），Windows Server 2016

準備您的資料中心網路控制卡部署，需要一或多個電腦或 Vm 和一部電腦或 VM。 您可以部署網路控制站之前，您必須設定安全性群組、 記錄檔位置 （如有需要），以及動態 DNS 註冊。


## <a name="network-controller-requirements"></a>網路控制站的需求

網路控制卡部署需要一或多個電腦或 Vm 做為網路控制站，以及一部電腦或 VM 做為網路控制器管理用戶端。 

- 所有 Vm 和計劃做為網路控制卡節點的電腦必須都執行 Windows Server 2016 Datacenter edition。 
- 任何電腦或虛擬機器 (VM) 的安裝網路控制站必須執行 Windows Server 2016 Datacenter edition。 
- 管理用戶端電腦或網路控制站的 VM 必須執行 Windows 10。 


## <a name="configuration-requirements"></a>設定需求

在部署網路控制站，您必須設定安全性群組、 記錄檔位置 （如有需要），以及動態 DNS 註冊。

### <a name="step-1-configure-your-security-groups"></a>步驟 1. 設定您的安全性群組

您想要執行的第一件事是建立兩個安全性群組的 Kerberos 驗證。 

您建立群組的權限的使用者來： 

1. 設定網路控制站<p>您可以群組命名為這個網路控制站的系統管理員，例如。 
2.  設定和使用網路控制卡管理網路<p>您可以命名此群組的網域控制站使用者，例如。 若要設定及管理網路控制站使用具像狀態傳輸 (REST)。

>[!NOTE]
>所有您加入的使用者必須在 Active Directory 使用者和電腦的 Domain Users 群組的成員。

### <a name="step-2-configure-log-file-locations-if-needed"></a>步驟 2. 如有需要設定記錄檔位置

您想要執行下一的步是設定來儲存網路控制卡偵錯記錄檔，網路控制站的電腦或 VM 上或遠端檔案共用上的檔案位置。 

>[!NOTE]
>如果您在遠端檔案共用中儲存的記錄檔，請從網路控制站可以存取共用。


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>步驟 3。 網路控制站設定動態 DNS 註冊

最後，您想要執行下一的步是部署網路控制站相同的子網路或不同的子網路上的叢集節點。 


|         如果...         |                                                                                                                                                         則...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  位於相同的子網路，  |                                                                                                                                您必須提供網路控制站的 REST IP 位址。                                                                                                                                 |
| 位於不同的子網路 | 您必須提供您在部署程序期間建立的網路控制站的 REST DNS 名稱。 您也必須執行下列動作：<ul><li>設定網路控制站的 DNS 名稱的 DNS 動態更新 DNS 伺服器上。</li><li>限制只有網路控制卡節點 DNS 動態更新。</li></ul> |

---

> [!NOTE]
> 中的成員資格**Domain Admins**，或同等權限，才能執行這些程序的最小值。

1. 允許區域的 DNS 動態更新。

   a. 開啟 DNS 管理員，並在主控台樹狀目錄中，以滑鼠右鍵按一下適用的區域，然後按**屬性**。 

   b. 在 **一般**索引標籤上，確認區域類型是否為**主要**或是**Active Directory 整合**。

   c. 在 **動態更新**，確認**只有安全**已選取，然後按一下**確定**。

2. 設定網路控制卡節點的 DNS 區域安全性權限

   a.  按一下 [安全性]  索引標籤，然後按一下 [進階]  。 

   b. 在 **進階安全性設定**，按一下**新增**。 

   c. 按一下 [**選取一個主體**]。 

   d. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，按一下**物件類型**。 

   e. 在 **物件類型**，選取**電腦**，然後按一下**確定**。

   f. 在 [**選取使用者、 電腦、 服務帳戶或群組**] 對話方塊中，在您部署中，輸入其中一個網路控制卡節點的 NetBIOS 名稱，然後按一下**確定**。

   g. 在 **權限項目**，確認下列值：

      - **型別**= 允許
      - **適用於**= 此物件及所有子系物件

   h. 中**權限**，選取**寫入全部內容**並**刪除**，然後按一下 **[確定]** 。

3. 在網路控制卡叢集中重複的所有電腦和 Vm。

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>步驟 4. 設定服務主體名稱，如果使用 Kerberos 式驗證

如果網路控制站與管理用戶端通訊使用 Kerberos 驗證，您必須設定 Active Directory 中的網路控制站的服務主體名稱 (SPN)。 網路控制站會自動設定 SPN。 您只需要為提供網路控制站機器註冊，並修改 SPN 的權限。 如需詳細資訊，請參閱 <<c0> [ 設定服務主體名稱 (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn)。

## <a name="deployment-options"></a>部署選項

### <a name="network-controller-deployment"></a>網路控制站部署

安裝程式包含三個虛擬機器上設定的網路控制卡節點是高可用性的。 也會顯示為兩個的租用戶與租用戶 2 的虛擬網路分成兩個虛擬子網路，以模擬 web 層和資料庫層。  

![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>網路控制卡和軟體負載平衡器部署

對於高可用性，有兩個或多個 SLB/MUX 節點。

![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>網路控制站、 軟體負載平衡器，以及 RAS 閘道部署

有三部閘道虛擬機器;兩個作用中，並有備援。

![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  



TP5 為基礎的部署自動化、 可用且可從這些子網路，就必須是一個 Active Directory。 如需有關 Active Directory 的詳細資訊，請參閱 < [Active Directory 網域服務概觀](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)。  

>[!IMPORTANT] 
>如果您使用 VMM 部署，請確定您的基礎結構虛擬機器 (VMM 伺服器 AD/DNS，SQL Server，等等) 不裝載在任何圖表所示的四個主機上。  


## <a name="next-steps"></a>後續步驟
[軟體定義網路基礎結構的計劃](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure)。

## <a name="related-topics"></a>相關主題
- [網路控制卡](../technologies/network-controller/Network-Controller.md) 
- [網路控制站的高可用性](../technologies/network-controller/network-controller-high-availability.md) 
- [使用 Windows PowerShell 部署網路控制卡](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [使用伺服器管理員安裝網路控制卡伺服器角色](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
