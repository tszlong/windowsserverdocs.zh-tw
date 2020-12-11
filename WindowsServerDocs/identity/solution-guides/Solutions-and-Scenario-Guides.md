---
description: 深入瞭解：解決方案和案例指南
ms.assetid: bdb9ad4b-139c-4031-8f26-827432779829
title: 解決方案和案例指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3a3ed5a096b1d78f793895d7811cd1571806bd87
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044526"
---
# <a name="solutions-and-scenario-guides"></a>解決方案和案例指南

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


有了 Microsoft 的存取和資訊保護解決方案，您就可以在內部部署環境和雲端應用程式之間部署和設定對公司資源的存取。 而且，您可以在執行這些工作時同時保護公司資訊。

存取和資訊保護

|指南|本指南可協助您
|-----|-----
| [確保從任意地點使用任意裝置存取公司資源時的安全](/previous-versions/windows/it-pro/solutions-guidance/dn550982(v=ws.11))|本指南說明如何讓員工使用個人和公司裝置安全地存取公司應用程式和資料。
| [從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](../ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md) | 員工可以隨時從任何裝置存取應用程式與資料。 員工可以在瀏覽器應用程式與企業應用程式中使用「單一登入」。 系統管理員可以根據應用程式、使用者、裝置與位置，來控制誰可以存取公司資源。
| [透過其他多重要素驗證管理機密應用程式的風險](../ad-fs/operations/manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications.md)| 在此案例中，您會根據特定應用程式的使用者群組成員資格資料來啟用 MFA。 換句話說，您將在同盟伺服器上設定驗證原則，在屬於特定群組的使用者要求存取裝載在網頁伺服器上的特定應用程式時要求 MFA。
| [使用條件式存取控制管理風險](../ad-fs/operations/manage-risk-with-conditional-access-control.md) | AD FS 中的存取控制會使用發行授權宣告規則來執行，這些宣告規則可用來發出允許或拒絕宣告，以決定是否允許使用者或使用者群組存取 AD FS 保護的資源。 只可以在信賴憑證者信任上設定授權規則。
|[在自訂埠上為憑證金鑰型更新設定憑證註冊 Web 服務](certificate-enrollment-certificate-key-based-renewal.md)|本文提供逐步指示，說明如何在443以外的自訂埠上，將憑證註冊 Web 服務 (或憑證註冊原則 (CEP) /憑證註冊服務 (CES) # A5，以利用 CEP 和 CES 的自動續約功能。 |
