---
description: 深入瞭解：設定夥伴組織
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: 設定夥伴組織
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 6f62c900e1b1f168f863e3bf34bd7e5f013d7a9b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043486"
---
# <a name="configuring-partner-organizations"></a>設定夥伴組織

若要在 Active Directory 同盟服務 AD FS 中部署新的夥伴組織 \( \) ，請完成下列任一檢查清單中的工作 [：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md) 或 [檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)，視您的 AD FS 設計而定。

> [!NOTE]
> 當您使用上述其中一種檢查清單時，強烈建議您先閱讀 [Windows Server 2012 AD FS 設計指南](../design/ad-fs-design-guide-in-windows-server-2012.md) 中的帳戶夥伴或資源夥伴規劃指導方針的參考，再繼續進行設定新的夥伴組織的程式。 以這種方式遵循檢查清單，將有助於進一步瞭解帳戶夥伴或資源夥伴組織的完整 AD FS 設計和部署案例。

## <a name="about-account-partner-organizations"></a>關於帳戶夥伴組織
帳戶夥伴是同盟信任關係中的組織，會實際將使用者帳戶儲存在 AD FS 支援的屬性存放區中。 帳戶夥伴負責收集與驗證使用者認證、建立該使用者的宣告，以及將宣告封裝到安全性權杖中。 然後，您可以透過同盟信任呈現這些權杖，以存取 \- 位於資源夥伴組織中的網路資源。

換句話說，帳戶夥伴代表帳戶 \- 端同盟伺服器發出安全性權杖之使用者的組織。 帳戶夥伴組織中的同盟伺服器會驗證本機使用者，並建立資源夥伴用來進行授權決策的安全性權杖。

關於屬性存放區，AD FS 中的帳戶夥伴在概念上等同于單一 Active Directory 樹系，其帳戶需要存取實體位於另一個樹系的資源。 只有當兩個樹系之間存在外部信任或樹系信任關係，而且使用者嘗試取得存取權的資源已設定適當的授權許可權時，此樹系中的帳戶才可以存取資源樹系中的資源。

## <a name="about-resource-partner-organizations"></a>關於資源夥伴組織
資源夥伴是在 Web 服務器所在的 AD FS 部署中的組織。 資源夥伴可信任帳戶夥伴來驗證使用者。 因此，若要進行授權決策，資源夥伴會取用來自帳戶夥伴中使用者的安全性權杖所封裝的宣告。

換句話說，資源夥伴代表其 Web 服務器受到資源 \- 端同盟伺服器保護的組織。 資源夥伴的同盟伺服器會使用帳戶夥伴所產生的安全性權杖，為資源夥伴中的網頁伺服器做出授權決定。

若要以 AD FS 資源的形式運作，資源夥伴組織中的網頁伺服器必須安裝 Windows Identity Foundation \( WIF， \) 或已 \( 安裝 Active Directory 同盟服務 AD FS 1.X \) 宣告 \- 感知 Web 代理程式角色服務。 做為 AD FS 資源的 web 伺服器可以裝載以網頁 \- 瀏覽器為 \- 基礎或 web \- 服務為基礎的 \- 應用程式。
