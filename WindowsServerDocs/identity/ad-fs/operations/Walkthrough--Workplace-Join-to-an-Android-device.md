---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: "逐步解說-Android 裝置的工作地點加入"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>逐步解說： Android 裝置的工作地點加入

>適用於：Windows Server 2016、Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>使用工作地點加入加入您的裝置

> [!NOTE]
> Android 的工作地點加入需要 Azure Active Directory 裝置登記服務。 為了執行條件裝置原則先，必須支援的裝置物件回寫選項部署 Directory 同步工具 (DirSync)。 現在，裝置寫入-回到 Active Directory Azure Active directory 花費高達 3 個小時。 因此，使用者必須等到存取先 web 應用程式，在建立工作 account 之後 3 個小時。 如需部署的 Azure Active Directory 裝置登記服務，請[Azure Active Directory 裝置登記服務概觀](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>建立工作帳號，加入您的工作地點加入的裝置

1.  您將需要安裝您的裝置上建立的工作帳號，加入您的裝置使用工作地點加入 Azure Authenticator 應用程式。 下列 URL 包含如何 Android 裝置上安裝 Azure 驗證器 app，並在新增工作帳號相關指示。 工作帳號到信任的裝置可讓您 Android 裝置，並提供單一登入 (SSO) 的裝置上的應用程式。 根據您的 IT 系統管理員的建議，您可以使用存取 web 應用程式和現代業務的應用程式受信任的裝置。 如需詳細資訊，請查看[適用於 Android Azure Authenticator](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to)。

## <a name="see-also"></a>也了
[加入的任何裝置 SSO 和順暢第二個因數驗證在公司應用程式的工作地點](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[設定上場所條件使用 Azure Active Directory 裝置登記服務的存取](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


