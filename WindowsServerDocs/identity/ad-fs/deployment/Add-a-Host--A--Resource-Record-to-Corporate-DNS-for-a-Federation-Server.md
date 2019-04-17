---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: "新增至企業 DNS 主機 (A) 資源記錄聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>新增至企業 DNS 主機 (A) 資源記錄聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


在公司戶端成功存取網路使用 Windows 整合式驗證聯盟伺服器，必須先在解析主機伺服器的名稱 account 聯盟公司網域名稱系統 \(DNS\) 建立一主機 \(A\) 資源筆資料 \ (例如，fs.fabrikam.com\) 聯盟伺服器叢集或聯盟伺服器的 IP 位址。 您可以將主機 \(A\) 資源記錄新增至企業 DNS 伺服器聯盟使用下列程序。  
  
資格在**系統管理員**，或相當於，才能完成此程序最小值。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>若要新增至企業 DNS 伺服器聯盟主機 \(A\) 資源記錄  
  
1.  在公司網路的 DNS 伺服器，開放 DNS snap\ 中。  
  
2.  在主機中樹狀結構 right\ 按一下適用的正向對應區域，，然後按一下**新主機 \(A or AAAA\)**。  
  
3.  在**名稱**，輸入只聯盟伺服器或聯盟伺服器叢集; 的電腦名稱例如的完整網域名稱 \(FQDN\) fs.fabrikam.com，輸入**fs**。  
  
4.  在**的 IP 位址**，輸入聯盟伺服器叢集，例如 192.168.1.4 或聯盟伺服器的 IP 位址。  
  
5.  按一下**新增主機**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[聯盟伺服器的名稱解析需求](https://technet.microsoft.com/library/dd807055.aspx)  
  

