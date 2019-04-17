---
title: 在 [DNS 建立 WEB1 別名 (CNAME) 記錄
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>在 [DNS 建立 WEB1 別名 \(CNAME\) 記錄

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以在您的網域控制站 DNS 在時區新增別名正式名稱 \(CNAME\) 資源記錄網頁伺服器使用此程序。 CNAME 記錄時，您可以使用多個名稱指向單一主機，讓您輕鬆地執行等主機相同的電腦上的網頁伺服器和檔案傳輸通訊協定 \(FTP\) 伺服器。   
  
因為您的免費使用您的網頁伺服器管理憑證撤銷您憑證授權單位清單 \(CRL\) \(CA\)，以及執行其他服務，例如 FTP 或網頁伺服器至於。  
  
當您執行此程序時，來取代**別名**及其他適用於您的部署的值與變數。  
  
若要執行此程序，您必須成員的**網域系統管理員**。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>若要新增至區域別名 \(CNAME\) 資源記錄  
  
>[!NOTE]  
>使用 Windows PowerShell 來執行這個程序，請查看[新增-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1，在伺服器管理員中，按一下**工具**，然後按一下 [ **DNS**。 開啟 DNS 管理員 Microsoft Management Console (MMC)。  
  
2.  在主控台按兩下 [**正向對應區域**，以滑鼠右鍵按一下您要新增別名資源記錄]，然後按一下 [轉寄對應區域**新別名 \(CNAME\)**。 **新資源記錄**對話方塊。  
  
3.  在**別名**，輸入別名**pki**。  
  
4.  當您輸入的值**別名**、**完整的網域名稱 \(FQDN\)**對話方塊中自動填滿。 例如，如果您別名「pki「與您的網域是 corp.contoso.com，值**pki.corp.contoso.com**是為您自動填入。  
  
5.  在**完整的網域名稱 \(FQDN\) 目標主機的**，輸入您的網頁伺服器的 FQDN。 例如，如果您的網頁伺服器稱為 WEB1 且您的網域 corp.contoso.com，輸入**WEB1.corp.contoso.com**。  
  
6.  按一下**[確定]**來新增到區域記錄。  
  

