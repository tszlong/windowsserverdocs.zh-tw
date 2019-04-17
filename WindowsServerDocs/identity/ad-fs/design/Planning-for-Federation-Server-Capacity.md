---
ms.assetid: 7013fc21-9ced-4f9d-9588-cb04d6d60924
title: "規劃聯盟伺服器容量"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 618dc9419be965dedaaf7dc946da436a5001f121
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-capacity"></a>規劃聯盟伺服器容量

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

規劃伺服器聯盟容量可協助您估計：  
  
-   哪些因素拓展 AD FS 設定資料庫的大小。  
  
-   每個聯盟伺服器的適當的硬體需求。  
  
-   將每個組織中聯盟伺服器數目。  
  
聯盟伺服器發行安全性權杖給使用者。 這些權杖顯示信賴供使用。 聯盟伺服器發行安全性權杖驗證使用者或之後接收的安全性權杖先前發行的夥伴同盟服務。 當使用者最初登入聯盟應用程式或其的安全性權杖到期時存取聯盟應用程式的安全性權杖要求同盟服務。  
  
聯盟伺服器專為容納 high\ 可用性伺服器發電廠設定使用 Microsoft 網路負載平衡 \(NLB\) 技術。 聯盟伺服器發電廠設定服務要求獨立，而不需要存取的任何通用發電廠元件每個要求。 因此，還有少參與出聯盟伺服器部署縮放比例。  
  
**建議：**  
  
-   適用於 mission\ 重大或 high\ 可用性部署，建議您建立的小聯盟伺服器陣列每個協力廠商組織，與每個發電廠，以提供容錯至少兩部聯盟伺服器。  
  
-   需可用性和輕鬆聯盟伺服器縮放比例，以查看縮放比例是處理大量的特定同盟服務秒要求建議的方法。 向上超過本指南基本設定太製作重大容量處理提高可及範圍。  
  
## <a name="ad-fs-configuration-database-size-and-growth"></a>AD FS 設定資料庫的大小和成長  
AD FS 設定資料庫大小通常被視為小，並資料庫大小不通常會主要考量 AD FS 部署。  AD FS 設定資料庫精確大小主要可以仰賴數目信任關係和相關的 trust\ 相關中繼資料，例如宣告，取得規則及監視設定設定為每個信任。 隨著信任設定資料庫中的項目數目增加，所以會需要更多磁碟空間。  
  
適用於其他部署 AD FS 設定資料庫有關，請查看[AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)。  
  
## <a name="memory-cpu-and-disk-space-requirements"></a>記憶體，CPU 磁碟空間需求  
幸好聯盟伺服器的記憶體、CPU 和磁碟空間需求是太大，且不會有可能是硬體決策開車因素。 如硬體需求的相關詳細資訊，請查看[附錄 a：審查 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)。  
  
> [!NOTE]  
> 在使用市集 AD FS 設定資料庫設定的專屬 SQL Server 聯盟伺服器陣列 AD FS product 小組所執行的測試，SQL Server 的整體負載打算低。 使用已設定為使用單一 SQL Server four\ federation\ 伺服器發電廠一個測試，在 CPU 使用率不超過 10%，即使測試的目標使用率整合聯盟伺服器。  
  
## <a name="bk_estimatefs"></a>估計聯盟伺服器，您的組織數目  
為了簡化計劃聯盟伺服器程序的硬體，AD FS product 的小組負責開發 AD FS 容量規劃縮放試算表。 此 Excel 試算表包含 calculator\ 般的功能，將會提供有關使用者在組織中，建議使用的最佳聯盟伺服器數目傳回 AD FS production 環境預期的使用方式資料。  
  
> [!NOTE]  
> 此試算表會建議可聯盟伺服器數目為基礎的硬體及網路規格期間測試使用 AD FS product 小組。 因此，聯盟伺服器，建議您將會試算表數目您必須在此處了解。  如需測試期間所使用的規格，查看主題[規劃 AD FS 伺服器容量](Planning-for-AD-FS-Server-Capacity.md)。  
  
