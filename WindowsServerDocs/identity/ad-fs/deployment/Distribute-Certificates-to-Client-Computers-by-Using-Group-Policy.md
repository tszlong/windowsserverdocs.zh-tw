---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: "使用群組原則來散發憑證 Client 的電腦"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則來散發憑證 Client 的電腦

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


您可以使用下列程序推入適當的安全通訊端層 \(SSL\) 憑證 \（或相當於憑證鏈結該受信任的 root\）account 聯盟伺服器、資源聯盟伺服器，與每個 client 的電腦使用群組原則來 account 合作夥伴森林中的網頁伺服器。  
  
在成員資格**網域系統管理員**或**企業系統管理員 」**，或相當於，在 Active Directory Domain Services \(AD DS\) 的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>將憑證 client 的電腦使用群組原則  
  
1.  Account 合作夥伴公司的樹系的網域控制站，在 [開始]**群組原則管理**snap\ 中。  
  
2.  尋找現有的群組原則物件 \(GPO\) 或建立新的 GPO 包含憑證設定。 確保相關聯的網域、網站或組織單位 GPO \(OU\) 適當的使用者及電腦帳號所在的位置。  
  
3.  Right\ 按一下 GPO，然後再按一下**編輯**。  
  
4.  在主控台開放**電腦 Configuration\\Policies\\Windows Settings\\Security Settings\\Public 原則**，right\ 按**受信任的根憑證授權單位**，，然後按一下 [**匯入**。  
  
5.  在**歡迎精靈憑證匯入**頁面上，按一下 [**下**。  
  
6.  在**匯入檔案**頁面上，輸入適當的憑證檔案的路徑 \ (例如，\\\fs1\\c$\\fs1.cer\)，然後按一下 [**下**。  
  
7.  在**憑證存放區**頁面上，按一下 [**將所有憑證都放在市集中下列**，然後按一下 [**下一步**。  
  
8.  在**完成精靈憑證匯入**頁面，確認使用正確，您所提供的資訊，然後按**完成]**。  
  
9. 重複步驟 2 透過新增額外的憑證每個聯盟伺服器 6。  
