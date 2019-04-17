---
title: 安裝憑證授權單位
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>安裝憑證授權單位

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，讓您可以註冊伺服器的憑證伺服器的網路原則 Server (NPS)、 路由並遠端存取服務 (RRAS) 或兩者執行安裝 Active Directory 憑證 Services (AD CS)。  
  
> [!IMPORTANT]  
> -   Active Directory 憑證服務安裝之前，您必須為電腦、 靜態 IP 位址，以設定電腦和加入網域的電腦。 如需如何完成這些工作，查看 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要執行此程序，安裝 AD CS 的電腦必須加入的網域裝有 Active Directory Domain Services (AD DS)。  
  
同時成員資格**企業系統管理員**並根網域的**網域系統管理員」**群組是才能完成此程序最小值。  
  
> [!NOTE]  
> 使用 Windows PowerShell 來執行這個程序，開放 Windows PowerShell 中輸入下列命令，並再按下 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> AD CS 安裝之後，輸入下列命令，然後按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>若要安裝 Active Directory 憑證服務  
  
1.  登入的群組企業系統管理員和根網域的網域管理群組成員。  
  
2.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。  
  
3.  在**在您開始之前，請先**，按一下 [**下**。  
  
    > [!NOTE]  
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。  
  
4.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。  
  
5.  在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。  
  
6.  在**選取伺服器角色**，請在**角色**、 選取**Active Directory 憑證服務**。 當您接到新增所需的功能時，請按一下**新增功能**，然後按一下 [**下一步**。  
  
7.  在**選擇功能**，按一下 [**下**。  
  
8.  在**Active Directory 憑證服務**，讀取提供的資訊，然後按**下**。  
  
9. 在**確認安裝選項**，按一下 [**安裝**。 不要安裝程序期間關閉精靈。 安裝完成時，請按一下**設定 Active Directory 憑證服務目的伺服器在**。 AD CS 設定精靈開啟。 朗讀憑證的資訊並視需要提供的認證針對為企業系統管理員群組成員。 按一下**下一步**。  
  
10. 在**角色服務**，按一下 [**憑證授權單位**，然後按一下 [**下一步**。  
  
11. 在**安裝類型**頁面上，確認**企業 CA**已選取，然後按一下 [**下一步**。  
  
12. 在**指定類型 CA 的**頁面上，確認**根 CA**已選取，然後按一下 [**下一步**。  
  
13. 在**指定類型私密金鑰的**頁面上，確認**建立新的私密金鑰**已選取，然後按一下 [**下一步**。  
  
14. 在**ca 密碼編譯**頁面中，保留 CSP 預設設定 (**RSA #Microsoft 軟體金鑰儲存提供者**) 和 hash 演算法 (**SHA1**)，並判斷您的部署的最佳鍵字元長度。 大鍵字元長度提供獲得最佳的安全性。不過，他們可能會影響伺服器的效能，可能無法與舊版應用程式相容。 建議您保留 2048年的預設設定。 按一下**下一步**。  
  
15. 在**CA 名稱**頁面上，繼續憑證授權單位建議一般的名稱，或變更名稱根據您的需求。 確定您已特定 CA 名稱是相容命名規格與用途，因為您已經安裝 AD CS 之後，您無法變更 CA 名稱。 按一下**下一步**。  
  
16. 在**有效期**頁面上，在**指定有效期**，輸入數字，然後選取時間的值 （年、 個月，星期或日）。 建議使用預設設定，5 年。 按一下**下一步**。  
  
17. 在**CA 資料庫**頁面上，在**指定位置資料庫**，指定資料夾的位置憑證資料庫和憑證資料庫登入。 若您指定位置以外的預設位置，確保資料夾的安全存取控制清單 (Acl)，以防止未經授權的使用者或電腦存取 CA 資料庫並登入檔案使用。 按一下**下一步**。  
  
18. 在**確認**，按一下 [**設定**以適用於您的選項，然後按**關閉**。  
  


