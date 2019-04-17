---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: 加入的任何裝置 SSO 和順暢的第二個工作地點因數驗證跨公司應用程式
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>加入的任何裝置 SSO 和順暢的第二個工作地點因數驗證跨公司應用程式

>適用於： Windows Server 2012 R2

快速消費者裝置，並普遍資訊存取權的數目增加變更連絡人察覺技術的方式。 常使用的資訊技術整天輕鬆存取的資訊，以及模糊傳統工作和家庭生命之間的邊界。 這些轉移邊界附帶信念技術選取及自訂使用者人物簡介預覽、活動及排程調整的個人-應該延伸到地點。 若要調整的個人消費者裝置連接到企業網路越來越需求，我們引進下列值建議：

-   系統管理員可以控制者可存取應用程式、 使用者、 裝置和位置為基礎的公司資源。

-   員工可以在任何裝置上存取應用程式與地方，資料。 員工可以在瀏覽器應用程式或企業中使用單一登入。

## <a name="key-concepts-introduced-in-the-solution"></a>主要概念方案

### <a name="workplace-join"></a>加入的工作地點
使用工作地點加入，資訊的背景工作就可以加入存取公司資源和服務的公司的工作地點電腦使用個人裝置。 當您到您的工作場所加入您的個人裝置時，在 [已知的裝置，並提供順暢的第二個因數驗證和單一登入的工作地點資源和應用程式。 當裝置已加入所加入的工作地點時，可以擷取屬性裝置的 directory 從磁碟機條件存取為了授權的應用程式的安全性權杖發行。 Windows 8.1 和 iOS 6.0 + 和 Android 4.0 + 裝置可以使用工作地點加入加入。

### <a name="BKMK_DRS"></a>Azure Active Directory 裝置登記服務
地點加入 Azure Active Directory 裝置登記服務是由可能。 當裝置已加入所加入的工作地點時，服務 provisions 裝置在 Azure Active Directory 物件，並會用來表示裝置的身分本機裝置上設定按鍵。 此裝置的身分裝載的雲端和先在應用程式再使用與存取控制規則。

如需關於 Azure Active Directory 裝置登記服務的詳細資訊，請查看[Azure Active Directory 裝置登記服務概觀](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx)。

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>為順暢的第二個工作地點加入因素驗證
公司可以管理相關資訊存取及磁碟機控管時授消費者與企業資源存取裝置的相容性風險。 在裝置上的工作地點加入提供給系統管理員下列功能：

-   裝置驗證辨識已知的裝置。 系統管理員可以使用此資訊來資源磁碟機條件存取和控制存取。

-   提供更順暢登入體驗的使用者存取公司資源來自信任的裝置。

### <a name="single-sign-on"></a>單一登入
單一登入 (SSO) 在本案例的範圍是減少密碼提示輸入存取公司資源已知的裝置的使用者的功能。 這項功能，表示使用者接到生命 SSO 存取公司應用程式和資源此裝置上的一次。 如果裝置使用工作地點加入，係使用此裝置的使用者會取得七天預設持續 SSO]。 此使用者有順暢登入體驗在相同的工作階段，或在新增工作階段。

## <a name="solution-overview"></a>方案概觀
此方案的一部分，您了解如何使用工作地點加入支援的裝置，並且在公司資源體驗單一登入。

> [!NOTE]
> 您必須支援 Windows 8.1，iOS 6.0 + 和 Android 4.0 + 裝置，以及裝置物件寫入返回設定 Azure Active Directory 裝置登記、查看[上場所條件使用 Azure Active Directory 裝置登記服務的存取逐步指南](https://msdn.microsoft.com/library/azure/dn788908.aspx)

此方案需要會引導您進行下列逐步解說步驟：

1.  [Windows 裝置的逐步解說： 地點加入](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [逐步解說： IOS 裝置加入的工作地點](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [使用 Android 裝置的逐步解說：地點加入](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>也了
[設定聯盟伺服器裝置登記服務與](../deployment/configure-a-federation-server-with-device-registration-service.md)



