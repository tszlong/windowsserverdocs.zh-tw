---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: 建立站台設計
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819069"
---
# <a name="creating-a-site-design"></a>建立站台設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立站台設計包含決定哪些位置將變成站台，建立站台物件、 建立子網路的物件，以及子網路關聯至站台。  
  
## <a name="deciding-which-locations-will-become-sites"></a>決定哪些位置將變成站台

決定哪些位置建立站台的如下所示：  
  
- 建立您打算將網域控制站的所有位置而言的站台。 請參閱 「 設置網域控制站 」 (DSSTOPO_4.doc) 工作表來識別包含網域控制站的位置中所記載的資訊。  
- 建立包含執行應用程式需要建立站台伺服器的那些位置的站台。 某些應用程式，例如分散式檔案系統命名空間 (DFSN)，以找出最接近的伺服器到用戶端使用站台物件。  

   > [!NOTE]  
   > 如果您的組織有多個網路具有快速、 可靠的連線非常接近，您可以將這些網路的子網路的所有併入單一 Active Directory 站台。 比方說，如果反覆存取傳回網路中不同的兩部伺服器之間的延遲的子網路是 10 毫秒或更少，您可以在相同的 Active Directory 站台包含這兩個子網路。 如果兩個位置之間的網路延遲大於 10 毫秒，則您不應包含子網路的單一 Active Directory 站台中。 即使延遲為 10 毫秒或更少，您可以選擇要部署不同的站台，如果您想要分割的 Active Directory 為基礎的應用程式的站台之間的流量。  

- 如果網站不需要的位置，新增站台位置具有的最大的廣域網路 (wan) 速度和可用的頻寬位置的子網路。  
  
會變成站台的網路位址和子網路遮罩，每個位置內的文件位置。 為協助您記錄網站的工作表，請參閱[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，，然後開啟 「 建立關聯的子網路站台 」 (DSSTOPO_6.doc)。  
  
## <a name="creating-a-site-object-design"></a>建立站台物件設計

您已決定建立站台每個位置，計劃建立 Active Directory 網域服務 (AD DS) 中的站台物件。 將變成 「 建立關聯的子網路與網站 」 工作表中的站台的文件位置。  
  
如需如何建立站台物件的詳細資訊，請參閱文章[建立站台](https://go.microsoft.com/fwlink/?LinkId=107067)。  
  
## <a name="creating-a-subnet-object-design"></a>建立子網路物件設計

每個 IP 子網路和子網路遮罩與每個位置相關聯，計劃來代表站台內的所有 IP 位址的 AD DS 中建立子網路物件。  
  
建立 Active Directory 子網路物件時，網路 IP 子網路和子網路遮罩的相關資訊會自動轉譯成網路首碼長度標記法格式<IP address> / <prefix length>。 例如，網路 IP version 4 (IPv4) 位址 172.16.4.0 子網路遮罩為 255.255.252.0 會顯示為 172.16.4.0/22。 除了 IPv4 位址，Windows Server 2008 也支援 IP 版本 6 (IPv6) 子網路前置詞，例如 3FFE:FFFF:0:C000:: / 64。 如需有關每個位置中的 IP 子網路的詳細資訊，請參閱中的 「 位置和子網路 」 (DSSTOPO_2.doc) 工作表[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)和[附錄 a:位置和子網路首碼](Appendix-A--Locations-and-Subnet-Prefixes.md)。  
  
關聯與站台物件參考 」 建立關聯的子網路與站台 」 (DSSTOPO_6.doc) 工作表來判斷哪一個子網路的 「 決定其位置會變成網站 」 一節中每一個子網路物件是要與哪個站台相關聯。 Active Directory 子網路物件相關聯 」 建立關聯的子網路與站台 」 (DSSTOPO_6.doc) 工作表中的每個位置的文件。  
  
如需如何建立子網路物件的詳細資訊，請參閱文章[建立子網路](https://go.microsoft.com/fwlink/?LinkId=107068)。
