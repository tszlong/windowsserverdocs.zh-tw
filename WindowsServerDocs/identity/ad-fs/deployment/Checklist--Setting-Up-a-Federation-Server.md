---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 檢查清單-設定同盟伺服器
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1eb25bdbd07088c13f70b51d5e78e7e12383e3f1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965910"
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單：設定同盟伺服器

此檢查清單包含在 &reg; Active Directory 同盟服務 AD FS 中為同盟伺服器角色準備執行 Windows server 2012 的伺服器所需的部署工作 \( \) 。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**同盟伺服器檢查清單：設定同盟伺服器**  
  
||工作|參考|  
|-|--------|-------------|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|開始部署 AD FS 同盟伺服器之前，請先參閱;1. \) 選擇 Windows 內部資料庫 \( WID \) 或 SQL Server 儲存 AD FS 設定資料庫2的優點和缺點。 \)AD FS 部署拓撲類型及其相關聯的伺服器位置和網路設定建議。|![設定同盟伺服器會](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[決定您的 AD FS 部署拓撲](../design/determine-your-ad-fs-deployment-topology.md)<p>![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考慮](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11))|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 容量規劃指南，以判斷您應該在生產環境中使用的適當同盟伺服器數目。|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[為同盟伺服器容量設定同盟伺服器規劃](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg749917(v=ws.11))|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱 AD FS 設計指南中的資訊，瞭解如何將同盟伺服器放置在您的組織中|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器位置](../design/planning-federation-server-placement.md)<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[要放置同盟伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11))的聯盟伺服器|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷獨立的 \- 同盟伺服器或同盟伺服器陣列是否較適合您的部署。|![建立](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807101(v=ws.11))伺服器的時機<p>![建立](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807062(v=ws.11))伺服器伺服器陣列的時機|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷是否要在帳戶夥伴組織中或資源夥伴組織中建立此新的同盟伺服器。|![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器檢查帳戶夥伴中的同盟伺服器角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807117(v=ws.11))<p>![設定](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器檢查資源夥伴中的同盟伺服器角色](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807065(v=ws.11))|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|查看同盟伺服器如何使用服務通訊憑證和權杖 \- 簽署憑證的相關資訊，以安全的方式驗證用戶端和同盟伺服器 proxy 要求。 **注意：** 雖然使用具有不合格主機名稱的憑證（例如 HTTPs： \/ \/ myserver）很長，但這些憑證沒有安全性價值，而且可以讓攻擊者模擬企業用戶端的 AD FS 同盟服務。 因此，建議您使用完整功能變數名稱 \( FQDN \) （例如 HTTPs： \/ \/ myserver.contoso.com），並且只使用發行給同盟服務 FQDN 的 SSL 憑證。|![設定同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[證書需求](../design/certificate-requirements-for-federation-servers.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|請參閱有關如何更新公司網路網域名稱系統 DNS 的資訊 \( ， \) 以便能夠成功解析同盟伺服器的名稱。|![設定同盟伺服器的聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名稱解析需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807041(v=ws.11))|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|將即將成為同盟伺服器的電腦加入至帳戶夥伴樹系或資源夥伴樹系中的網域，以便用來驗證該樹系的使用者或信任的樹系。 **注意：** 如果您想要在帳戶夥伴組織中設定同盟伺服器，電腦必須先聯結至樹系中的任何網域，而您的同盟伺服器將用來驗證來自該樹系的使用者，或來自信任的樹系。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在公司網路 DNS 中建立新的資源記錄，將同盟伺服器的 DNS 主機名稱指向同盟伺服器的 IP 位址。|![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器將主機 &#40;&#41; 資源記錄新增至同盟伺服器的公司 DNS](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性 \) 如果您要將同盟伺服器新增至同盟伺服器陣列，您可能必須先在 \- 伺服器陣列中的第一部同盟伺服器上匯出現有權杖簽署憑證的私密金鑰， \( \) 如此當其他同盟伺服器必須匯入相同的憑證時，憑證的檔案格式就已準備就緒。<p>當您所發出的伺服器驗證憑證可以由多部電腦重複使用， \( 而不需要匯出， \) 或當您將為伺服器陣列中的每部同盟伺服器取得唯一的伺服器驗證憑證時，就不需要匯出私密金鑰。 **注意：** 中的 [AD FS 管理] 嵌入式管理單元 \- 是指做為服務通訊憑證之同盟伺服器的伺服器驗證憑證。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|當您 \( 從憑證授權單位單位 CA 取得伺服器驗證憑證或私密金鑰之後 \) \( \) ，您必須將憑證檔案匯入每部同盟伺服器的預設網站。 **注意：** 您必須先在預設的網站上安裝此憑證，才能使用 AD FS 同盟伺服器設定向導。|![設定同盟伺服器將](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入到預設的網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性 \) ：若要從 CA 取得伺服器驗證憑證，您可以使用 Internet Information Services \( IIS \) 來為您的同盟伺服器建立範例憑證。 **注意：** 使用自我 \- 簽署的伺服器驗證憑證，在生產環境中部署同盟伺服器並不是安全性最佳作法。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立自我 \- 簽署的伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)，然後完成將[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)的程式|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果您要在帳戶夥伴組織中設定同盟伺服器陣列環境，您必須在伺服器陣列所在的 Active Directory Domain Services AD DS 中建立並設定專用的服務帳戶， \( \) 並設定伺服器陣列中的每部同盟伺服器使用此帳戶。 藉由執行此程式，您將可讓公司網路上的用戶端使用 Windows 整合式驗證，向伺服器陣列中的任何同盟伺服器進行驗證。|![設定同盟伺服器手動設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器陣列的服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器的電腦上安裝同盟服務角色服務。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務角色服務](Install-the-Federation-Service-Role-Service.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|使用 [AD FS 同盟伺服器設定] 嚮導，將電腦上的 AD FS 軟體設定為同盟伺服器角色。<p>當您想要設定獨立的同盟伺服器時，請遵循此程式 \- ，在新的伺服器陣列中建立第一部同盟伺服器，或將電腦加入現有的同盟伺服器陣列。 **注意：** 針對同盟 Web 單一登入 \- \( SSO \) 設計，您必須在帳戶夥伴組織中至少有一部同盟伺服器，並在資源夥伴組織中至少要有一部同盟伺服器。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立獨立同盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<p>![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在同盟伺服器陣列中建立第一部同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<p>![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將同盟伺服器新增至同盟伺服器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)陣列|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性 \) 使用 [AD FS 管理] 嵌入式管理單元， \- 新增和設定部署設計所需的必要 AD FS 憑證。 如需有關如何使用嵌入式管理單元新增或變更憑證的詳細資訊 \- ，請參閱[同盟伺服器的憑證需求](../design/certificate-requirements-for-federation-servers.md)。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖簽署憑證](Add-a-Token-Signing-Certificate.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖解密憑證](Add-a-Token-Decrypting-Certificate.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果這是您組織中的第一部同盟伺服器，請設定同盟服務，使其符合您的 AD FS 設計。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)<p>![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|從用戶端電腦，確認同盟伺服器可運作。|![設定](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[同盟伺服器驗證同盟伺服器是否可運作](Verify-That-a-Federation-Server-Is-Operational.md)| 
