---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "使用身分委派"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>使用身分委派

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>身分委派為何？  
身分委派是 Active Directory 同盟服務，可模擬使用者 administrator\ 指定帳號 \(AD FS\) 的功能。 模擬使用者 account 稱為*委派*。 此委派功能是重要的許多分散式應用程式的還有一系列存取控制檢查必須每個應用程式、 資料庫，或服務的原始要求授權鏈結中，依序進行。 許多 real\ 世界案例中有的 Web 應用程式 」 前端 「 必須擷取更安全 」 後端 」，例如已連接到的 Microsoft SQL Server 資料庫 Web 服務的資料。  
  
例如，現有 parts\ 排序的網站可以增強以程式設計方式，讓它可以讓合作夥伴公司檢視他們自己的購買歷史及 account 狀態。 基於安全性考量，所有的 partner 財務資料會儲存在安全資料庫專用的結構化查詢語言 \(SQL\) 伺服器上。 此時，front\ 端的應用程式中的程式碼完全不知道財務合作夥伴公司的資料。 因此，就必須從另一部電腦裝載網路上的其他地方擷取的資料 \ （在本 case\) Web 服務的部分資料庫 \(the back end\)。  
  
成功此 data\ 擷取程序，「 hand\ 搖動 「 必須接受的授權的一些連續放置 Web 應用程式與 Web 服務為部分資料庫，如下所示。  
  
![身分委派](media/adfs2_identitydelegationconcept.gif)  
  
因為原始要求的 Web 伺服器，這是位於完全不同的使用者嘗試存取的網頁伺服器組織的組織可能，請傳送以及要求您的安全性權杖不符合存取的網頁伺服器以外的任何其他電腦所需的授權條件。 因此，是中繼聯盟伺服器置於重新有適當存取權限的安全性權杖發行可協助資源合作夥伴組織，可以完成原始的使用者要求的唯一方式。  
  
## <a name="how-does-identity-delegation-work"></a>身分委派如何運作？  
Web 架構多應用程式中的應用程式通常通話 Web 服務存取常用的資料或功能。 請務必知道的原始的使用者身分服務可以做出授權，並促進稽核，這些 Web 服務。 若是如此，front\ 端 Web 應用程式表示使用者 Web 服務為代理人。 AD FS 幫助您允許 Active Directory 帳號，做為另一部信賴使用者本案例。 下圖顯示身分委派案例。  
  
![身分委派](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank 嘗試存取 part\ 訂購歷史從在另一部組織 Web 應用程式。 他 client 的電腦會要求並權杖接收 AD FS front\ 端 part\ 訂購 Web 應用程式。  
  
2.  Client 電腦 Web 應用程式，包括在步驟 1 所證明 client 的身分獲得權杖傳送要求。  
  
3.  需要 Web 應用程式與 Web 服務才能完成交易 client 的通訊。 Web 應用程式的連絡人取得委派預付碼和 Web 服務互動 AD FS。 委派權杖的安全性權杖發給做為使用者委派。 AD FS 傳回使用 Web 服務為目標，client 宣告委派預付碼。  
  
4.  Web 應用程式使用的從在執行 「 步驟 3 存取做為 client Web 服務的 AD FS 取得預付碼。 檢查委派預付碼，Web 服務可以判斷 Web 應用程式，做為 client。 Web 服務執行它授權的原則、 登的要求，並提供所需的部分歷史資料原來 Web 應用程式要求 Frank，因此到 Frank。  
  
適用於特定代理人，AD FS] 可以限制的 Web 應用程式可能會要求委派權杖 Web 服務。 不需要 client 的電腦不會有才能繼續此操作 Active Directory 負責。 最後，如之前所述，Web 服務可以輕鬆地判斷代理人做為使用者的身分。 這可讓 Web 服務依據他們的交談直接 client 電腦，或是透過委派不同的行為。  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>設定 AD FS 進行身分委派  
您可以使用 AD FS 管理 snap\ 中設定的身分委派 AD FS 每當您需要協助的資料擷取程序。 您將其設定之後，AD FS 可產生新的安全性權杖將會包含 back\ 後端服務可能需要授權操作之前它可提供存取受保護資料。  
  
AD FS 不會限制可模擬的使用者。 AD FS 進行身分委派設定之後，它會執行下列：  
  
-   它也會判斷哪一部伺服器委派要求權杖模擬使用者的授權。  
  
-   建立，並保留不同委派 client account 和伺服器做為委派這兩個身分操作。  
  
您可以設定委派身分加入信賴廠商信任 snap\ 中 AD FS 管理委派授權規則。 如需如何執行此動作，請查看[檢查清單︰ 建立理賠要求規則可以方信任](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)。  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>設定的身分委派 front\ 端 Web 應用程式  
開發人員可擁有數個選項可供它們適當計畫將 AD FS 電腦委派要求 Web front\ 端的應用程式或服務。 如需如何自訂身分委派使用 Web 應用程式的詳細資訊，請查看[Windows 身分基本知識 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
