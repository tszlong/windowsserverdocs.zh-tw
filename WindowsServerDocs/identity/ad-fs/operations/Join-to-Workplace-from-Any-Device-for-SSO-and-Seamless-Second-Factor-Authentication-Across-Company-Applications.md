---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: 從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9d6161666be89673cff6ef1a975d3205fa4b5c9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189094"
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證



消費性裝置數目與資訊存取需求的快速增加正在變更人們對其科技的看待方式。 今日對於資訊技術的持續使用與簡單的資訊存取方式，使得工作與居家生活的界線越來越模糊。 這些正在變動的界線會伴隨著一個信念技術選取和自訂以符合使用者的人物、 活動和排程該個人-應該延伸到工作地點。 為了配合讓個人消費性裝置連線到企業網路這種持續成長的需求，我們引進了下列價值主張：

-   系統管理員可以根據應用程式、使用者、裝置與位置，來控制誰可以存取公司資源。

-   員工可以隨時從任何裝置存取應用程式與資料。 員工可以在瀏覽器應用程式與企業應用程式中使用「單一登入」。

## <a name="key-concepts-introduced-in-the-solution"></a>此解決方案引進的重要概念

### <a name="workplace-join"></a>工作地方聯結
透過使用「加入工作地點網路」，資訊工作者可以讓其個人裝置加入公司工作地點電腦所在的網路，以存取公司資源與裝置。 當您將個人裝置加入到您的工作地點時，它會成為已知裝置並提供無縫式次要因素驗證與「單一登入」，讓您可以存取工作地點資源與應用程式。 當裝置是使用「加入工作地點網路」加入時，您可以從目錄擷取裝置屬性，以針對授權簽發應用程式安全性權杖目的提供條件式存取。 可使用 [加入工作地點網路] 來加入 Windows 8.1 與 iOS 6.0+ 和 Android 4.0+ 裝置。

### <a name="BKMK_DRS"></a>Azure Active Directory 裝置註冊服務
可使用 Azure Active Directory 裝置註冊服務來執行 [加入工作地點網路]。 使用 [加入工作地點網路] 加入裝置時，服務會在 Azure Active Directory 中佈建裝置物件，然後在本機裝置上設定可用來代表裝置身分識別的金鑰。 接著可針對雲端和內部部署中裝載的應用程式，搭配使用此裝置身分識別與存取控制規則。

如需詳細資訊，請參閱 < [Azure Active Directory 中的裝置管理簡介](https://docs.microsoft.com/azure/active-directory/device-management-introduction)。

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>透過「加入工作地點網路」提供無縫式次要因素驗證
公司可以管理與資訊存取相關的風險並進行合規性控管，同時將對公司資源的存取權授消費性裝置。 裝置上的「加入工作地點網路」為系統管理員提供下列功能：

-   透過裝置驗證識別已知裝置。 系統管理員可以使用此資訊來允許條件式存取並控制對資源的存取。

-   為使用者提供更好的無縫式登入體驗，讓其可以從信任的裝置存取公司資源。

### <a name="single-sign-on"></a>單一登入
本案例中的單一登入 (SSO) 功能可以減少當使用者從已知裝置存取公司資源時會看到的密碼提示次數。 此功能表示在 SSO 存留期中，系統只會提示使用者輸入一次密碼，使用者就可以從該裝置存取公司應用程式與資源。 若裝置使用「加入工作地點網路」，已登錄使用此裝置的使用者會取得永續性 SSO (預設有效期是七天)。 此使用者在相同的工作階段中或新的工作階段中可擁有無縫式登入體驗。

## <a name="solution-overview"></a>解決方案概觀
在此解決方案中，您將了解如何在支援的裝置上使用 [加入工作地點網路]，以及如何使用 [單一登入] 來存取公司資源。

> [!NOTE]
> 若要支援 Windows 8.1、iOS 6.0+ 和 Android 4.0+ 裝置，您必須設定 Azure Active Directory 裝置註冊以及裝置物件寫回，請參閱 [使用 Azure Active Directory 裝置註冊設定內部部署條件式存取](https://msdn.microsoft.com/library/azure/dn788908.aspx)

此解決方案指南會帶您完成下列逐步解說步驟：

1.  [逐步解說：將 Windows 裝置加入工作地點網路](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [逐步解說：將 iOS 裝置加入工作地點網路](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [逐步解說：將 Android 裝置加入工作場所網路](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>另請參閱
[使用裝置註冊服務設定同盟伺服器](../deployment/configure-a-federation-server-with-device-registration-service.md)