### <a name="using-the-ad-fs-capacity-planning-sizing-spreadsheet"></a>請使用 AD FS 容量計畫縮放試算表  
當您使用此試算表時，您將需要選取 [值 \ (任一個**40%**，**60%**，或**80%**\) 最佳代表您預期的總使用者百分比將會傳送驗證要求您聯盟伺服器使用尖峰期間。  
  
然後，您將需要選取 [值 \ (任一個**1 分鐘**，**15 分鐘**，或**1 小時**\) 最佳代表您預期澳地區的山峰使用句點持續的時間長度。 例如，您可能會總數人員將會在一段 15 分鐘，登入或 60%的使用者將會在 1 小時的時間登入的使用者的值為估計 40%。 在一起，這些值定義您當時建議計算所用的山峰載入設定檔。  
  
接下來，您將需要指定總數需要目標 claims\ 感知應用程式，根據使用者是否的單一 sign\ 上存取的使用者：  
  
-   從本機電腦確實連接到您的企業網路的登入 Active Directory \（透過整合 authentication\ Windows)  
  
-   不確實連接到您的企業網路的電腦從遠端登入 Active Directory \（透過 Windows 整合驗證或使用者名稱和 password\）  
  
-   從另一個組織，而且嘗試值得信賴的合作夥伴從存取目標 claims\ 感知應用程式  
  
-   從 SAML 2.0 身分提供者和已嘗試存取目標 claims\ 感知應用程式  
  
#### <a name="how-to-use-this-spreadsheet"></a>如何使用這個試算表  
您可以使用下列步驟想要部署判斷聯盟伺服器建議的數目每個聯盟伺服器發電廠執行個體。  
  
1.  下載，然後打開[AD FS 容量規劃縮放試算表 Windows Server 2012 R2 的](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)或[AD FS 容量規劃縮放試算表 Windows Server 2016 的](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
  
2.  格中右邊的**期間澳地區的山峰系統使用量預期百分比驗證我的使用者**儲存格，按一下 [儲存格，然後選取您估計的系統使用量層級，或是使用 drop\ 向下箭號**40%**、**60%**或**80%**部署。  
  
3.  格中右邊的**下列一段時間中**儲存格，按一下 [儲存格，然後選取使用的 drop\ 向下箭號，**1 分鐘**、**15 分鐘**，或**1 小時**選取尖峰的持續時間。  
  
4.  格中右邊的**Enter 估計的數字內部應用程式的 \ (例如 SharePoint \(2007 or 2010\) 或宣告注意 web applications\)**儲存格、輸入內部應用程式，您將會在組織中使用的數字。  
  
5.  格中右邊的**Enter 估計的數字 online 應用程式 \（例如 Office 365 Exchange Online、SharePoint Online 或 Lync Online\）**儲存格、輸入 online 應用程式或服務您將會在您的組織中使用的數字。  
  
6.  要介紹標題為儲存在**號碼使用者的**，輸入每個用於您的使用者案例應用程式列上的數字將會需要單一 sign\ 上的存取權。 此資料行應包含定義的使用者，不澳地區的山峰使用者秒數。 如果您對應用程式存取嘗試必須先進行首頁領域探索頁面上，輸入**Y**。如果您不確定此選項，輸入**Y**。  
  
7.  檢視的下列建議提供的值：  
  
    1.  如建議的聯盟伺服器總數，較低右儲存格反白顯示灰色。  
  
    2.  伺服器建議的每個案例應用程式數目，如儲存格列中的灰反白顯示。  
  
> [!NOTE]  
> 將會自動儲存格要介紹標題為右邊的儲存格計算值**總數聯盟伺服器建議的**試算表底部包含即可將以每個它之前的個人資料列中的所有值總和新增額外的 20%緩衝公式。 公式新增到**總數聯盟伺服器建議的**部署的聯盟伺服器，讓它太農場整體載入曾經會叫用它飽點數量，建議您總計此緩衝中的行動組建。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
