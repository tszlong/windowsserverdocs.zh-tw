---
title: AD 樹系復原-設定 DNS 伺服器服務
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842959"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>AD 樹系復原-設定 DNS 伺服器服務

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

如果您從備份還原的 DC 上未安裝 DNS 伺服器角色，您必須安裝並設定 DNS 伺服器。 

## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務

針對每個還原網域控制站不應執行的 DNS 伺服器完成還原之後完成此步驟。 

> [!NOTE]
> 如果您從備份還原的 DC 正在執行 Windows Server 2008 R2，您必須到隔離的網路連接的 DC，才能安裝 DNS 伺服器項目。 然後彼此共用的隔離網路來連接每個已還原的 DNS 伺服器。 執行 repadmin /replsum，若要確認已還原的 DNS 伺服器之間的複寫正常運作。 確認複寫之後，如果已安裝 DNS 伺服器角色，您可以到實際執行網路連線已還原的網域控制站，您可以套用 hotfix 可讓您啟動伺服器未連線到任何網路的 DNS 伺服器。 您應該在您的自動化的建置程序期間，將 hotfix 匯集到作業系統的安裝映像。 如需有關 hotfix 的詳細資訊，請參閱 <<c0> [ 文章 975654](https://go.microsoft.com/fwlink/?LinkId=184691)在 Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=184691)。 

完成安裝和設定步驟。

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>安裝及使用伺服器管理員的 DNS 伺服器服務  

1. 開啟伺服器管理員，然後按一下**新增角色及功能**。 
2. 在 [新增角色精靈] 中，如果出現 [**在您開始前**] 頁面，請按 [**下一步**]。 
3. 在 **安裝類型**畫面上，選取**以角色為基礎或功能安裝**，按一下 **下一步**。
4. 在 [**伺服器選取項目**畫面中選取伺服器，然後按一下**下一步]**。
5. 上**伺服器角色**畫面上，選取**DNS 伺服器**，如果系統提示您按一下 [**新增功能**然後按一下**下一步]**。
6. 在 [**功能**畫面上，按一下**下一步]**。
7. 在讀取資訊**DNS 伺服器**頁面，然後再按一下**下一步**。
   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. 在 **確認**頁面上，確認 DNS 伺服器角色將會安裝，然後按一下**安裝**。 

### <a name="to-configure-the-dns-server-service"></a>若要設定 DNS 伺服器服務

1. 開啟 伺服器管理員中，按一下**工具**然後按一下**DNS**。
   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. 嚴重的故障前的 DNS 伺服器上建立相同的 DNS 網域名稱裝載 DNS 區域。 如需詳細資訊，請參閱 < 新增正向對應區域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。
3. 重要的問題之前就存在，請設定 DNS 資料。 例如:   

   - 設定儲存在 AD DS 的 DNS 區域。 如需詳細資訊，請參閱變更區域類型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。
   - 設定 DNS 區域的網域控制站定位程式 （DC 定位程式） 資源記錄以允許安全動態更新授權。 如需詳細資訊，請參閱允許只安全動態更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。

4. 確認父系 DNS 區域包含委派的資源記錄 （名稱伺服器 (NS) 」 和 「 黏附主機 (A) 資源記錄） 此 DNS 伺服器裝載子區域。 如需詳細資訊，請參閱 < 建立區域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。
5. 設定 DNS 之後，您可以加速 NETLOGON 記錄的登錄。

   > [!NOTE]
   > 安全動態更新只能在通用類別目錄伺服器可用時才運作。 

   在命令提示字元中輸入下列命令，然後按 ENTER：  

   **net stop netlogon**  

6. 輸入下列命令，然後按 ENTER：  

   **net start netlogon&lt**  

   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
