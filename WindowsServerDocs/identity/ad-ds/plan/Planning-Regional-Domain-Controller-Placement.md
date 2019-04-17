---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: "規劃區域的網域控制站位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>規劃區域的網域控制站位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要確保成本效率，計畫盡可能放置越少區域網域控制站。 首先，檢查」地理位置和通訊連結「(DSSTOPO_1.doc) 工作表中使用[收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)以判斷位置是否中心。  
  
若要將在每個中樞位置每個網域地區網域控制站計劃。 您區域網域控制站放所有中樞位置之後，評估附屬位置撥區域網域控制站的需求。 排除不必要的地區網域控制站衛星位置降低維護遠端伺服器基礎結構所需的支援成本。  
  
此外，確定實體網域控制站在中心] 和 [附屬位置的安全性，因此未經授權的人員不能存取它們。 不放寫入網域控制站中心] 和 [附屬位置無法保證實體網域控制站的安全性。 存取實體寫入網域控制站的人員可以攻擊系統：  
  
-   存取實體磁碟網域控制站在開始另一個作業系統。  
  
-   移除（也可以使用取代）網域控制站的實體磁碟。  
  
-   取得並操作一份網域控制站系統狀態備份。  
  
新增寫入地區網域控制站位置，您可以在其中保證實體安全性。  
  
在位置不當實體的安全性，以部署唯讀網域控制站 (RODC) 是建議的方案。 除了 account 的密碼，RODC 會保留所有的 Active Directory 物件和寫入網域控制站保留屬性。 不過，無法變更已儲存在 RODC 資料庫。 必須在 [寫入網域控制站和再複製到 RODC 變更。  
  
要驗證 client 登入和本機檔案伺服器的存取權，大部分的組織加上的所有區域網域中指定的位置表示區域網域控制站。 不過時，您必須考慮許多變數評估是否商務位置需要有 [本機驗證戶端或戶端可以賴以驗證和查詢寬區域 (wan) 的連結。 下圖顯示如何判斷是否放置網域控制站在衛星位置。  
  
![規劃區域俠位置](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>現場專業技術可用性  
基於各種原因持續管理需要網域控制站。 只是在包含人員可以管理網域控制站或務必網域控制站可以從遠端管理的位置地區網域控制站的地方。  
  
通常不佳實體安全性分支 office 環境和小資訊技術知識的人員，將部署 RODC 通常會建議的方案。 不需要任何網域或其他網域控制站的使用者權限授與使用者 RODC 本機系統管理員權限可以委派給任何網域使用者。 這可讓本機分支使用者登入 RODC 並執行的工作維護伺服器，例如升級驅動程式。 不過，分支使用者無法登入其他網域控制站或執行網域中的任何其他管理工作。 如此一來，可以分支使用者委派有效的網域或樹系的其他安全性危害管理分公司 RODC 的能力。  
  
## <a name="wan-link-availability"></a>WAN 連結可用性  
WAN 連結經常中斷的位置，不包含可以驗證使用者的網域控制站如果使用者導致重大生產力遺失。 如果您 WAN 連結可用性不是 100%您遠端網站無法容許服務中斷，將地區網域控制站放的位置使用者位置需要登入或交換存取伺服器 WAN 連結向時的能力。  
  
## <a name="authentication-availability"></a>驗證可用性  
例如銀行，某些組織需要驗證的使用者，在所有的時間。 地區網域控制站置於 WAN 連結可用性不是 100%，但使用者需要驗證隨時的位置。  
  
## <a name="logon-performance-over-wan-links"></a>登入效能透過 WAN 連結  
如果您 WAN 連結可用性可靠，位置撥網域控制站需求而定登入效能 WAN 連結。 登入效能影響透過 WAN 因素包含連結速度和可用的頻寬的使用者並登入網路流量複寫資料傳輸與使用的設定檔，數字。  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>WAN 連結速度和頻寬使用量  
活動的單一使用者可以 congest 保守型 WAN 連結。 如果無法接受 WAN 連結登入效能，置於位置網域控制站。  
  
頻寬利用平均百分比表示如何壅塞網路的連結。 如果網路連結平均頻寬使用量大於可接受的值，將網域控制站在該位置。  
  
### <a name="number-of-users-and-usage-profiles"></a>數字的使用者與使用方式設定檔  
使用者，他們使用的設定檔指定的位置，可協助您判斷是否需要放置地區的網域控制站在這個位置。 若要避免 WAN 連結失敗生產力遺失，放置地區網域控制站 100 或多個使用者的位置。  
  
使用的設定檔表示使用者如何使用的網路資源。 您不需要將網域控制站包含只有少數使用者通常不會存取網路資源位置中。  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>與複寫流量登入網路流量  
如果無法為 Active Directory client 在同一個位置中使用的網域控制站，client 會在網路上建立流量登入。 由數個原因，包括群組成員資格; 受影響的實體網路建立的登入網路流量數字與大小，群組原則物件 (Gpo)。登入指令碼。和功能，例如離線資料夾、資料夾重新導向，及漫遊設定檔。  
  
手動，會放在特定位置的網域控制站產生複寫網路上的資料傳輸。 頻率及更新的磁碟分割上的網域控制站的影響網路建立的複寫傳輸的量。 不同可的磁碟分割上的網域控制站的更新類型包括新增或變更使用者和使用者屬性、變更密碼，以及新增或變更的全域群組、印表機或磁碟區。  
  
若要判斷您需要將地區網域控制站放在一個位置，比較建立網域控制站的成本不複寫流量由位置撥網域控制站的位置登入流量的費用。  
  
例如，考慮分公司透過保守型連結到總部，可以輕鬆加入的網域控制站在連接的網路。 如果每日登入及 directory 查詢流量的幾個遠端網站使用者造成更多的網路流量比分支複寫所有公司資料，請考慮將新加入的網域控制站。  
  
如果降低成本維護網域控制站的網路流量比重要，網域控制站的集中管理和執行不將任何區域網域控制站的位置或考慮位置撥 Rodc。  
  
試算表中列出的位置區域網域控制站以及的每個網域都會在每個位置的使用者來協助您，會看到工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, and 打開 (DSSTOPO_4.doc)」網域控制站位置]。  
  
您將需要指向您要部署區域網域時放置地區的網域控制站的位置的相關資訊。 如需部署網域區域的相關資訊，請查看[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)。  
  


