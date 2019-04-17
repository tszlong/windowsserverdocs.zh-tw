---
title: "廣告樹系復原設定的 DNS 伺服器服務"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>廣告樹系修復-設定 DNS 伺服器服務 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
如果未安裝在您從備份還原網域控制站的 DNS 伺服器角色，您必須安裝和設定的 DNS 伺服器。  
  

## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定的 DNS 伺服器服務  
針對每個不是執行為 DNS 伺服器還原完成後還原網域控制站完成這個步驟。  
  
> [!NOTE]
>  如果您已從備份還原俠執行的 Windows Server 2008 R2，您必須連接隔離網路 DC 以安裝 DNS 伺服器。 然後將每個還原 DNS 伺服器連接到互相共用、隔離的網路。 執行 repadmin /replsum 驗證複寫運作之間還原的 DNS 伺服器。 您確認複寫之後，您可以還原網域控制站連接到 production 網路如果已安裝的 DNS 伺服器角色，您可以將 DNS 伺服器開始伺服器未連接到任何網路時，可讓 hotfix 套用。 在您的自動化的建置程序期間，您應該在作業系統安裝映像增加 hotfix。 如需 hotfix 的詳細資訊，請查看[文章 975654](https://go.microsoft.com/fwlink/?LinkId=184691)中「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=184691)。 

完成安裝和設定步驟。
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>安裝和使用伺服器管理員 DNS 伺服器服務  
  
1.  打開伺服器管理員中，按一下 [**新增角色與功能**。  
2.  在 [新增角色精靈，如果**在您開始之前，請先**頁面上，按一下**下一步**。  
3.  在**安裝類型**畫面選取**以角色為基礎的功能或安裝**，按一下 [**下一步**。
4.  在**選擇伺服器**選取伺服器] 畫面，然後按一下 [**下**。
5.  在**伺服器角色**畫面選取**DNS 伺服器**如果系統提示按一下，**新增功能**按一下**下一步**。
6.  在**功能**畫面上按一下**下**。
7.  在朗讀資訊**的 DNS 伺服器**頁面，然後按一下 [**下**。
![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  在**確認**頁面上，確認 DNS 伺服器角色將會安裝，然後按**安裝**。  
  
     
### <a name="to-configure-the-dns-server-service"></a>若要設定的 DNS 伺服器服務 
1.  打開伺服器管理員中，按一下**工具**，按一下 [ **DNS**。
![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  重要故障之前的 DNS 伺服器建立已裝載的相同 DNS 網域名稱 DNS 區域。 如需詳細資訊，查看 [新增正向對應區域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
3.  設定存在之前重要故障 DNS 資料。 例如：  
  
    -   設定，並儲存在 AD DS DNS 區域。 如需詳細資訊，請變更區域類型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
  
    -   設定的網域控制站定位器 （DC 定位） 以允許安全的動態更新的資源記錄授權 DNS 區域。 如需詳細資訊，查看 [允許只有安全動態更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  
  
4. 確定家長 DNS 區域包含委派資源記錄 （伺服器 （奈秒） 和名稱黏附主機的資源 (A) 記錄） 子女區此 DNS 伺服器上。 如需詳細資訊，請建立區域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
5. 設定 DNS 之後，您可以加速的 NETLOGON 記錄登記。  
  
    > [!NOTE]
    >  安全的動態更新只工作時使用通用伺服器。  
  
     在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
     **網路停止 netlogon**  
  
6. 輸入下列命令，並按一下 ENTER:  
  
     **網路的 [開始] 畫面 netlogon**  

![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
