---
title: Kerberos 搭配服務主體名稱 (SPN)
description: 網路控制站支援多個驗證方法與管理用戶端進行通訊。 您可以使用 Kerberos 式驗證，X509 憑證型驗證。 您也可以針對測試部署中使用任何驗證。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828969"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos 搭配服務主體名稱 (SPN)

>適用於：Windows Server 2019

網路控制站支援多個驗證方法與管理用戶端進行通訊。 您可以使用 Kerberos 式驗證，X509 憑證型驗證。 您也可以針對測試部署中使用任何驗證。

System Center Virtual Machine Manager 會使用 Kerberos 驗證。 如果您使用 Kerberos 驗證，您必須設定 Active Directory 中的網路控制站的服務主體名稱 (SPN)。 SPN 是網路控制卡服務執行個體，會使用 Kerberos 驗證服務登入帳戶相關聯的服務執行個體的唯一識別碼。 如需詳細資訊，請參閱 <<c0> [ 服務主體名稱](https://docs.microsoft.com/windows/desktop/ad/service-principal-names)。

## <a name="configure-service-principal-names-spn"></a>設定服務主要名稱 (SPN)

網路控制站會自動設定 SPN。 您只需要為提供網路控制站機器註冊，並修改 SPN 的權限。

1.  網域控制站電腦上，啟動**Active Directory 使用者和電腦**。

2.  選取 **檢視\>進階**。

3.  底下**電腦**，找出其中一個網路控制站電腦帳戶，然後以滑鼠右鍵按一下並選取**屬性**。

4.  選取 **安全性**索引標籤，然後按一下**進階**。

5.  在清單中，如果所有網路控制站電腦帳戶或安全性群組具有未列出的所有網路控制站電腦帳戶，請按一下**新增**將它加入。

6.  針對每個網路控制站電腦帳戶或單一安全性群組，其中包含網路控制站電腦帳戶：

    a.  選取的帳戶或群組，然後按一下**編輯**。

    b.  [選取權限] 底下**驗證撰寫 servicePrincipalName**。

    d.  向下的捲動底下**屬性**選取：

       -  **讀取 servicePrincipalName**

       -  **撰寫 servicePrincipalName**

    e.  按兩次 **[確定]** 。

7.  針對每個網路控制站機器重複步驟 3-6。

8.  關閉 [Active Directory 使用者和電腦] 。

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>無法提供的 SPN 註冊/修改權限

在 **新增**Windows Server 2019 部署，如果您選擇的 REST 用戶端驗證的 Kerberos 並不授與網路控制卡節點註冊，或修改之 SPN 的權限，在網路控制站上的 REST 作業失敗允許您管理 SDN。

針對從 Windows Server 2016 升級至 Windows Server 2019，而且您選擇 REST 用戶端驗證 Kerberos，REST 作業，不會封鎖，確保透明度，現有的生產環境部署。 

如果未註冊 SPN，REST 用戶端驗證會使用 NTLM，這是較不安全。 您也會管理通道中取得的重大事件**NetworkController Framework**事件通道，要求您提供網路控制卡節點註冊 SPN 的權限。 一旦您提供權限，網路控制站自動註冊的 SPN，而且用戶端的所有作業都使用 Kerberos。


>[!TIP]
>一般而言，您可以設定網路控制站，以使用 IP 位址或 DNS 名稱以 REST 為基礎的作業。 不過，當您設定 Kerberos，您無法使用 IP 位址的網路控制站的 REST 查詢。 例如，您可以使用\< https://networkcontroller.consotso.com\>，但是您無法使用\< https://192.34.21.3\>。 服務主體名稱不能函式，如果使用 IP 位址。
>
>如果您使用 IP 位址的 REST 作業，以及 Windows Server 2016 中的 Kerberos 驗證，實際的通訊就已透過 NTLM 驗證。 在這類部署中，一旦您升級至 Windows Server 2019，您繼續使用 NTLM 驗證。 若要移動以 Kerberos 為基礎的驗證，您必須使用 REST 作業的網路控制站的 DNS 名稱，並提供網路控制卡節點註冊 SPN 的權限。

---