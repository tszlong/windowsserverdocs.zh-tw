---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: "檢查清單-設定聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 44448088184d86874e91855d8a51ef40d8cea049
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-setting-up-a-federation-server"></a>檢查清單︰ 設定聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

檢查此清單包含部署工作所需準備聯盟伺服器角色 Active Directory 同盟服務 \(AD FS\) 在執行 Windows Server® 2012 年的伺服器。  
  
> [!NOTE]  
> 完成此訂單中的檢查清單中的工作。 當參考連結可讓您的程序時，返回本主題之後在您完成該程序中的步驟操作，以便您可以繼續檢查清單中的其餘的工作。  
  
![設定聯盟伺服器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**檢查清單︰ 設定聯盟伺服器**  
  
||工作|參考資料|  
|-|--------|-------------|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|部署 AD FS 聯盟伺服器在您開始之前，先檢視。1。\) 優點和缺點選擇 [Windows 內部資料庫 \(WID\) 或 SQL Server 儲存 AD FS 設定資料庫 2。\) AD FS 部署拓撲類型與他們相關聯的伺服器位置與網路的版面配置建議。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[判斷您 AD FS 部署拓撲](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓撲注意事項](https://technet.microsoft.com/library/gg982489.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|檢查 AD FS 容量規劃指導方針，以判斷您應該 production 環境中使用聯盟伺服器的適當的數字。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟伺服器容量的計劃](https://technet.microsoft.com/library/gg749917.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|檢查 AD FS 程式設計指南聯盟伺服器放置在組織中相關的資訊|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟計畫伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[放置聯盟伺服器的位置](https://technet.microsoft.com/library/dd807127.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|只 stand\ 聯盟伺服器或聯盟伺服器陣列適合您的部署。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[當建立聯盟伺服器](https://technet.microsoft.com/library/dd807101.aspx)<br /><br />![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[當建立聯盟伺服器陣列](https://technet.microsoft.com/library/dd807062.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|判斷 account 合作夥伴公司或資源合作夥伴組織中是否要建立這個新的聯盟伺服器。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢視中 Account 合作夥伴聯盟伺服器角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[檢視的資源合作夥伴聯盟伺服器角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|檢視聯盟伺服器如何使用服務通訊憑證與 token\ 簽署憑證來安全驗證 client 和聯盟伺服器 proxy 要求的相關資訊。 **注意：**長已經常見主機不完整的名稱，例如 https:\/\/myserver 使用憑證的方式，但這些憑證不有任何安全性值和可以讓攻擊模擬企業戶端 AD FS 聯盟服務。 因此，建議您使用的完整的網域名稱 \(FQDN\) 例如 https:\/\/myserver.contoso.com 和只能使用 SSL 發給同盟服務的 FQDN。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|檢視更新網域名稱系統 \(DNS\) 企業網路，使成功名稱解析聯盟伺服器可能會發生的方式的相關資訊。|![設定聯盟伺服器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[聯盟伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|加入會成為聯盟伺服器中 account 合作夥伴樹系或是資源合作夥伴它用來驗證或來自信任的樹系的樹系的使用者網域的電腦。 **注意：**如果您想要聯盟伺服器 account 合作夥伴組織中設定，電腦必須先加入位置將會使用您的聯盟伺服器驗證使用者的樹系或來自信任的樹系森林中的任何網域。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[加入網域的電腦](Join-a-Computer-to-a-Domain.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|建立新的資源記錄公司網路的 DNS 伺服器聯盟的 DNS 名稱主機指向聯盟伺服器的 IP 位址。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增主機和 #40;A 與 #41;企業的 DNS 伺服器聯盟資源記錄](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|如果您將加入聯盟伺服器聯盟伺服器陣列 \(Optional\)，您可能需要先匯出現有 token\ 簽署的憑證的私密金鑰 \（farm\ 在第一次聯盟伺服器）上，您已準備好憑證的檔案格式時其他聯盟伺服器必須匯入相同的憑證。<br /><br />匯出私密金鑰時，不需要您發出的伺服器驗證憑證，即可重複使用多部電腦 \（而不需要 export\）或當您將會取得的唯一伺服器驗證憑證陣列中每個聯盟伺服器。 **注意：** AD FS 管理 snap\ 中指的是伺服器驗證憑證的同盟服務通訊的憑證以的伺服器。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯出伺服器驗證憑證的私人鍵部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|取得伺服器驗證憑證之後 \(or private key\) 憑證授權單位 \(CA\)，您必須再匯入憑證檔案的每個聯盟伺服器的預設網站。 **注意：**安裝這個憑證預設網站上，才能使用 AD FS 聯盟伺服器設定精靈是需求。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[匯入伺服器驗證憑證的預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|取得伺服器驗證憑證 CA 從另一種 \(Optional\)，您可以使用網際網路資訊服務 \(IIS\) 聯盟伺服器建立範例憑證。 **注意：**不安全的最佳做法部署聯盟伺服器 production 環境中的使用簽署 self\ 伺服器驗證憑證。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：建立 Self\-Signed 伺服器的憑證](https://go.microsoft.com/fwlink/?LinkID=108271)，然後完成程序[匯入伺服器驗證憑證的預設網站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|如果您將會 account 合作夥伴組織中設定伺服器聯盟農場環境，您必須建立和專用的服務 account 設定在 Active Directory Domain Services \(AD DS\) 位置發電廠會位於並設定每個聯盟伺服器中使用此帳號發電廠。 來執行這個程序，您將會向任何中使用 Windows 整合驗證發電廠聯盟伺服器的驗證的企業網路允許戶端。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手動設定服務 Account 聯盟伺服器陣列](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|在的電腦將會變成聯盟伺服器上安裝同盟服務角色。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安裝同盟服務的角色](Install-the-Federation-Service-Role-Service.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|AD FS 軟體的電腦上設定，請使用 AD FS 聯盟伺服器設定精靈做聯盟伺服器角色。<br /><br />當您想要設定 stand\ 只聯盟伺服器、建立新的發電廠的第一個聯盟伺服器，或將電腦加入現有的聯盟伺服器發電廠，請執行此程序。 **注意：**的聯盟網路單一 Sign\-On \(SSO\) 設計，您必須有至少一個聯盟伺服器 account 合作夥伴組織和至少一個聯盟伺服器資源合作夥伴組織中的。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[建立獨立聯盟伺服器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[第一個聯盟伺服器建立聯盟伺服器陣列](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[新增至聯盟伺服器陣列聯盟伺服器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|\(Optional\) 使用 AD FS 管理 snap\ 中新增及設定所需的 AD FS 憑證部署所需您的設計。 如需有關何時要新增或變更 snap\ 中使用的憑證的詳細資訊，請查看[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)。|![設定聯盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[加入權杖簽署的憑證](Add-a-Token-Signing-Certificate.md)<br /><br />![設定聯盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[加入權杖解密憑證](Add-a-Token-Decrypting-Certificate.md)<br /><br />![設定聯盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[設定服務通訊憑證](Set-a-Service-Communications-Certificate.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|如果這是您在組織中的第一個聯盟伺服器，設定同盟服務，使其符合您 AD FS 設計。|![設定聯盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定 Account 合作夥伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![設定聯盟伺服器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[檢查清單︰ 設定資源合作夥伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![聯盟伺服器設定](media/icon_checkboxo.gif)|從 client 的電腦，請確認聯盟伺服器操作。|![設定聯盟伺服器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[確認聯盟伺服器是操作](Verify-That-a-Federation-Server-Is-Operational.md)| 
