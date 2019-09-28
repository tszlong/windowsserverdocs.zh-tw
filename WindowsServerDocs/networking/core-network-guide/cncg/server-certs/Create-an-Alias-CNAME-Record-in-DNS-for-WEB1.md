---
title: 在 DNS 中為 WEB1 建立別名 (CNAME) 記錄
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 77b8e464d2d8fab8803477e59826c3e715c0a6d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406292"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>在 WEB1 的 DNS 中建立別名 \(CNAME @ no__t-1 記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，將 Web 服務器的別名正式名稱 \(CNAME @ no__t-1 資源記錄新增至網域控制站上 DNS 中的區域。 使用 CNAME 記錄時，您可以使用多個名稱指向單一主機，讓您輕鬆地將檔案傳輸通訊協定 @no__t 0FTP @ no__t-1 伺服器和 Web 服務器裝載在同一部電腦上。   
  
因此，您可以免費使用 Web 服務器來裝載憑證授權單位單位的憑證撤銷清單 \(CRL @ no__t-1，\(CA @ no__t-3，以及執行其他服務，例如 FTP 或 Web 服務器。  
  
當您執行此程式時，請將**別名名稱**和其他變數取代為適用于您部署的值。  
  
若要執行此程式，您必須是**Domain Admins**的成員。  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>若要將別名 \(CNAME @ no__t-1 資源記錄新增至區域  
  
>[!NOTE]  
>若要使用 Windows PowerShell 執行此程式，請參閱[DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx)。  
  
1.  在 DC1 的伺服器管理員中，按一下 [**工具**]，然後按一下 [ **DNS**]。 DNS 管理員 Microsoft Management Console （MMC）隨即開啟。  
  
2.  在主控台樹中，按兩下 [**正向對應區域**]，以滑鼠右鍵按一下您要新增別名資源記錄的正向對應區域，然後按一下 [**新增別名 \(CNAME @ no__t-3**]。 [**新增資源記錄**] 對話方塊隨即開啟。  
  
3.  在 [**別名名稱**] 中，輸入別名名稱**pki**。  
  
4.  當您輸入 **別名名稱** 的值時，會在對話方塊中 **\(FQDN @ no__t-3**自動填滿 的完整功能變數名稱。 例如，如果您的別名名稱是 "pki"，而您的網域是 corp.contoso.com，則會為您自動填入值**pki.corp.contoso.com** 。  
  
5.  在 [**完整功能變數名稱 \(FQDN @ no__t-2 針對目標主機**] 中，輸入您的 Web 服務器的 FQDN。 例如，如果您的 Web 服務器名為 WEB1，而您的網域是 corp.contoso.com，請輸入**WEB1.corp.contoso.com**。  
  
6.  按一下 **[確定]** ，將新記錄新增至區域。  
  

