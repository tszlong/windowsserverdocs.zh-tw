---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: 設定夥伴組織
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ac83d9754365f3ceea5b363af4df93862bdb59e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855491"
---
# <a name="configuring-partner-organizations"></a>設定夥伴組織

若要在 Active Directory 同盟服務 \(AD FS\)中部署新的夥伴組織，請根據您的 AD FS 設計，完成其中一個[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)或[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)中的工作。  
  
> [!NOTE]  
> 當您使用這其中一個檢查清單時，強烈建議您先閱讀[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)中的帳戶夥伴或資源夥伴規劃指引的參考，再繼續進行設定新的夥伴組織的程式。 依照這種方式執行檢查清單，可讓您更瞭解帳戶夥伴或資源夥伴組織的完整 AD FS 設計和部署案例。  
  
## <a name="about-account-partner-organizations"></a>關於帳戶夥伴組織  
帳戶夥伴是同盟信任關係中的組織，實際上會將使用者帳戶儲存在 AD FS 支援的屬性存放區中。 帳戶夥伴負責收集與驗證使用者認證、建立該使用者的宣告，以及將宣告封裝到安全性權杖中。 然後，您可以在同盟信任中顯示這些權杖，以存取位於資源夥伴組織中的 Web\-資源。  
  
換句話說，帳戶夥伴代表其使用者帳戶\-端同盟伺服器發出安全性權杖的組織。 帳戶夥伴組織中的同盟伺服器會驗證本機使用者，並建立資源夥伴用來進行授權決策的安全性權杖。  
  
就屬性存放區而言，AD FS 中的帳戶夥伴在概念上相當於單一 Active Directory 樹系，其帳戶需要存取實體位於另一個樹系中的資源。 只有當兩個樹系之間有外部信任或樹系信任關係存在，而且使用者嘗試取得存取權的資源已設定適當的授權許可權時，此樹系中的帳戶才能存取資源樹系中的資源。  
  
## <a name="about-resource-partner-organizations"></a>關於資源夥伴組織  
資源夥伴是 Web 服務器所在之 AD FS 部署中的組織。 資源夥伴可信任帳戶夥伴來驗證使用者。 因此，若要進行授權決策，資源夥伴會取用封裝于帳戶夥伴中使用者的安全性權杖中的宣告。  
  
換句話說，資源夥伴代表其 Web 服務器受到資源\-端同盟伺服器保護的組織。 資源夥伴的同盟伺服器會使用帳戶夥伴所產生的安全性權杖，對資源夥伴中的 Web 服務器做出授權決策。  
  
若要做為 AD FS 資源，資源夥伴組織中的 Web 服務器必須已安裝 Windows Identity Foundation \(WIF\)，或已安裝 Active Directory 同盟服務 \(AD FS\) 中的 1. x 宣告\-感知 Web 代理程式角色服務。 當做 AD FS 資源運作的 web 伺服器可以裝載 Web\-瀏覽器\-或 Web\-服務\-應用程式。  
