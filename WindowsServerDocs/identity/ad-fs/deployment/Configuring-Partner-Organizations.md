---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: 設定夥伴組織
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875179"
---
# <a name="configuring-partner-organizations"></a>設定夥伴組織

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

若要部署新的夥伴組織中 Active Directory Federation Services \(AD FS\)，在完成[檢查清單：Configuring the Resource Partner Organization](Checklist--Configuring-the-Resource-Partner-Organization.md)或[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)，取決於您的 AD FS 設計。  
  
> [!NOTE]  
> 當您使用任一檢查清單時，我們強烈建議您先閱讀參考帳戶夥伴或資源夥伴計劃中的指導方針[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)然後再繼續設定新的交易夥伴組織的程序。 遵循的檢查清單，如此一來，可幫助提供進一步了解完整 AD FS 設計和部署案例的帳戶夥伴或資源夥伴組織。  
  
## <a name="about-account-partner-organizations"></a>關於帳戶夥伴組織  
帳戶夥伴是同盟信任關係，實際儲存在 AD FS 支援的屬性存放區中的 使用者帳戶中的組織。 帳戶夥伴負責收集和驗證使用者的認證、 建立該使用者的宣告，以及將宣告封裝到安全性權杖。 這些權杖接著會看到跨同盟信任，以存取 Web\-位於資源夥伴組織中的資源。  
  
換句話說，帳戶夥伴代表組織為其使用者帳戶\-端同盟伺服器簽發安全性權杖。 帳戶夥伴組織中的同盟伺服器會驗證本機使用者，並建立資源夥伴使用的安全性權杖進行授權決策。  
  
關於屬性存放區，在 AD FS 帳戶夥伴是概念上相當於單一 Active Directory 樹系的帳戶需要存取實體上位於另一個樹系的資源。 外部信任或樹系信任存在兩個樹系之間的關聯性，並使用適當的權限已設定的使用者嘗試存取的資源時，才在此樹系中的帳戶可以存取資源樹系中的資源權限。  
  
## <a name="about-resource-partner-organizations"></a>關於資源夥伴組織  
資源夥伴是 AD FS 部署中的組織，網頁伺服器位於何處。 資源夥伴可信任帳戶夥伴來驗證使用者。 因此，來製作授權決策，資源夥伴所使用的封裝在來自帳戶夥伴中的使用者的安全性權杖的宣告。  
  
換句話說，資源夥伴代表的組織之 Web 伺服器受到資源\-側邊的同盟伺服器。 在資源夥伴同盟伺服器會使用帳戶夥伴要在資源夥伴中的 Web 伺服器的授權決策時所產生的安全性權杖。  
  
若要做為 AD FS 資源，資源夥伴組織中的網頁伺服器必須有 Windows Identity Foundation \(WIF\)安裝，或有 Active Directory Federation Services \(AD FS\) 1.x宣告\-感知網路代理程式安裝的角色服務。 Web 伺服器，做為 AD FS 資源可以裝載任一 Web\-瀏覽器\-或 Web\-服務\-架構的應用程式。  
