---
title: Kerberos 搭配服務主體名稱 (SPN)
description: 網路控制卡支援多種驗證方法來與管理用戶端通訊。 您可以使用以 Kerberos 為基礎的驗證，以 X509 憑證為基礎的驗證。 您也可以選擇不使用測試部署的驗證。
manager: grcusanz
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 3937f124ba91a597af83c00cd5497ea57c1b2fed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962132"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos 搭配服務主體名稱 (SPN)

>適用於：Windows Server 2019

網路控制卡支援多種驗證方法來與管理用戶端通訊。 您可以使用以 Kerberos 為基礎的驗證，以 X509 憑證為基礎的驗證。 您也可以選擇不使用測試部署的驗證。

System Center Virtual Machine Manager 使用以 Kerberos 為基礎的驗證。 如果您使用以 Kerberos 為基礎的驗證，您必須為 Active Directory 中的網路控制站) 設定 (SPN 的服務主體名稱。 SPN 是網路控制站服務實例的唯一識別碼，由 Kerberos 驗證用來將服務實例與服務登入帳戶建立關聯。 如需詳細資訊，請參閱[服務主體名稱](https://docs.microsoft.com/windows/desktop/ad/service-principal-names)。

## <a name="configure-service-principal-names-spn"></a> (SPN) 設定服務主體名稱

網路控制站會自動設定 SPN。 您只需要提供網路控制站電腦的許可權，就可以註冊和修改 SPN。

1.  在網域控制站電腦上，啟動**Active Directory 使用者和電腦**]。

2.  選取 [ **View \> Advanced**]。

3.  在 [**電腦**] 下，找出其中一個網路控制站電腦帳戶，然後按一下滑鼠右鍵**並選取 [** 內容]。

4.  選取 [安全性] **** 索引標籤，然後按一下 [進階] ****。

5.  在清單中，如果沒有列出所有網路控制站電腦帳戶或具有所有網路控制卡機器帳戶的安全性群組，請按一下 [**新增**] 將它新增。

6.  針對每個網路控制站電腦帳戶或包含網路控制卡電腦帳戶的單一安全性群組：

    a.  選取帳戶或群組，然後按一下 [**編輯**]。

    b.  在 [許可權] 底下，選取 [**驗證寫入 servicePrincipalName**]。

    d.  在 [**屬性**] 下向下選取：

       -  **閱讀 servicePrincipalName**

       -  **寫入 servicePrincipalName**

    e.  按兩次 **[確定]**。

7.  針對每部網路控制站電腦重複步驟 3-6。

8.  關閉 [Active Directory 使用者和電腦]****。

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>無法提供 SPN 註冊/修改的許可權

在**新**的 Windows Server 2019 部署中，如果您選擇使用 KERBEROS 進行 REST 用戶端驗證，但未授與網路控制卡節點註冊或修改 SPN 的許可權，則網路控制卡上的 REST 作業會失敗，讓您無法管理 SDN。

若要從 Windows Server 2016 升級至 Windows Server 2019，而且您選擇使用 Kerberos 進行 REST 用戶端驗證，則不會封鎖 REST 作業，以確保現有生產部署的透明度。

如果 SPN 未註冊，REST 用戶端驗證會使用較不安全的 NTLM。 您也會在**NetworkController 架構**事件通道的系統管理員通道中取得重大事件，要求您提供許可權給網路控制站節點來註冊 SPN。 當您提供許可權後，網路控制卡會自動註冊 SPN，而所有用戶端作業會使用 Kerberos。


>[!TIP]
>一般來說，您可以將網路控制站設定為使用 IP 位址或 DNS 名稱進行以 REST 為基礎的作業。 不過，當您設定 Kerberos 時，您無法使用 IP 位址來將 REST 查詢用於網路控制站。 例如，您可以使用 \<https://networkcontroller.consotso.com\> ，但無法使用 \<https://192.34.21.3\> 。 如果使用 IP 位址，服務主體名稱就無法運作。
>
>如果您使用 IP 位址進行 REST 作業，以及 Windows Server 2016 中的 Kerberos 驗證，則實際通訊會經過 NTLM 驗證。 在這種部署中，一旦升級至 Windows Server 2019 之後，您就可以繼續使用 NTLM 驗證。 若要移至以 Kerberos 為基礎的驗證，您必須使用網路控制站 DNS 名稱進行 REST 作業，並提供網路控制站節點的許可權來註冊 SPN。

---