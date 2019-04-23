---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 檢查清單-設定同盟伺服器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 44448088184d86874e91855d8a51ef40d8cea049
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880499"
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單：設定同盟伺服器

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

這份檢查清單包括部署工作所需準備同盟伺服器角色在 Active Directory Federation Services 中執行 Windows Server® 2012年的伺服器\(AD FS\)。  
  
> [!NOTE]  
> 請依序完成此檢查清單中的工作。 當某個參考連結帶您進入某個程序時，請在完成該程序中的步驟之後返回本主題，讓您可以繼續進行此檢查清單中其餘的工作。  
  
![設定同盟伺服器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單：設定同盟伺服器**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在開始部署您的 AD FS 同盟伺服器之前，請檢閱;1.\)選擇其中一個 Windows 內部資料庫的優缺點\(WID\)或 SQL Server 來儲存 AD FS 設定資料庫 2。\)AD FS 部署拓撲類型和其相關聯的伺服器位置和網路配置建議。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您的 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲考量](https://technet.microsoft.com/library/gg982489.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|檢閱 AD FS 容量規劃指引，以判斷適當的數字，您應該使用您的生產環境中的同盟伺服器。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器容量規劃](https://technet.microsoft.com/library/gg749917.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|檢閱有關組織中放置同盟伺服器的位置中的 AD FS 設計指南|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[規劃同盟伺服器的位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何處放置同盟伺服器](https://technet.microsoft.com/library/dd807127.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|判斷是否有攤\-單獨使用同盟伺服器或同盟伺服器陣列是更適合您的部署。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立同盟伺服器的時機](https://technet.microsoft.com/library/dd807101.aspx)<br /><br />![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[建立同盟伺服器陣列的時機](https://technet.microsoft.com/library/dd807062.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|決定在帳戶夥伴組織中或資源夥伴組織中，是否會建立這個新的同盟伺服器。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢閱帳戶夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢閱資源夥伴中的同盟伺服器角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|檢閱有關同盟伺服器會如何使用服務通訊憑證和權杖\-簽署憑證來安全地驗證用戶端和同盟伺服器 proxy 的要求。 **注意**：雖然長久以來的常見的作法是使用憑證搭配非完整的主機名稱，例如 https:\/\/myserver，這些憑證沒有安全性價值，並且可以讓攻擊者模擬以 AD FS 同盟服務企業用戶端。 因此，建議使用完整的網域名稱\(FQDN\)例如 https:\/\/myserver.contoso.com 和只使用 SSL 憑證發給您的 Federation Service 的 FQDN。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|檢閱有關如何更新公司的網路網域名稱系統資訊\(DNS\) ，以便進行同盟伺服器的成功名稱解析。|![設定同盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[同盟伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|將即將成為同盟伺服器，它用來驗證使用者，該樹系，或是從信任樹系的資源夥伴樹系的帳戶夥伴樹系中的網域電腦。 **注意：** 如果您想要設定帳戶夥伴組織中的同盟伺服器時，電腦必須先加入其中您的同盟伺服器將用來驗證使用者從該樹系或信任樹系的樹系中任何網域。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將電腦加入網域](Join-a-Computer-to-a-Domain.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|公司網路之同盟伺服器的 DNS 主機名稱指向同盟伺服器的 IP 位址的 DNS 中建立新的資源記錄。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[主機&#40;的&#41;至同盟伺服器的公司 DNS 資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\)如果您將新增同盟伺服器至同盟伺服器陣列，您可能必須先將現有的語彙基元的私密金鑰匯出\-簽署憑證\(伺服陣列中第一部同盟伺服器\) ，所以您需要準備憑證的檔案格式時的其他同盟伺服器必須匯入相同的憑證。<br /><br />匯出私密金鑰時不需要您的發行的伺服器驗證憑證，即可重複使用多部電腦\(而不需要匯出\)或當您將會取得 「 唯一的伺服器驗證憑證伺服器陣列中每部同盟伺服器。 **注意：** AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|取得伺服器驗證憑證後\(或 私用金鑰\)從憑證授權單位\(CA\)，您必須再將憑證檔案匯入預設的網站的每部同盟伺服器。 **注意：** 預設的網站上安裝此憑證是必要條件，才能使用 AD FS 同盟伺服器設定精靈。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\)從 CA 取得伺服器驗證憑證的替代方案，您可以使用 Internet Information Services \(IIS\)來建立您的同盟伺服器的範例憑證。 **注意**：它不是安全性最佳作法，部署在生產環境中的同盟伺服器使用自我\-簽署伺服器驗證憑證。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS:建立自我\-簽署伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)，然後完成此程序[伺服器驗證憑證匯入至預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果您將為帳戶夥伴組織中設定同盟伺服器陣列環境，您必須建立並設定專用的服務帳戶在 Active Directory 網域服務中\(AD DS\)伺服器陣列所在，若要使用此帳戶伺服陣列中設定每部同盟伺服器。 藉由執行此程序，您將會允許公司網路，以便向任何使用 Windows 整合式驗證的伺服器陣列中的同盟伺服器的用戶端。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動設定同盟伺服器陣列的 服務帳戶](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|在即將成為同盟伺服器電腦上安裝 Federation Service 角色服務。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝 Federation Service 角色服務](Install-the-Federation-Service-Role-Service.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|設定 AD FS 軟體以扮演同盟伺服器角色使用 AD FS 同盟伺服器設定精靈在電腦上。<br /><br />遵循此程序，當您想要設定獨立\-單獨的同盟伺服器，建立新的伺服陣列中的第一部同盟伺服器，或將電腦加入現有的同盟伺服器陣列。 **注意：** 針對 同盟網頁單一登\-上\(SSO\)設計中，您必須在帳戶夥伴組織中的至少一部同盟伺服器和資源夥伴組織中的至少一部同盟伺服器。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立獨立同盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立同盟伺服器陣列中的第一部同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[將同盟伺服器新增至同盟伺服器陣列](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|\(選擇性\)使用 AD FS 管理嵌入式管理單元\-中加入和設定必要的 AD FS 憑證，才能部署您的設計。 如需有關加入或變更憑證使用嵌入式管理單元的時機\-中，請參閱[Certificate Requirements for Federation Servers](https://technet.microsoft.com/library/dd807040.aspx)。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖簽署憑證](Add-a-Token-Signing-Certificate.md)<br /><br />![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[新增權杖解密憑證](Add-a-Token-Decrypting-Certificate.md)<br /><br />![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|如果這是您組織中的第一個同盟伺服器，設定 Federation Service，使其符合您的 AD FS 設計。|![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定帳戶夥伴組織](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![設定同盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單：設定資源夥伴組織](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![設定同盟伺服器](media/icon_checkboxo.gif)|從用戶端電腦驗證同盟伺服器可運作。|![設定同盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[確認同盟伺服器是否正確運作](Verify-That-a-Federation-Server-Is-Operational.md)| 
