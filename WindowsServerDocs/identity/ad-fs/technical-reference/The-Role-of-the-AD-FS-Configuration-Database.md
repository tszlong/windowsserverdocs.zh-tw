---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: "AD FS 設定資料庫的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>AD FS 設定資料庫的角色
AD FS 設定資料庫儲存，表示 Active Directory 同盟服務 \(AD FS\) \(that is, the Federation Service\) 一個執行個體的所有設定資料。 AD FS 設定資料庫定義參數同盟服務需要找出合作夥伴、憑證，屬性商店、宣告和有關這些相關聯的實體各種資料的集。 您可以將此設定資料 Microsoft SQL Server® 資料庫或包含 Windows Server® 2008 年、Windows Server 2008 R2 和 Windows Server® 2012 年的 Windows 內部資料庫 \(WID\) 功能。  
  
> [!NOTE]  
> AD FS 設定資料庫整個到可以是儲存在 WID 的執行個體或執行個體的 SQL 資料庫，但不是這兩個。 這表示您無法使用使用 WID 與其他人使用 AD FS 設定資料庫同一個執行個體 SQL Server 資料庫某些聯盟伺服器。  
  
您可以使用下列資訊，以及 content 中提供此主題中[AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489.aspx)以深入了解的優點和缺點選擇 [WID 或 SQL Server 儲存 AD FS 資料庫設定：  
  
WID 使用關聯的資料儲存並不具有其本身管理使用者介面 \(UI\)。 而是系統管理員可以使用廣告 FS 管理 snap\ 中、Fsconfig.exe 或 Windows PowerShell™ cmdlet 修改廣告 FS 設定資料庫到。  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>儲存設定資料庫 AD FS 使用 WID  
您可以建立 AD FS 設定資料庫使用在市集中為 WID 使用 Fsconfig.exe command\ 列工具或 AD FS 聯盟伺服器設定精靈。 當您使用這些工具時，您可以選擇下列其中一個選項來建立您的聯盟伺服器拓撲任何。 每個選項來儲存 AD FS 設定資料庫中使用 WID:  
  
-   建立 stand\ 只聯盟伺服器  
  
-   建立的第一個聯盟伺服器聯盟伺服器陣列  
  
-   新增至聯盟伺服器陣列聯盟伺服器  
  
如果您選擇 stand\ 只，WID 用來儲存 AD FS 設定資料庫一個執行個體。 無法分享焦跨多個聯盟伺服器。 它是適用於對短片測試 lab 環境。 如需有關 stand\ 只聯盟伺服器選項或一項設定的詳細資訊，請查看[獨立聯盟伺服器使用 WID](https://technet.microsoft.com/library/gg982486.aspx)或[建立伺服器獨立聯盟](https://technet.microsoft.com/library/ee913579.aspx)。  
  
如果您在聯盟伺服器發電廠選項中選取的第一個聯盟伺服器，WID 延展性，允許其他聯盟伺服器加入稍後發電廠設定。 如需部署 WID 發電廠或一項設定的相關資訊，請查看[聯盟伺服器發電廠使用 WID](https://technet.microsoft.com/library/gg982492.aspx)或[建立第一個聯盟伺服器聯盟伺服器陣列](https://technet.microsoft.com/library/dd807070.aspx)  
  
如果您選取新增聯盟伺服器] 選項，WID 已複製到新的聯盟伺服器的變更設定資料庫設定的時間間隔。 如需有關新增至 WID 陣列聯盟伺服器，查看[聯盟伺服器發電廠使用 WID](https://technet.microsoft.com/library/gg982492.aspx)或[新增聯盟伺服器聯盟伺服器陣列到](https://technet.microsoft.com/library/ee913575.aspx)。  
  
> [!NOTE]  
> 當您部署聯盟伺服器陣列使用 WID 時，AD FS 的部分功能可能無法使用。 若要讓您設定您的伺服器陣列時設定的完整功能的存取，請考慮使用 Microsoft SQL Server 改為市集 AD FS 設定資料庫。 如需詳細資訊，請查看[AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx)。  
  
### <a name="how-a-wid-federation-server-farm-works"></a>WID 聯盟伺服器陣列的運作方式  
本節重要的概念描述 WID 聯盟伺服器發電廠之間伺服器聯盟主要和次要聯盟伺服器複寫資料的方式。 .  
  
#### <a name="primary-federation-server"></a>主要聯盟伺服器  
電腦執行的是 Windows Server 2008、Windows Server 2008 R2 或 Windows Server® 2012 聯盟伺服器角色使用 AD FS 聯盟伺服器設定精靈中的設定，且具有 AD FS 設定資料庫讀取/寫入複本主要聯盟伺服器。 當您使用 AD FS 聯盟伺服器設定精靈，並選取 [建立新的同盟服務，以及陣列中的第一個聯盟伺服器讓該電腦一律建立主要聯盟伺服器。 所有其他聯盟伺服器這個，也稱為「次要聯盟伺服器必須同步一份儲存在本機 AD FS 設定資料庫主要聯盟伺服器上所做的變更。  
  
#### <a name="secondary-federation-servers"></a>第二個聯盟伺服器  
第二個聯盟伺服器會儲存一份 AD FS 設定資料庫從主要聯盟伺服器，但這些複本 read\ 僅限。 連接到第二個聯盟伺服器，並輪詢它定期檢查是否已變更的資料與農場中的主要同盟伺服器同步處理資料。 次要同盟伺服器存在於提供主要聯盟伺服器容錯時 load\ 平衡存取要求其他網站整個網路環境中執行的。  
  
> [!NOTE]  
> 如果主要聯盟伺服器當機，離線所有第二個聯盟伺服器繼續處理要求正常。 不過，不新可進行變更同盟服務直到主要聯盟伺服器帶回上網。 您也可以提名使用 Windows PowerShell 成為聯盟主要伺服器次要聯盟伺服器。 如需詳細資訊，請查看[使用 Windows PowerShell AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>AD FS 設定資料庫進行同步處理  
AD FS 設定資料庫播放重要的角色，因為它可供網路中的所有聯盟伺服器上提供容錯和 load\ 平衡處理要求的功能 \（網路 load\ 平衡器時 used\）。 不過，可在此容量次要聯盟伺服器，必須同步 AD FS 設定資料庫儲存主要聯盟伺服器上。  
  
當您新增發電廠聯盟伺服器時、新電腦，將會變成次要聯盟伺服器連接到主要聯盟伺服器複寫 AD FS 設定資料庫的複本。 點之後，從新聯盟伺服器持續更新會從主要聯盟伺服器定期，如下所示。  
  
![AD FS 角色](media/adfs2_WID.png)  
  
每個次要聯盟伺服器輪詢主要聯盟伺服器變更為每個 5 分鐘。 您可以調整此預設 five\ 分鐘值，或使用 Windows PowerShell cmdlet anytime 強制立即同步處理。 如需如何執行此動作，請查看[使用 Windows PowerShell AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
在 WID 同步處理程序也支援更有效率傳輸中繼變更的增量傳輸。 增量傳輸程序會需要少網路上的資料傳輸與傳輸速度完成。  
  
> [!NOTE]  
> AD FS 設定資料庫從 WID SQL Server 的執行個體的移轉支援。 如需如何執行此動作，請查看[AD FS：移轉 AD FS 設定資料庫 SQL Server 以](https://go.microsoft.com/fwlink/?LinkId=192232)TechNet Wiki 網站上。  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>儲存設定資料庫 AD FS 使用 SQL Server  
您可以建立 AD FS 設定資料庫使用在市集中為單一 SQL Server 資料庫執行個體使用 Fsconfig.exe command\ 列工具。 使用 AD FS 設定資料庫 SQL Server 資料庫透過 WID 提供下列優點：  
  
-   系統管理員可以利用 SQL Server 的可用性功能  
  
-   高流量提供額外的效能增加。  
  
-   提供的 SAML 成品解析度及 SAML 日 WS\-聯盟權杖重播偵測 \(described below\) 功能的支援。  
  
字詞「主要聯盟伺服器」不適用於 AD FS 設定資料庫儲存在資料庫執行個體 SQL 因為所有聯盟伺服器同樣讀取和寫入 AD FS 設定資料庫使用相同叢集的 SQL Server 的執行個體，如下所示。  
  
![AD FS 角色](media/adfs2_SQL.png)  
  
您可以使用 SQL Server 設定兩個或更多一同合作，以確保 AD FS 進行傳入 client 要求服務的可用性伺服器叢集伺服器。 可用性提供中，您可以透過新增額外的伺服器增加伺服器容量 scale\ 出架構。 自動叢集容錯移轉的減輕單點失敗。  
  
您可以使用的網路 load\ 平衡和 SQL 叢集技術提供容錯移轉服務達成可用性。 如需了解如何設定可用性 SQL Server 的詳細資訊，請查看[高可用性方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)。  
  
### <a name="saml-artifact-resolution"></a>SAML 成品解析度  
安全性判斷提示標記語言 \(SAML\) 成品解析度為基礎 SAML 2.0 通訊協定告訴您如何信賴可以直接從宣告提供者擷取預付碼的一部分，並且端點。 在第一階段中的解析度程序，瀏覽器 client 連絡人資源聯盟伺服器，並提供成品。 在第二個階段中，資源聯盟伺服器傳送成品解析成品郵件帳號合作夥伴組織中地方裝載 SAML 成品端點 url。 中的最後階段，account 聯盟伺服器會發出權杖代表瀏覽器 client 聯盟伺服器。  
  
> [!NOTE]  
> 如果您是 account 合作夥伴公司的系統管理員，請務必指派，或是 SSL 憑證鏈結根憑證的 Windows 根憑證計畫的成員，繫結至在聯盟被動式網站 \ (<ComputerName>\\Sites\\Default WebSite\\adfs\\ls\) 發電廠中的所有 account 聯盟伺服器上。 這是重要防止手動新增到本機電腦受信任的人憑證存放區 SSL 憑證有或無法解析在組織中發行成品資源聯盟伺服器。  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML 日 WS-聯盟權杖重播偵測  
字詞*權杖執行*指的是，讓瀏覽器 client account 合作夥伴組織中的嘗試資源聯盟伺服器傳送驗證相同權杖它收到 account 聯盟伺服器多次動作。  這個動作會發生當使用者按下**回**為了重新提交驗證網頁瀏覽器的按鈕。  
  
AD FS 提供的功能稱為*權杖偵測執行*的多個權杖要求所使用的相同權杖偵測到，然後捨棄。 這項功能，當權杖重播偵測會用來保護驗證要求 WS\-聯盟被動式設定檔和 SAML WebSSO 基本資料的完整性確定相同權杖一律不會超過一次。 這項功能應該會在安全性時非常高考量例如支援使用 kiosk。  
  
在 kiosk 範例中，使用者可以使用所有網站登入，稍後惡意的使用者可以嘗試使用的瀏覽器歷史以重新提交的使用者先前已載入驗證聯盟的頁面。 這項功能來儲存其他資訊，有關偵測後續重播的預付碼和防止後續多個驗證嘗試以 account 合作夥伴公司所做的每個成功驗證降低此問題。  
  

