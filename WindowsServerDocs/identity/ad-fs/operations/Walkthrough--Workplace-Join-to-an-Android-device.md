---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 逐步解說-Android 裝置加入工作地點
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73dbe4d62d460f9487467c7d4198d62b3b6af539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188926"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>逐步解說：Android 裝置加入工作地點



## <a name="join-your-device-with-workplace-join"></a>將您的裝置加入工作地點網路

> [!NOTE]
> Android 的 「 工作地方聯結需要 Azure Active Directory 裝置註冊服務。 若要強制執行條件式裝置原則在內部，必須部署目錄同步作業工具 (DirSync) 啟用的裝置物件回寫選項。 在現階段，裝置寫回至 Active Directory 從 Azure Active Directory 可能需要最多 3 個小時。 因此，使用者必須等候存取內部部署 web 應用程式，在建立工作帳戶之後的 3 小時內。 如需有關部署 Azure Active Directory 裝置註冊服務，請參閱， [Azure Active Directory 裝置註冊服務概觀](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>建立工作帳戶加入您工作場所聯結的裝置

1.  您必須建立工作帳戶加入您的裝置加入工作地點網路裝置上安裝 Azure Authenticator 應用程式。 下列 URL 中的指示如何在 Android 裝置上安裝 Azure authenticator 應用程式，並新增工作帳戶。 工作帳戶到受信任的裝置讓您的 Android 裝置，並在裝置上的應用程式提供單一登入 (SSO)。 依照您的 IT 系統管理員的建議，您可以使用受信任的裝置來存取 web 應用程式和現代化的特定業務應用程式。 如需詳細資訊，請參閱 <<c0> [ 適用於 Android 的 Azure Authenticator](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)。

## <a name="see-also"></a>另請參閱
[從任何裝置加入工作地點網路提供 SSO 和無縫式第二個因素 Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[設定使用 Azure Active Directory 裝置註冊服務的內部部署條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


