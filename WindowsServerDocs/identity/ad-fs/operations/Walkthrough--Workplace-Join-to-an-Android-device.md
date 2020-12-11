---
description: 深入瞭解：逐步解說： Workplace Join 至 Android 裝置
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 逐步解說-Workplace Join 至 Android 裝置
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 70885fa5fa3407f3f5d324798bd4cc03ab6ad81c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039626"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>逐步解說： Workplace Join 至 Android 裝置



## <a name="join-your-device-with-workplace-join"></a>將您的裝置加入工作地點網路

> [!NOTE]
> Android workplace join 需要 Azure Active Directory 裝置註冊 Service。 若要在內部部署環境中強制執行條件式裝置原則，您必須在啟用裝置物件回寫選項的情況下，部署目錄同步作業工具 (DirSync) 。 在目前的時間，裝置回寫至 Active Directory 從 Azure Active Directory 最多可能需要3小時的時間。 因此，在建立工作帳戶之後，使用者必須等待3小時才能存取內部部署 web 應用程式。 如需部署 Azure Active Directory 裝置註冊服務的詳細資訊，請參閱 [Azure Active Directory 裝置註冊 Service 總覽](/previous-versions/azure/dn788908(v=azure.100))

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>建立公司帳戶以將您的裝置加入工作地方聯結

1.  您將需要在裝置上安裝 Azure 驗證器應用程式，以建立可將您的裝置加入工作地點聯結的工作帳戶。 下列 URL 提供如何在 Android 裝置上安裝 Azure 驗證器應用程式並新增工作帳戶的指示。 工作帳戶會讓您的 Android 裝置進入受信任的裝置，並為裝置上的應用程式提供單一 Sign-On (SSO) 。 您可以使用受信任的裝置來存取 web 應用程式，以及您的 IT 系統管理員所建議的現代化企業營運應用程式。 如需詳細資訊，請參閱 [適用于 Android 的 Azure 驗證](/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)器。

## <a name="see-also"></a>另請參閱
[從任何裝置加入工作場所，以在公司應用程式之間進行 SSO 和無縫式的次要因素驗證](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) 
[使用 Azure Active Directory 裝置註冊服務來設定內部部署條件式存取](/azure/active-directory/active-directory-device-registration-on-premises-setup)
