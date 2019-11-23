---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 逐步解說-Workplace Join 至 Android 裝置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c021e8b67963df4f885059c75eff5e94d6dcdafb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407473"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>逐步解說： Workplace Join 至 Android 裝置



## <a name="join-your-device-with-workplace-join"></a>將您的裝置加入工作地點網路

> [!NOTE]
> Android workplace join 需要 Azure Active Directory 裝置註冊服務。 若要在內部部署環境中強制執行條件式裝置原則，必須部署目錄同步作業工具（DirSync），並啟用裝置物件回寫選項。 從 Azure Active Directory 到目前為止，裝置寫回 Active Directory 可能需要最多3小時的時間。 因此，在建立工作帳戶之後，使用者必須等候3小時來存取內部部署 web 應用程式。 如需部署 Azure Active Directory 裝置註冊服務的詳細資訊，請參閱[Azure Active Directory 裝置註冊服務總覽](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>建立工作帳戶，以將您的裝置加入工作地點聯結

1.  您將需要在您的裝置上安裝 Azure 驗證器應用程式，以建立可將您的裝置加入工作地點聯結的公司帳戶。 下列 URL 提供如何在 Android 裝置上安裝 Azure 驗證器應用程式並新增工作帳戶的指示。 工作帳戶會讓您的 Android 裝置進入受信任的裝置，並提供單一登入（SSO）給裝置上的應用程式。 您可以使用受信任的裝置，依您的 IT 系統管理員的建議來存取 web 應用程式和現代化的企業營運應用程式。 如需詳細資訊，請參閱[適用于 Android 的 Azure 驗證](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)器。

## <a name="see-also"></a>請參閱
[從任何裝置加入工作場所以進行 SSO，並在公司應用程式之間進行無縫第二因素驗證](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[設定使用 Azure Active Directory 裝置註冊服務的內部部署條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


