---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: "設定合作夥伴公司"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>設定合作夥伴公司

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要部署新的合作夥伴組織中 Active Directory 同盟服務 \(AD FS\)，完成的工作中[檢查清單︰ 設定資源合作夥伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)或[檢查清單：設定 Account 合作夥伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)、根據您的設計 AD FS。  
  
> [!NOTE]  
> 當您使用這些檢查清單時，建議您第一次看到 account 協力廠商或計劃中的指導方針資源合作夥伴參考[在 Windows Server 2012 中 AD FS 程式設計指南](https://technet.microsoft.com/library/dd807036.aspx)繼續新的合作夥伴公司所設定的程序。 遵循檢查清單，如此一來，有助於提供更好了解完整 AD FS 設計和部署的資訊 account 協力廠商或資源合作夥伴組織。  
  
## <a name="about-account-partner-organizations"></a>關於 account 合作夥伴公司  
Account 合作夥伴是在聯盟信任關係的實際儲存帳號，AD FS – 支援屬性市集中的組織。 Account 合作夥伴負責收集驗證使用者的認證，建立宣告的使用者，並將宣告封裝安全性權杖到。 然後，這些權杖可以顯示跨，可讓存取 Web\ 資源資源合作夥伴組織都位於聯盟信任。  
  
亦即，account 協力廠商代表其使用者伺服器端 account\ 聯盟問題的安全性權杖組織。 聯盟伺服器 account 合作夥伴組織驗證本機使用者和建立資源合作夥伴使用的安全性權杖中決策授權。  
  
對於屬性存放區，AD FS 中的 account 合作夥伴等於概念單一 Active Directory 樹系的帳號需要實際上另一個森林中的資源的存取權。 有兩個樹系之間的關係，並使用授權的適當權限已設定的使用者想要存取的資源信任的樹系或外部信任時，只帳號此森林中的可以存取資源資源樹系。  
  
## <a name="about-resource-partner-organizations"></a>相關資源合作夥伴公司  
資源夥伴在組織中 AD FS 部署網頁伺服器的所在位置。 資源合作夥伴信任 account 合作夥伴驗證使用者。 因此，以做出授權，資源夥伴所使用的安全性權杖來自 account 合作夥伴使用者在已封裝宣告。  
  
囉資源合作夥伴代表的組織的網頁伺服器受到 resource\ 端聯盟伺服器。 聯盟伺服器，資源合作夥伴使用的由 account 合作夥伴做出授權網頁伺服器資源夥伴中的安全性權杖。  
  
如 AD FS 資源，資源合作夥伴組織中的網頁伺服器可能必須具有 Windows 身分基本知識 \(WIF\) 運作安裝或安裝的 Active Directory 同盟服務 \(AD FS\) 1.x Claims\ 感知 Web 代理程式角色服務。 作為 AD FS 資源的網頁伺服器可裝載 Web\ browser\ 根據或 Web\ service\ 型應用程式。  
