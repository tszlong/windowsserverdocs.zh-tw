---
title: 在 DNS 中為 WEB1 建立別名 (CNAME) 記錄
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813159"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>建立別名\(CNAME\) WEB1 dns 記錄

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序新增標準的別名\(CNAME\)您的 Web 伺服器，以在 DNS 區域，您的網域控制站上的資源記錄。 CNAME 記錄時，您可以使用多個名稱指向單一主機，方便您進行這類動作，為這兩個主機檔案傳輸通訊協定\(FTP\)伺服器和 Web 伺服器在同一部電腦上的。   
  
基於這個原因，您可以自由使用您的 Web 伺服器裝載的憑證撤銷清單\(CRL\)您的憑證授權單位\(CA\)以及執行其他服務，例如 FTP 或 Web 伺服器。  
  
當您執行此程序時，來取代**別名**和其他變數值，適用於您的部署。  
  
若要執行此程序，您必須隸屬**Domain Admins**。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>若要新增的別名\(CNAME\)區域資源記錄  
  
>[!NOTE]  
>若要使用 Windows PowerShell 執行此程序，請參閱[新增 DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1 上，在 [伺服器管理員] 中，按一下**工具**，然後按一下**DNS**。 此時會開啟 DNS 管理員 Microsoft Management Console (MMC)。  
  
2.  在主控台樹狀目錄中，按兩下**正向對應區域**，以滑鼠右鍵按一下您要新增別名的資源記錄，然後再按正向對應區域**新別名\(CNAME\)** . **新的資源記錄**對話方塊隨即開啟。  
  
3.  在 **別名**，輸入別名名稱**pki**。  
  
4.  當您輸入的值**別名**，則**完整的網域名稱\(FQDN\)** 自動填滿對話方塊中。 例如，如果您的別名名稱是"pki 」 與您的網域是 corp.contoso.com，值**pki.corp.contoso.com**是為您自動填入。  
  
5.  在 **完整的網域名稱\(FQDN\)目標主機**，輸入您的 Web 伺服器的 FQDN。 例如，如果您的 Web 伺服器名為 WEB1，而且您的網域是 corp.contoso.com，輸入**WEB1.corp.contoso.com**。  
  
6.  按一下 **確定**將新的記錄新增至區域。  
  

