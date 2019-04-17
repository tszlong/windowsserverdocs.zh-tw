---
title: 安裝和部署 Network Controller 準備需求
description: 準備您的資料中心部署 Network Controller 的您可以使用此主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>安裝和部署 Network Controller 準備需求

>適用於：Windows Server（以每年次管道）、Windows Server 2016

準備您的資料中心部署 Network Controller 的您可以使用此主題。  
  
> [!NOTE]  
> 本主題中，除了下列 Network Controller 的文件會提供。  
> 
> - [Network Controller](../technologies/network-controller/Network-Controller.md)
> - [網路控制器可用性](../technologies/network-controller/network-controller-high-availability.md)
> - [部署 Network Controller 使用 Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安裝使用伺服器管理員 Network Controller 伺服器角色](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

以下是您必須先部署 Network Controller 需要安裝、 軟體和其他需求和準備步驟。

## <a name="installation-requirements"></a>安裝需求

以下是 Network Controller 的安裝需求。

- 針對 Windows Server 2016 部署，您可以上一或多個電腦、 一或多個 Vm 或的電腦和 Vm 部署網路控制器。 所有 Vm 和為 Network Controller 節點計劃的電腦必須都執行 Windows Server 2016 Datacenter edition。

## <a name="software-requirements"></a>軟體需求

網路控制器部署需要一或多個電腦或做為一部電腦與網路控制器，請 Vm VM 做為 Network Controller 的管理 client。 這些電腦或 Vm 必須執行下列作業系統。  

- 任何電腦或一樣 (VM) 時，您可以安裝 Network Controller 必須執行 Windows Server 2016 Datacenter 版本。  
  
- 管理 client 電腦或網路控制器 VM 必須執行 Windows 8、 Windows 8.1 或 Windows 10。  
  
## <a name="additional-requirements"></a>額外需求

以下是您必須先部署 Network Controller 需要額外的步驟。
  
### <a name="configure-security-groups"></a>設定安全性群組
  
如果電腦或網路控制器和管理 client Vm 加入網域的設定下列安全性群組 Kerberos 驗證。

- 建立安全性群組並新增所有的使用者設定 Network Controller 的權限。 例如，建立名為群組**網路控制器管理員**。 所有的使用者，您將這個群組也必須成員**網域使用者**群組中 Active Directory 使用者電腦。  
  
    > [!NOTE]  
    > 如需有關 Active Directory 使用者電腦中建立群組，請查看[建立新的群組](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx)。  

- 建立安全性群組並新增所有的使用者設定及管理網路使用 Network Controller 的權限。  例如，建立新群組命名為**網路控制器使用者**。 您新增到新群組的使用者的所有也必須成員**網域使用者**群組中 Active Directory 使用者電腦。 使用代表狀態傳輸 \(REST\) 被執行所有 Network Controller 設定及管理。

### <a name="configure-log-file-locations-if-needed"></a>視需要設定檔登入的位置

您可以將網路控制器偵錯登 Network Controller 的電腦上 VM 或遠端檔案共用。 如果您想要儲存登入遠端檔案共用，請從網路控制器可以存取分享。

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>設定動態 DNS 登記 Network Controller
  
您可以將網路控制器叢集節點在相同的子網路或其他子網路上的部署。 

>[!NOTE]
>如果 Network Controller 節點上相同的子網路，您必須提供的網路控制器其餘 IP 位址，當您設定動態 DNS 登記 Network Controller 的。 如果節點上不同子網路，您必須提供的網路控制器其他 DNS 名稱，當您設定動態 DNS 登記。

如果網路控制器節點上不同子網路，您必須執行下列 DNS 設定：

- 部署程序期間建立 Network Controller 的 DNS 名稱

- 設定 DNS 伺服器上的網路控制器 DNS 名稱 DNS 動態更新

- 只 Network Controller 節點僅 DNS 動態更新

您可以使用下列程序，設定 DNS 動態更新，並只名稱記錄 Network Controller 的動態更新。

> [!NOTE]
> 資格在**網域系統管理員**，或相當於，才能執行這些程序最小值。
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>若要允許 DNS 區域的動態更新

1. 打開 DNS 管理員。

2. 主控台中適用的區域，以滑鼠右鍵按一下，然後按一下**屬性**。 這個區域的**屬性**對話方塊。

3. 在**一般**索引標籤上，確認區域類型 \**主要**或**Active Directory 整合**。

4. 在**的動態更新**，確認**只有安全**選取。 未選取，如果變更的值**動態更新**以**僅限安全性**，然後按一下 [ **[確定]**。

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>設定 DNS Network Controller 節點區域的安全性權限

1.  打開 DNS 管理員。

2.  主控台中適用的區域，以滑鼠右鍵按一下，然後按一下**屬性**。 這個區域的**屬性**對話方塊。

3.  按一下**安全性**索引標籤，然後按一下 [**進階]**。 **進階安全性設定]**對話方塊。

4. 在**進階安全性設定]**，按一下 [**新增]**。 **的權限的項目**對話方塊。
  
5. 按一下**請選取主體**。 **選擇使用者、 電腦、 或群組**對話方塊。

6. 在**選擇使用者、 電腦、 或群組**對話方塊中，按**物件類型**。 **物件類型**對話方塊。 

7. 在**物件類型**，請選取**電腦**，然後按一下 [ **[確定]**。

8. 在**選擇使用者、 電腦、 或群組**對話方塊中，輸入您的部署的其中一個節點 Network Controller NetBIOS 名稱，然後按一下 [ **[確定]**。

9. 在**的權限的項目**，確保的值**輸入**是**允許**的值與**適用於**是**這個物件和所有系物件**。
  
10. 在 [權限，選取 [**寫入所有屬性**和**Delete**，，然後按一下 [ **[確定]**。

11. 重複執行步驟**5**透過**10**的所有電腦和 Vm 中 Network Controller 叢集。

如需詳細資訊，請查看[計劃軟體定義網路基礎架構](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure)。
