---
title: Windows Server 2016 中的系統服務的安全性指導方針
description: 關於在 Windows Server 2016 (含桌面體驗) 中停用服務的安全性指導方針
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 323985cf316bda2fa6ab6a1721e2b6316450391a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844469"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>停用 含有桌面體驗的 Windows Server 2016 上的系統服務的指引

適用於：Windows Server 2016

Windows 作業系統包含許多的系統服務可提供重要的功能。 不同的服務有不同的預設啟動原則： 一些預設會啟動 （自動），有些時所需 （手動），以及一些預設會停用，且必須在可以執行之前加以明確啟用。 這些預設值已仔細選擇每個服務來平衡一般客戶的效能、 功能和安全性。

不過，某些企業客戶可能會偏好更以安全性為主的平衡，針對其 Windows 電腦和伺服器，可以降低攻擊的其中一個絕對的最小值，介面，並可能因此想要完全停用所有不需要在其特定的服務環境。 對於這些客戶，Microsoft® 提供哪些服務可以安全地停用針對此目的的隨附的指導方針。

本指南是僅針對 含有桌面體驗的 Windows Server 2016 （除非桌面取代為用於使用者）。 從 Windows Server 2019，預設會設定這些指導方針。 在系統上的每個服務的分類方式如下：

-   **應該停用：** 安全性為主的企業最有可能會想要停用此服務並放棄其功能 （請參閱下列的其他詳細資料）。
- **確定要停用：** 此服務會提供一些但非全部的企業，適合的功能，並不能用在安全性為主的企業可以安全地將它停用。
- **請勿停用：** 停用此服務，將會影響重要的功能，或防止特定角色或功能正確運作。 因此它應該不會停用。
-  **（沒有指引）：** 停用這些服務的影響不完全評估。 因此，不應該變更這些服務的預設組態。


客戶可以設定他們的 Windows 電腦和伺服器，若要停用所選的服務，其群組原則中使用安全性範本，或使用 PowerShell 自動化。 在某些情況下，本指南會包含直接停用服務的功能，或者停用服務本身的特定群組原則設定。

Microsoft 建議客戶停用下列服務和其各自的排程的工作，使用桌面體驗的 Windows Server 2016 上：

服務： 
1. Xbox Live 的驗證管理員
2. Xbox Live 的遊戲儲存

排定的工作： 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(您也可以存取此文件中詳述檢視附加的 Microsoft Excel 試算表的所有服務上的資訊：[停用 含有桌面體驗的 Windows Server 2016 上的系統服務的指引](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>停用服務預設不安裝

若要停用預設不會安裝的服務，Microsoft 建議針對套用原則。
-  如果安裝此功能，則通常會需要服務。 安裝服務或功能需要系統管理權限。 不允許功能安裝，而不是將服務啟動。
-  封鎖 Microsoft Windows 服務不會停止安裝類似的第三方對等，可能是一個較高的安全性風險的系統管理員 （或非系統管理員在某些情況下）。
-  比較基準或停用非預設的 Windows 服務 (例如，W3SVC) 的基準測試會提供一些稽核員誤用的印象技術 (例如 IIS) 原本就不安全，並請勿使用。
-  如果從未安裝的功能 （和服務），這只會新增不必要的大量基準，並驗證工作。

<br />
本文件中列出的所有系統服務，請依照下列兩個資料表會都提供啟用和停用 含有桌面體驗的 Windows Server 2016 中的系統服務的資料行和 Microsoft 建議說明： 
 
<br />

### <a name="explanation-of-columns"></a>資料行的說明

| | |
|---|---|
|**服務描述**|   服務的描述，從 sc.exe qdescription。|
|**名稱** |索引鍵 （內部） 的服務名稱|
|**安裝** |永遠都會安裝：服務是在 Server Core 與 Server 含桌面體驗  <br /> 只能與桌面體驗：服務位於 含有桌面體驗的 Windows Server 2016，但已***不***在 Server Core 上 |
|**StartType**  |Windows Server 2016 上的服務啟動類型|
|**建議** |Microsoft 建議/建議停用此服務在 Windows Server 2016 上一般又管理完善的企業部署中，其中伺服器未使用的使用者桌面取代為。|
|**註解** |其他說明|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Microsoft 建議的說明

| | |
|---|---|
|**請勿停用** |應該不會停用此服務|
|**若要停用 [確定]**| 如果未使用它所支援的功能，可以停用此服務。|
|**已停用**|  此服務會停用的預設值;不需要強制執行原則|
|**應該停用** |管理完善的企業系統上時，應該永遠不會啟用此服務。|

<br />

下表提供 Microsoft 的指導方針，停用 含有桌面體驗的 Windows Server 2016 上的系統服務：

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX 安裝程式 (AxInstSV)

| | |
|---|---|
|   **服務描述** |   提供來自網際網路的 ActiveX 控制項安裝的使用者帳戶控制驗證，並可讓您以群組原則設定為基礎的 ActiveX 控制項安裝的管理。 此服務在要求時啟動，並停用 ActiveX 控制項的安裝將會行為會根據預設瀏覽器設定。    |
|   **服務名稱**    |   AxInstSV    |
|   **安裝**    |   只含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  |   若要停用 [確定]   |
|   **註解**    |   確定要停用，如果不需要的功能 |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn 路由器服務   
| | |
|---|---|
|   **服務描述** |   本機 AllJoyn 用戶端的路由 AllJoyn 訊息。 如果此服務停止並沒有自己的配套的路由器 AllJoyn 用戶端將無法執行。 |
|   **服務名稱**    |   AJRouter    |
|   **安裝**    |   只含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  | 沒有指引       |
|   **註解**    |       |
| | |

<br />

## <a name="app-readiness"></a>應用程式整備
| | |
|---|---|
**服務描述** |   取得應用程式可供使用的使用者登入到此電腦，並新增新的應用程式時的第一次。
**服務名稱**    |   AppReadiness
**安裝**    |   只含桌面體驗
**StartType**   |   Manual
**建議**  |   請勿停用
**註解**    |   
| | |

<br />

##  <a name="application-identity"></a>應用程式身分識別
| | |       
|---|---|   
**服務描述** |   判斷和身分識別的應用程式。 停用此服務會防止 AppLocker 強制執行。
**服務名稱**    |   AppIDSvc
**安裝**    |   永遠都會安裝
**StartType**   |   Manual
**建議**  |沒有指引    
**註解**    |   
|||     

<br />

##  <a name="application-information"></a>應用程式資訊 
| | |       
|---|---|   
|   **服務描述** |   有助於進行互動式的應用程式使用額外的系統管理權限執行。  如果此服務停止時，使用者將無法啟動應用程式，以執行所需的使用者工作時，它們可能需要額外的系統管理權限。
|   **服務名稱**    |   應用程式資訊
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   支援 UAC 同一部桌上型電腦提高權限
|||     

<br />

##  <a name="application-layer-gateway-service"></a>應用程式層閘道服務       
| | |           
|---|---|           
|   **服務描述** |   提供支援網際網路連線共用的協力廠商通訊協定外掛程式
|   **服務名稱**    |   ALG
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |沒有指引    
|   **註解**    |   
|||     

<br />

##  <a name="application-management"></a>應用程式管理      
| | |           
|---|---|       
|   **服務描述** |   處理透過群組原則部署的軟體安裝、 移除和列舉型別要求。 如果服務已停用，使用者將無法安裝、 移除或列舉透過群組原則部署的軟體。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   AppMgmt
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>AppX 部署服務 (AppXSVC)       
| | |           
|---|---|
|   **服務描述** |   提供基礎結構支援部署儲存區應用程式。 在要求時啟動此服務，以及如果已停用的市集應用程式將不會部署到系統，可能無法正常運作。
|   **服務名稱**    |   AppXSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>自動時區更新程式           
| | |           
|---|---|           
|   **服務描述** |   會自動設定系統時區。
|   **服務名稱**    |   tzautoupdate
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>背景智慧型傳送服務          
| | |           
|---|---|   
|   **服務描述** |   傳輸檔案，在背景中使用閒置網路頻寬。 如果服務已停用，則取決於位元，例如 Windows 更新或 MSN 總管 中，任何應用程式將無法自動下載程式和其他資訊。
|   **服務名稱**    |   BITS
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>背景工作的基礎結構服務      
| | |           
|---|---|   
|   **服務描述** |   Windows 基礎結構服務，可控制哪個背景工作可以在系統上執行。
|   **服務名稱**    |   BrokerInfrastructure
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>篩選引擎的基底            
| | |           
|---|---|       
|   **服務描述** |   基礎篩選引擎 (BFE) 是一項服務來管理防火牆與網際網路通訊協定安全性 (IPsec) 原則，以及實作使用者模式下篩選。 停止或停用 BFE 服務將會大幅降低系統的安全性。 它也會導致無法預期的行為，IPsec 管理和防火牆的應用程式中。
|   **服務名稱**    |   BFE
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>藍芽支援服務            
| | |           
|---|---|   
|   **服務描述** |   藍牙服務支援探索和遠端的藍芽裝置的關聯。  停止或停用此服務可能會導致無法正常運作，並防止新的裝置被探索到，或相關聯的已安裝藍芽裝置。
|   **服務名稱**    |   bthserv
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   若要停用未使用的 [確定]。 另一個停用的機制： https://technet.microsoft.com/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **服務描述** |   此使用者的服務用於連接的裝置平台案例
|   **服務名稱**    |   CDPUserSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          


##  <a name="certificate-propagation"></a>憑證傳播     
| | |           
|---|---|
|   **服務描述** |   從智慧卡的使用者憑證和根憑證複製到目前的使用者憑證存放區，偵測到智慧卡插入智慧卡讀取裝置時，並在必要時，會安裝智慧卡隨插即用迷你驅動程式。
|   **服務名稱**    |   CertPropSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>用戶端授權服務 (ClipSVC)        
| | |           
|---|---|   
|   **服務描述** |   提供 Microsoft Store 的基礎結構支援。 在要求時啟動此服務，如果已停用的應用程式購買使用 Microsoft Store 將會無法正確運作。
|   **服務名稱**    |   ClipSVC
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>CNG 金鑰隔離
| | |           
|---|---|   
|   **服務描述** |   CNG 金鑰隔離服務裝載於 LSA 處理序。 服務提供私用金鑰和相關聯的密碼編譯作業所需的通用條件的索引鍵的處理序隔離。 服務會儲存，並在安全的程序，而且符合一般條件需求中使用長時間執行的索引鍵。
|   **服務名稱**    |   KeyIso
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>COM + 事件系統       
| | |           
|---|---|       
|   **服務描述** |   支援系統事件通知服務 (SENS)，可提供自動散發至訂閱元件物件模型 (COM) 元件的事件。 如果服務已停止，SENS 會關閉，而不能提供登入和登出的通知。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   EventSystem
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>COM + 系統應用程式     
| | |           
|---|---|       
|   **服務描述** |   管理組態和追蹤的元件物件模型 (COM) + 為基礎的元件。 如果服務已停止，大部分的 COM + 為基礎的元件將無法正確運作。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   COMSysApp
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>電腦瀏覽器        
| | |           
|---|---|       
|   **服務描述** |   會維護網路上電腦的更新的清單，並提供此清單，以做為瀏覽器所指定的電腦。 如果此服務停止時，此清單將會不更新或維護。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   瀏覽器
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>連接的裝置平台服務       
| | |           
|---|---|       
|   **服務描述** |   這項服務適用於已連接的裝置和通用的半透明效果的情況
|   **服務名稱**    |   CDPSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>已連線的使用者體驗與遙測     
| | |           
|---|---|       
|   **服務描述** |   已連線使用者體驗與遙測服務可讓支援中的應用程式，而且已連線使用者體驗的功能。 此外，此服務會管理事件驅動的收集及傳輸診斷和使用方式資訊 （用來改善體驗和品質的 Windows 平台） 的診斷和使用方式的隱私權選項設定底下啟用時意見反應與診斷。
|   **服務名稱**    |   DiagTrack
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>請連絡資料        
| | |           
|---|---|       
|   **服務描述** |   索引用於快速連絡搜尋連絡資料。 如果您停止或停用這項服務，連絡人可能會遺失您的搜尋結果。
|   **服務名稱**    |   PimIndexMaintenanceSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **服務描述** |   管理系統元件之間的通訊。
|   **服務名稱**    |   CoreMessagingRegistrar
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>認證管理員           
| | |           
|---|---|       
|   **服務描述** |   提供給使用者、 應用程式和安全性服務套件的安全儲存和擷取認證。
|   **服務名稱**    |   VaultSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>密碼編譯服務           
| | |           
|---|---|       
|   **服務描述** |   提供三種管理服務：目錄資料庫服務，可確認 Windows 檔案的簽章，並允許新的程式來安裝;受保護的根服務，可新增和移除受信任的根憑證授權單位憑證，從這台電腦;並自動根憑證更新服務，可從 Windows Update 擷取根憑證，並啟用 SSL 的情況。 如果此服務停止時，這些管理服務將無法正常運作。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   CryptSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>資料共用服務         
| | |           
|---|---|       
|   **服務描述** |   提供應用程式之間仲介的資料。
|   **服務名稱**    |   DsSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **服務描述** |   DCP （資料收集和發行） 服務支援資料上傳至雲端的第一方應用程式。
|   **服務名稱**    |   DcpSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>DCOM 伺服器處理序啟動器         
| | |           
|---|---|       
|   **服務描述** |   DCOMLAUNCH 服務會啟動以回應物件的啟用要求的 COM 和 DCOM 伺服器。 如果此服務已停止或停用，使用 COM 或 DCOM 的程式將無法正常運作。 強烈建議您有 DCOMLAUNCH 服務執行中。
|   **服務名稱**    |   DcomLaunch
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |沒有指引    
|   **註解**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>裝置關聯服務      
| | |           
|---|---|       
|   **服務描述** |   啟用在系統與有線或無線裝置之間的配對。
|   **服務名稱**    |   DeviceAssociationService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>裝置安裝服務      
| | |           
|---|---|       
|   **服務描述** |   可讓辨識及調整硬體變更幾乎沒有任何使用者輸入的電腦。 停止或停用此服務會導致系統不穩定。
|   **服務名稱**    |   DeviceInstall
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>裝置管理註冊服務        
| | |           
|---|---|       
|   **服務描述** |   執行裝置管理的裝置註冊活動
|   **服務名稱**    |   DmEnrollmentSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>裝置安裝管理員         
| | |           
|---|---|       
|   **服務描述** |   可讓偵測、 下載和安裝的裝置相關的軟體。 如果此服務已停用，裝置可能使用過期的軟體，設定，並可能無法正確運作。
|   **服務名稱**    |   DsmSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>DevQuery 背景探索訊息代理程式         
| | |           
|---|---|           
|   **服務描述** |   可讓應用程式探索裝置與背景工作
|   **服務名稱**    |   DevQueryBroker
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>DHCP 用戶端          
| | |           
|---|---|       
|   **服務描述** |   註冊，並更新 IP 位址與這台電腦的 DNS 記錄。 如果此服務停止時，動態 IP 位址和 DNS 更新，將不會收到這台電腦。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   Dhcp
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>診斷原則服務            
| | |           
|---|---|       
|   **服務描述** |   問題偵測、 疑難排解和解決 Windows 元件，可讓診斷原則服務。  如果此服務停止時，將無法再運作診斷。
|   **服務名稱**    |   DPS
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>診斷服務主機     
| | |           
|---|---|       
|   **服務描述** |   主應用程式的診斷工具，需要在本機服務內容中執行診斷原則服務會使用診斷的服務主機。  如果此服務停止時，任何依存於此的診斷將無法再運作。
|   **服務名稱**    |   WdiServiceHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>診斷系統主應用程式           
| | |           
|---|---|       
|   **服務描述** |   診斷系統主應用程式會使用診斷原則服務需要在本機系統內容中執行的主應用程式診斷。  如果此服務停止時，任何依存於此的診斷將無法再運作。
|   **服務名稱**    |   WdiSystemHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>分散式追蹤用戶端的連結            
| | |           
|---|---|   
|   **服務描述** |   維護在電腦或網路中的電腦上的 NTFS 檔案之間的連結。
|   **服務名稱**    |   TrkWks
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>分散式交易協調器     
| | |           
|---|---|   
|   **服務描述** |   協調跨越多個資源管理員，例如資料庫、 訊息佇列和檔案系統的交易。 如果此服務停止時，這些交易將會失敗。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   MSDTC
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **服務描述** |   WAP 推播訊息路由服務
|   **服務名稱**    |   dmwappushservice
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   Intune、 MDM 和類似的管理技術，以及整合寫入篩選器，用戶端裝置上所需的服務。 不需要伺服器。
|||         
            
<br />      

##  <a name="dns-client"></a>DNS 用戶端      
| | |           
|---|---|       
|   **服務描述** |   DNS 用戶端服務 (dnscache) 會快取網域名稱系統 (DNS) 名稱，並會為此電腦註冊完整的電腦名稱。 如果服務已停止，會繼續解析 DNS 名稱。 不過，不會快取的 DNS 名稱查詢的結果，而且不會登錄在電腦的名稱。 如果已停用該服務，所有明確依賴該服務的所有服務，將無法啟動。
|   **服務名稱**    |   Dnscache
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>下載的對應管理員     
| | |           
|---|---|   
|   **服務描述** |   下載對應的應用程式存取的 Windows 服務。 此服務啟動隨應用程式所存取下載對應。 停用此服務會阻止應用程式存取對應。
|   **服務名稱**    |   MapsBroker
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   停用服務; 所依賴的中斷應用程式確定要停用不依賴它的應用程式
|||         
            
<br />          

## <a name="embedded-mode"></a>內嵌的模式            
| | |           
|---|---|       
|   **服務描述** |   內嵌模式服務可讓背景應用程式相關的案例。  停用此服務會導致背景應用程式，使其無法啟動。
|   **服務名稱**    |   embeddedmode
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>加密檔案系統 (EFS)
| | |                   
|---|---|           
|   **服務描述** | 提供用來儲存加密的檔案在 NTFS 檔案系統磁碟區上的核心檔案加密技術。 如果此服務已停止或停用，則應用程式將無法存取加密的檔案。            
|   **服務名稱**  |  EFS            
|   **安裝**  |  永遠都會安裝           
|   **StartType**   |  Manual           
|   **建議**  | 沒有指引           
|   **註解**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>企業應用程式管理的服務            
| | |           
|---|---|       
|   **服務描述** |   可讓企業應用程式管理。
|   **服務名稱**    |   EntAppSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>可延伸驗證通訊協定           
| | |           
|---|---|   
|   **服務描述** |   在此情況下為 802.1x 有線和無線網路驗證、 VPN 和網路存取保護 (NAP)，就會提供可延伸驗證通訊協定 (EAP) 服務。  EAP 也提供應用程式開發介面 (Api) 所使用的網路存取用戶端，包括無線及 VPN 用戶端，在驗證程序。  如果您停用這項服務，此電腦無法存取需要 EAP 驗證的網路。
|   **服務名稱**    |   EapHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>功能探索提供者裝載         
| | |           
|---|---|       
|   **服務描述** |   FDPHOST 服務裝載函式探索 (FD) 網路探索提供者。 這些 FD 提供者會提供網路探索服務 「 簡易服務探索通訊協定 (SSDP) 和 Web 服務-探索 (WS-D) 通訊協定。 停止或停用 FDPHOST 服務將會停用這些通訊協定的網路探索時使用 FD。 此服務無法使用時，會找不到網路裝置或資源使用 FD 和依賴這些探索通訊協定的網路服務。
|   **服務名稱**    |   fdPHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>功能探索資源發佈      
| | |           
|---|---|       
|   **服務描述** |   發佈此電腦和連接到這部電腦，以便在網路上探索到的資源。  如果停止此服務，將不會再發行網路資源，並不會探索網路上其他電腦。
|   **服務名稱**    |   FDResPub
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>地理位置服務          
| | |           
|---|---|       
|   **服務描述** |   此服務會監視系統的目前位置，並管理地理柵欄 （具有相關聯的事件的地理位置）。  如果您關閉這項服務時，應用程式將無法使用，或是針對地理位置或地理柵欄接收通知。
|   **服務名稱**    |   lfsvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   停用服務; 所依賴的中斷應用程式確定要停用不依賴它的應用程式
|||         
            
<br />          

##  <a name="group-policy-client"></a>群組原則用戶端     
| | |           
|---|---|       
|   **服務描述** |   服務會負責套用設定的電腦和使用者可透過群組原則元件的系統管理員設定。 如果服務已停用，不會套用的設定和應用程式和元件將無法透過群組原則管理。 任何元件或應用程式是依賴群組原則元件可能無法運作，如果服務已停用。
|   **服務名稱**    |   gpsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>人性化介面裝置服務           
| | |           
|---|---|       
|   **服務描述** |   啟用，並維護鍵盤、 擃勂厞和其他多媒體裝置上常用按鈕的使用。 建議您保持執行此服務。
|   **服務名稱**    |   hidserv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>HV 主機服務     
| | |           
|---|---|   
|   **服務描述** |   提供介面供 HYPER-V hypervisor 提供給主機作業系統的每個磁碟分割效能計數器。
|   **服務名稱**    |   HvHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   客體 Vm 的效能增強工具。 未使用的今天，除了明確填入 Vm，但將用於應用程式防護
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Hyper-V 資料交換服務        
| | |           
|---|---|   
|   **服務描述** |   提供一種機制，以便在虛擬機器及實體電腦上執行之作業系統間交換資料。
|   **服務名稱**    |   vmickvpexchange
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Hyper-V 客體服務介面          
| | |           
|---|---|   
|   **服務描述** |   提供介面供 HYPER-V 主機與虛擬機器內執行的特定服務互動。
|   **服務名稱**    |   vmicguestinterface
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Hyper-V 客體關機服務           
| | |           
|---|---|       
|   **服務描述** |   提供一個機制來關閉在實體電腦上的管理介面從這部虛擬機器的作業系統。
|   **服務名稱**    |   vmicshutdown
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Hyper-V 活動訊號服務            
| | |           
|---|---|           
|   **服務描述** |   藉由定期報告活動訊號監視此虛擬機器的狀態。 這項服務可協助您識別執行中虛擬機器已停止回應。
|   **服務名稱**    |   vmicheartbeat
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Hyper-V PowerShell Direct 服務            
| | |           
|---|---|       
|   **服務描述** |   提供一個機制來管理使用 PowerShell 透過 VM 工作階段不使用虛擬網路的虛擬機器。
|   **服務名稱**    |   vmicvmsession
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Hyper-V 遠端桌面虛擬服務            
| | |           
|---|---|       
|   **服務描述** |   提供虛擬機器與實體電腦上執行的作業系統之間的通訊平台。
|   **服務名稱**    |   vmicrdv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Hyper-V 時間同步化服務         
| | |           
|---|---|       
|   **服務描述** |   此虛擬機器的系統時間同步實體電腦的系統時間。
|   **服務名稱**    |   vmictimesync
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Hyper-V 磁碟區陰影複製要求者         
| | |           
|---|---|           
|   **服務描述** |   協調使用磁碟區陰影複製服務來備份應用程式和資料在此虛擬機器上從實體電腦上的作業系統所需的通訊。
|   **服務名稱**    |   vmicvss
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE 和 AuthIP IPsec 金鑰處理模組          
| | |           
|---|---|       
|   **服務描述** |   IKEEXT 服務主控的網際網路金鑰交換 (IKE) 和已驗證網際網路通訊協定 (AuthIP) 金鑰處理模組。 這些金鑰產製模組用於驗證和金鑰交換中網際網路通訊協定安全性 (IPsec)。 停止或停用 IKEEXT 服務將會停用對等電腦與 IKE 和 AuthIP 金鑰交換。 IPsec 通常設定為使用 IKE 或 AuthIP;因此，停止或停用 IKEEXT 服務可能會導致 IPsec 失敗，而且可能會危及系統的安全性。 強烈建議您有 IKEEXT 服務執行中。
|   **服務名稱**    |   IKEEXT
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>互動式服務偵測           
| | |           
|---|---|   
|   **服務描述** |   可讓使用者輸入的互動的服務，可供存取對話方塊出現時，互動式服務所建立的使用者通知。 如果此服務停止時，新的互動式服務對話方塊的通知將無法再運作及可能不會有互動式服務對話方塊的存取。 如果此服務已停用，通知的和新的互動式服務對話方塊的存取將無法再運作。
|   **服務名稱**    |   UI0Detect
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>網際網路連線共用 (ICS)            
| | |           
|---|---|           
|   **服務描述** |   提供網路位址轉譯、 定址，家用或小型辦公室網路的名稱解析和/或入侵預防服務。
|   **服務名稱**    |   SharedAccess
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   可用來 WiFi 熱點，並也 Miracast 投影的兩端的用戶端的必要項。 ICS 可以封鎖使用 GPO 設定，「 禁止使用您的 DNS 網域網路上的網際網路連線共用 」
|||         
            
<br />          

## <a name="ip-helper"></a>IP 協助程式            
| | |           
|---|---|       
|   **服務描述** |   提供使用 IPv6 轉換技術 (6to4，ISATAP、 連接埠 Proxy 和 Teredo) 和 IP-HTTPS 通道連線能力。 如果此服務停止時，電腦就沒有這些技術提供加強連線能力優點。
|   **服務名稱**    |   iphlpsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>IPSec 原則代理程式      
| | |           
|---|---|       
|   **服務描述** |   網際網路通訊協定安全性 (IPsec) 支援網路層級對等驗證、 資料來源驗證、 資料完整性、 資料機密性 （加密），以及重新執行保護。  此服務會強制執行透過 IP 安全性原則嵌入式管理單元或命令列工具 「 netsh ipsec 」 建立的 IPsec 原則。  如果您停止此服務時，您可能會遇到網路連線問題，如果您的原則需要的連線會使用 IPsec。  此外，Windows 防火牆遠端管理無法使用。 當此服務停止
|   **服務名稱**    |   PolicyAgent
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>KDC Proxy Server 服務 (KPS)      
| | |           
|---|---|       
|   **服務描述** |   KDC Proxy Server 服務上執行 edge server 到 proxy Kerberos 到公司網路上的網域控制站的通訊協定訊息。
|   **服務名稱**    |   KPSSVC
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>用於分散式交易協調器的 KtmRm            
| | |           
|---|---|       
|   **服務描述** |   協調 Distributed Transaction Coordinator (MSDTC) 和核心交易管理員 (KTM) 之間的交易。 如果不需要建議您使用此服務會保留已停止。 必要時，MSDTC 和 KTM 將這項服務自動啟動。 如果此服務已停用，互動與核心資源管理員 」 中的任何 MSDTC 交易將會失敗，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   KtmRm
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>連結階層拓樸探索對應        
| | |       
|---|---|       
|   **服務描述** |   建立網路對應，其中包含電腦和裝置拓撲 （連線） 資訊，並且描述每個電腦和裝置的中繼資料。  如果此服務已停用，在網路地圖將無法正常運作。
|   **服務名稱**    |   lltdsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   若要停用 如果沒有相依性網路地圖上的 確定
|||         
            
<br />

## <a name="local-session-manager"></a>本機工作階段管理員                    
| | |                   
|---|---|   
|   **服務描述** |   管理本機使用者工作階段的核心 Windows 服務。 停止或停用此服務會導致系統不穩定。    
|   **服務名稱**    |   LSM |
|   **安裝**    |   永遠都會安裝    |
|   **StartType**   |   自動   |
|   **建議**  | 沒有指引   
|   **註解**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Microsoft (R) 診斷中樞標準收集器         
| | |           
|---|---|           
|   **服務描述** |   管理本機使用者工作階段的核心 Windows 服務。 停止或停用此服務會導致系統不穩定。
|   **服務名稱**    |   LSM
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Microsoft 帳戶登入小幫手          
| | |           
|---|---|       
|   **服務描述** |   可讓使用者登入時透過 Microsoft 帳戶身分識別服務。 如果此服務停止時，使用者將無法登入 Microsoft 帳戶與公司電腦。
|   **服務名稱**    |   wlidsvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   Microsoft 帳戶是 Windows Server 上的 n/A
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft APP-V 用戶端      
| | |           
|---|---|       
|   **服務描述** |   管理 APP-V 使用者和虛擬應用程式
|   **服務名稱**    |   AppVClient
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Microsoft iSCSI 啟動器服務            
| | |           
|---|---|       
|   **服務描述** |   管理從這台電腦的網際網路 SCSI (iSCSI) 工作階段，到遠端 「 iSCSI 目標裝置。 如果此服務停止時，此電腦將無法登入或存取 iSCSI 目標。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   MSiSCSI
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   我們的診斷資料會指出這用在用戶端，以及伺服器上。 若要停用此任何好處。
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **服務描述** |   提供用來驗證使用者的相關聯的身分識別提供者的密碼編譯金鑰的程序隔離。 如果此服務已停用，所有使用和管理這些金鑰將無法使用，其中包含機器登入和單一登上的應用程式和網站。 此服務會啟動，並會自動停止。 建議您不要重新設定此服務。
|   **服務名稱**    |   NgcSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   需要 PIN/Hello 登入，不支援在伺服器
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Microsoft Passport 容器         
| | |           
|---|---|       
|   **服務描述** |   管理本機使用者身分識別金鑰用來驗證使用者身分識別提供者，以及 TPM 虛擬智慧卡。 如果已停用這項服務，本機使用者身分識別金鑰和 TPM 虛擬智慧卡會無法存取。 建議您不要重新設定此服務。
|   **服務名稱**    |   NgcCtnrSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Microsoft 軟體陰影複製提供者          
| | |           
|---|---|       
|   **服務描述** |   管理磁碟區陰影複製服務所花費的以軟體為基礎的磁碟區陰影複製。 如果此服務停止時，就無法管理軟體為基礎的磁碟區陰影複製。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   swprv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>Microsoft 儲存空間 SMP         
| | |           
|---|---|       
|   **服務描述** |   Microsoft 儲存空間管理提供者的主機服務。 如果此服務已停止或停用，無法管理儲存空間。
|   **服務名稱**    |   smphost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   存放管理 Api 失敗而不需要這項服務。 範例："Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Net.Tcp Port Sharing Service         
| | |           
|---|---|       
|   **服務描述** |   提供共用 TCP 連接埠，透過 net.tcp 通訊協定的能力。
|   **服務名稱**    |   NetTcpPortSharing
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Netlogon         
| | |           
|---|---|           
|   **服務描述** |   維護此電腦與網域控制站，以驗證使用者和服務之間的安全通道。 如果此服務停止時，電腦可能不會驗證使用者和服務，而且網域控制站無法登錄 DNS 記錄。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   Netlogon
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>網路連線代理人            
| | |           
|---|---|       
|   **服務描述** |   允許從網際網路接收通知的 Microsoft Store 應用程式的代理程式連線。
|   **服務名稱**    |   NcbService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>網路連線         
| | |           
|---|---|   
|   **服務描述** |   管理網路和撥號連線 資料夾，您可以在這裡檢視區域網路和遠端連接的物件。
|   **服務名稱**    |   Netman
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>網路連線助理      
| | |           
|---|---|       
|   **服務描述** |   提供 DirectAccess UI 元件的狀態通知
|   **服務名稱**    |   NcaSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>網路清單服務        
| | |           
|---|---|   
|   **服務描述** |   識別的網路的電腦已連線、 收集與儲存屬性的這些網路，以及這些屬性變更時通知應用程式。
|   **服務名稱**    |   netprofm
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>網路定位知悉           
| | |           
|---|---|       
|   **服務描述** |   會收集和儲存網路組態資訊，這項資訊遭到修改時通知程式。 如果此服務停止時，可能無法使用組態資訊。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   NlaSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>網路安裝程式服務       
| | |           
|---|---|       
|   **服務描述** |   網路設定服務管理的網路驅動程式安裝，並允許低層級的網路設定的組態。  如果此服務停止時，可能會取消正在進行中的任何驅動程式安裝。
|   **服務名稱**    |   NetSetupSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Network Store Interface Service      
| | |           
|---|---|   
|   **服務描述** |   此服務會傳遞至使用者模式用戶端的網路 （例如介面新增/刪除等） 的通知。 停止此服務會導致失去網路連線。 如果已停用這項服務，任何明確依存於此服務的服務將無法啟動。
|   **服務名稱**    |   nsi
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="offline-files"></a>離線檔案            
| | |           
|---|---|       
|   **服務描述** |   服務會執行離線檔案離線檔案快取上的進行維護活動回應使用者的登入和登出事件，會實作公用 API 中，內部，並分派關注這些事件感興趣離線檔案的活動和快取狀態的變更。
|   **服務名稱**    |   CscService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>最佳化磁碟機          
| | |           
|---|---|   
|   **服務描述** |   可協助更有效率地執行，藉由最佳化儲存體磁碟機上的檔案的電腦。
|   **服務名稱**    |   defragsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>效能計數器 DLL 主機         
| | |           
|---|---|       
|   **服務描述** |   可讓遠端使用者和 64 位元處理序來查詢 32 位元 Dll 所提供的效能計數器。 如果此服務停止時，只有本機使用者和 32 位元處理序能夠查詢 32 位元 Dll 所提供的效能計數器。
|   **服務名稱**    |   PerfHost
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>效能記錄檔與警示            
| | |           
|---|---|   
|   **服務描述** |   效能記錄檔及警示收集效能資料，從本機或遠端電腦，根據預先設定的排程參數，然後將資料寫入至記錄檔，或會觸發警示。 如果此服務停止時，不會收集效能資訊。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   pla
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>電話服務       
| | |           
|---|---|   
|   **服務描述** |   管理裝置上的電話語音狀態
|   **服務名稱**    |   PhoneSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用現代化的 VoIP 應用程式
|||         
            
<br />          

##      <a name="plug-and-play"></a>隨插即用       
| | |           
|---|---|   
|   **服務描述** |   可讓辨識及調整硬體變更幾乎沒有任何使用者輸入的電腦。 停止或停用此服務會導致系統不穩定。
|   **服務名稱**    |   PlugPlay
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>可攜式裝置列舉程式服務           
| | |           
|---|---|       
|   **服務描述** |   卸除式大量儲存裝置的群組原則會強制執行。 例如，Windows Media Player 和映像匯入精靈 來傳送及同步處理內容使用卸除式的大型存放裝置，可讓應用程式。
|   **服務名稱**    |   WPDBusEnum
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="power"></a>電源            
| | |           
|---|---|       
|   **服務描述** |   管理電源原則和電源原則通知傳遞。
|   **服務名稱**    |   電源
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>列印多工緩衝處理器            
| | |           
|---|---|   
|   **服務描述** |   此服務多工緩衝處理列印工作，並會處理與印表機的互動。  如果您關閉這項服務時，您將無法列印，或請參閱您的印表機。
|   **服務名稱**    |   多工緩衝處理器
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |   確定要停用 如果沒有 「 列印伺服器 」 或 「 DC
|   **註解**    |   網域控制站上的 DC 角色安裝，請將負責執行列印剪除 – 從 Active Directory 中移除過時的列印佇列物件的多工緩衝處理器服務執行緒。  如果多工緩衝處理器服務未執行在每個站台至少一個網域控制站，AD 就會有任何方法來移除舊的佇列不存在。 https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>印表機延伸模組和通知        
| | |           
|---|---|       
|   **服務描述** |   此服務會開啟 自訂印表機對話方塊，並會處理從遠端列印伺服器或印表機的通知。 如果您關閉這項服務時，將無法看到印表機延伸模組或通知。
|   **服務名稱**    |   PrintNotify
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]，如果沒有列印伺服器
|   **註解**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>問題報告及解決方案控制面板支援     
| | |           
|---|---|   
|   **服務描述** |   這項服務都提供檢視、 傳送和刪除的問題報告及解決方案 控制台中的系統層級問題報告的支援。
|   **服務名稱**    |   wercplsupport
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>程式相容性助理 （jlca） 服務     
| | |           
|---|---|       
|   **服務描述** |   這項服務提供支援的程式相容性助理 (PCA)。  PCA 監視安裝，並在使用者執行的程式，並偵測已知的相容性問題。 如果此服務停止，PCA 將無法正常運作。
|   **服務名稱**    |   PcaSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>高品質 Windows 音訊/視訊體驗      
| | |           
|---|---|   
|   **服務描述** |   品質 Windows 音訊/視訊體驗 (qWave) 是網路平台的音訊視訊 (AV) 串流在 IP 家用網路上的應用程式。 qWave 可增強 AV 串流藉由確保網路的服務品質 (QoS) AV 應用程式的效能和可靠性。 它提供許可控制、 執行階段監視和強制執行、 應用程式的意見反應和流量優先順序排定等機制。
|   **服務名稱**    |   QWAVE
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   用戶端 QoS 服務
|||         
            
<br />          

##      <a name="radio-management-service"></a>選項管理服務        
| | |           
|---|---|   
|   **服務描述** |   選項管理和飛航模式服務
|   **服務名稱**    |   RmSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Remote Access Auto Connection Manager            
| | |           
|---|---|   
|   **服務描述** |   每當程式參考的遠端 DNS 或 NetBIOS 名稱或位址，請建立遠端網路的連線。
|   **服務名稱**    |   RasAuto
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>遠端存取連線管理員         
| | |           
|---|---|   
|   **服務描述** |   管理從這台電腦的撥號和虛擬私人網路 (VPN) 連線到網際網路或其他遠端網路。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   RasMan
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>遠端桌面設定         
| | |           
|---|---|   
|   **服務描述** |   遠端桌面組態服務 (RDCS) 負責所有的遠端桌面服務和遠端桌面的相關的組態和工作階段維護活動需要系統內容。 其中包括每個工作階段的暫存資料夾中，RD 佈景主題，以及 RD 憑證。
|   **服務名稱**    |   SessionEnv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>遠端桌面服務          
| | |           
|---|---|   
|   **服務描述** |   可讓使用者以互動方式連接到遠端電腦。 遠端桌面和遠端桌面工作階段主機伺服器取決於此服務。  若要防止這台電腦的遠端使用，請清除 系統屬性控制面板項目中的 遠端 索引標籤上的核取方塊。
|   **服務名稱**    |   TermService
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>遠端桌面服務使用者模式連接埠重新導向器        
| | |           
|---|---|   
|   **服務描述** |   允許 RDP 連線印表機磁碟機/連接埠重新導向
|   **服務名稱**    |   UmRdpService
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   在 連接的伺服器端上支援重新導向。
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>遠端程序呼叫 (RPC)          
| | |           
|---|---|   
|   **服務描述** |   RPCSS 服務是服務控制管理員 COM 和 DCOM 伺服器。 它會執行物件的啟用要求、 物件匯出工具的解析度和分散式記憶體回收，COM 和 DCOM 伺服器。 如果此服務已停止或停用，使用 COM 或 DCOM 的程式將無法正常運作。 強烈建議您有 RPCSS 服務執行中。
|   **服務名稱**    |   RpcSs
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>遠端程序呼叫 (RPC) 定位器             
| | |               
|---|---|   
|   **服務描述** |   在 Windows 2003 和較早版本的 Windows 中，遠端程序呼叫 (RPC) Locator service 會管理 RPC 名稱服務資料庫。 在 Windows Vista 和更新版本的 Windows 中，此服務不提供任何功能，並在於應用程式相容性。   |
|   **服務名稱**    |   RpcLocator  |
|   **安裝**    |   只含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  | 沒有指引   |
|   **註解**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>遠端登錄          
| | |           
|---|---|   
|   **服務描述** |   可讓遠端使用者可以修改此電腦上的登錄設定。 如果此服務已停止，則可以修改登錄，只能由這部電腦上的使用者。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   RemoteRegistry
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>原則提供者原則結果組            
| | |           
|---|---|       
|   **服務描述** |   提供處理要求，以模擬應用程式的目標使用者或電腦在各種情況下的群組原則設定，並計算原則結果組設定的網路服務。
|   **服務名稱**    |   RSoPProv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |沒有指引    
|   **註解**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>路由及遠端存取            
| | |           
|---|---|   
|   **服務描述** |   區域和廣域網路網路環境中的企業路由服務的供應項目。
|   **服務名稱**    |   RemoteAccess
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   已停用
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>RPC 端點對應程式          
| | |           
|---|---|   
|   **服務描述** |   傳輸端點將解析 RPC 介面識別項。 如果此服務已停止或停用，使用遠端程序呼叫 (RPC) 服務的程式將無法正常運作。
|   **服務名稱**    |   RpcEptMapper
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>次要登入     
| | |           
|---|---|       
|   **服務描述** |   啟用開始以替代認證的程序。 如果此服務停止時，將無法使用這種類型的登入存取。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   seclogon
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>安全通訊端通道通訊協定服務            
| | |               
|---|---|       
|   **服務描述** |   提供支援的安全通訊端通道通訊協定 (SSTP) 連線到遠端電腦使用 VPN。 如果此服務已停用，使用者將無法使用 SSTP 存取遠端伺服器。    |
|   **服務名稱**    |   SstpSvc |
|   **安裝**    |   永遠都會安裝    |
|   **StartType**   |   Manual  |
|   **建議**  |   請勿停用  |
|   **註解**    |   停用符號 RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>安全性帳戶管理員            
| | |           
|---|---|       
|   **服務描述** |   啟動此服務會通知其他服務安全性帳戶管理員 (SAM) 已準備好接受要求。  停用此服務會導致其他服務中 SAM 就緒時通知系統，進而可能導致無法正確啟動這些服務。 此外，此服務不應該停用。
|   **服務名稱**    |   SamSs
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 請勿停用
|   **註解**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>感應器資料服務  
| | |           
|---|---|   
|   **服務描述** |   提供各式各樣的感應器資料
|   **服務名稱**    |   SensorDataService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>感應器監視服務            
| | |           
|---|---|       
|   **服務描述** |   監視各種感應器，才能公開資料，並且配合系統和使用者狀態。  如果此服務已停止或停用，光源狀況不調整顯示器亮度。 停止此服務可能會影響其他的系統功能和功能。
|   **服務名稱**    |   SensrSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>感應器服務           
| | |           
|---|---|       
|   **服務描述** |   管理不同感應器功能的感應器服務。 管理簡單裝置方向 (SDO) 和歷程記錄的感應器。 載入報告裝置方向變更 SDO 感應器。  如果此服務已停止或停用，將不會載入 SDO 感應器，因此不會發生自動輪替。 也會停止來自感應器歷程記錄集合。
|   **服務名稱**    |   SensorService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="server"></a>Server           
| | |           
|---|---|   
|   **服務描述** |   支援的檔案、 列印及具名管道透過這台電腦的網路共用。 如果此服務停止時，將無法使用這些函式。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   LanmanServer
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   所需的遠端管理、 IPC$ SMB 檔案共用
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Shell Hardware Detection             
| | |           
|---|---|       
|   **服務描述** |   提供 [自動播放] 硬體事件的通知。
|   **服務名稱**    |   ShellHWDetection
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="smart-card"></a>智慧卡           
| | |           
|---|---|   
|   **服務描述** |   管理智慧卡讀取此電腦的存取。 如果此服務停止時，此電腦將無法讀取智慧卡。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   SCardSvr
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>智慧卡裝置列舉型別服務                    
| | |               
|---|---|       
|   **服務描述** |   針對所有的智慧卡讀取裝置的軟體裝置節點建立可存取指定的工作階段。 如果此服務已停用，WinRT Api 會無法列舉智慧卡讀取裝置。   |
|   **服務名稱**    |   ScDeviceEnum    |
|   **安裝**    |   永遠都會安裝    |
|   **StartType**   |   Manual  |
|   **建議**  |   若要停用 [確定]   |
|   **註解**    |   需要幾乎是專供 WinRT 應用程式    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>智慧卡移除原則        
| | |           
|---|---|       
|   **服務描述** |   可讓系統設定來鎖定智慧卡移除後的使用者桌面。
|   **服務名稱**    |   SCPolicySvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>SNMP 陷阱            
| | |           
|---|---|       
|   **服務描述** |   接收本機或遠端的簡易網路管理通訊協定 (SNMP) 代理程式所產生的設陷訊息，並將訊息轉送到此電腦上執行 SNMP 管理程式。 如果此服務停止時，在此電腦上的 SNMP 程式不會收到 SNMP 設陷訊息。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   SNMPTRAP
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>軟體保護             
| | |           
|---|---|       
|   **服務描述** |   下載、 安裝以及 Windows 和 Windows 的數位授權強制，可讓應用程式。 如果服務已停用，以通知模式可能執行的作業系統和授權的應用程式。 強烈建議您不停用軟體保護服務。
|   **服務名稱**    |   sppsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>特殊的系統管理主控台的協助程式        
| | |           
|---|---|   
|   **服務描述** |   允許從遠端存取 命令提示字元使用緊急管理服務的系統管理員。
|   **服務名稱**    |   sacsvr
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>錯誤檢查器            
| | |           
|---|---|   
|   **服務描述** |   驗證可能的檔案系統損毀。
|   **服務名稱**    |   svsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>SSDP 探索           
| | |           
|---|---|   
|   **服務描述** |   探索網路的裝置和服務使用 SSDP 探索通訊協定，例如 UPnP 裝置。 也宣佈 SSDP 裝置和服務在本機電腦上執行。 如果此服務停止時，不會探索 SSDP 為基礎的裝置。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   SSDPSRV
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>服務狀態的儲存機制         
| | |           
|---|---|   
|   **服務描述** |   應用程式模型提供必要的基礎結構支援。
|   **服務名稱**    |   StateRepository
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>靜止影像擷取事件
| | |           
|---|---|   
|   **服務描述** |   啟動仍映像擷取事件相關聯的應用程式。
|   **服務名稱**    |   WiaRpc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />  

## <a name="storage-service"></a>儲存體服務          
| | |           
|---|---|       
|   **服務描述** |   提供儲存體設定和外部儲存體擴充的啟用服務
|   **服務名稱**    |   StorSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>儲存層管理        
| | |           
|---|---|   
|   **服務描述** |   最佳化所有的階層式的儲存空間上的儲存層中的資料放置在系統中。
|   **服務名稱**    |   TieringEngineService
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>Superfetch          
| | |           
|---|---|       
|   **服務描述** |   會維護並改善系統效能，經過一段時間。
|   **服務名稱**    |   SysMain
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="sync-host"></a>同步處理主機            
| | |           
|---|---|       
|   **服務描述** |   郵件、 連絡人、 行事曆和各種其他使用者資料，此服務會同步處理。 郵件及其他應用程式相依於這項功能將無法正常運作時此服務並未執行。
|   **服務名稱**    |   OneSyncSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          

## <a name="system-event-notification-service"></a>系統事件通知服務            
| | |           
|---|---|       
|   **服務描述** |   監視系統事件，並通知至 COM + 事件系統的這些事件的訂閱者。
|   **服務名稱**    |   SENS
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>系統事件訊息代理程式             
| | |           
|---|---|       
|   **服務描述** |   協調的 WinRT 應用程式的背景工作的執行。 如果此服務已停止或停用，則背景工作可能不會觸發。
|   **服務名稱**    |   SystemEventsBroker
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   儘管其描述表示它是只針對 WinRT 應用程式，它需要工作排程器、 訊息代理程式基礎結構服務，以及其他內部元件。
|||         
            
<br />          

## <a name="task-scheduler"></a>工作排程器           
| | |           
|---|---|   
|   **服務描述** |   可讓使用者可以設定及排程自動化的工作，在此電腦上。 服務也會裝載多個 Windows 重要的系統工作。 如果此服務已停止或停用，將不會執行這些工作在排定的時間。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   排程
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>TCP/IP NetBIOS 協助程式            
| | |           
|---|---|   
|   **服務描述** |   提供支援 NetBIOS over TCP/IP (NetBT) 服務和 NetBIOS 名稱解析用戶端在網路上，因此共用檔案，可讓使用者列印及登入網路。 如果此服務停止時，可能無法使用這些函式。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   lmhosts
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telephony           
| | |           
|---|---|       
|   **服務描述** |   支援電話語音 API (TAPI) 控制在本機電腦上，並透過區域網路，也會執行服務的伺服器上的電話語音裝置的程式。
|   **服務名稱**    |   TapiSrv
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   停用符號 RRAS
|||         
            
<br />          

## <a name="themes"></a>佈景主題           
| | |           
|---|---|
|   **服務描述** |   提供使用者體驗的佈景主題的管理。
|   **服務名稱**    |   佈景主題
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   此服務已停用時，無法設定協助工具主題
|||         
            
<br />  

## <a name="tile-data-model-server"></a>圖格的資料模型伺服器           
| | |           
|---|---|   
|   **服務描述** |   圖格並排顯示更新的伺服器。
|   **服務名稱**    |   tiledatamodelsvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   如果此服務已停用，開始功能表符號
|||         
            
<br />          

##  <a name="time-broker"></a>時間 Broker     
| | |           
|---|---|       
|   **服務描述** |   協調的 WinRT 應用程式的背景工作的執行。 如果此服務已停止或停用，則背景工作可能不會觸發。
|   **服務名稱**    |   TimeBrokerSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   儘管其描述表示它是只針對 WinRT 應用程式，它需要工作排程器、 訊息代理程式基礎結構服務，以及其他內部元件。
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>觸控式鍵盤與手寫面板服務         
| | |           
|---|---|   
|   **服務描述** |   可啟用觸控式鍵盤與手寫面板手寫筆和筆跡功能
|   **服務名稱**    |   TabletInputService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Windows Update 的更新協調器服務           
| | |           
|---|---|       
|   **服務描述** |   管理 Windows 更新。 如果停止，您的裝置將無法再下載並安裝最新的更新。
|   **服務名稱**    |   UsoSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   V1607; 中的服務描述遺漏Windows 更新 （包括 WSUS） 相依於此服務。
|||         
            
<br />          

## <a name="upnp-device-host"></a>UPnP 裝置主機         
| | |           
|---|---|   
|   **服務描述** |   允許 UPnP 裝置可以裝載在此電腦上。 如果此服務停止時，任何裝載的 UPnP 裝置將會停止運作，並可以新增任何其他的裝載的裝置。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   upnphost
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>User Access Logging Service          
| | |           
|---|---|   
|   **服務描述** |   這項服務記錄中的 IP 位址和使用者名稱，已安裝的產品和角色，在本機伺服器上的表單的唯一用戶端的存取要求。 這項資訊可以進行查詢，透過 Powershell，系統管理員不必量化離線的用戶端存取使用權 (CAL) 管理伺服器軟體的用戶端需求。 如果服務已停用，用戶端要求將不會記錄，並不會透過 Powershell 查詢可擷取。 停止服務並不會影響查詢的歷程記錄資料 （請參閱支援文件的步驟來刪除歷程記錄資料）。 本機系統管理員必須參閱，對方，Windows Server 授權條款，以判斷所需的適當授權; 的伺服器軟體的 Cal 數目使用 UAL 服務和資料不會改變此等義務。
|   **服務名稱**    |   UALSVC
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>使用者資料存取        
| | |           
|---|---|   
|   **服務描述** |   提供結構化的使用者資料，包括連絡資訊、 行事曆、 訊息和其他內容的應用程式存取。 如果您停止或停用這項服務，使用這項資料的應用程式可能無法正確運作。
|   **服務名稱**    |   UserDataSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          

## <a name="user-data-storage"></a>使用者資料儲存體            
| | |           
|---|---|       
|   **服務描述** |   處理結構化的使用者資料，包括連絡資訊、 行事曆、 訊息和其他內容的儲存體。 如果您停止或停用這項服務，使用這項資料的應用程式可能無法正確運作。
|   **服務名稱**    |   UnistoreSvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>使用者經驗虛擬化服務           
| | |           
|---|---|       
|   **服務描述** |   提供應用程式和 OS 設定漫遊支援
|   **服務名稱**    |   UevAgentService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>使用者管理員        
| | |           
|---|---|   
|   **服務描述** |   使用者管理員提供多使用者互動所需的執行階段元件。  如果此服務停止時，某些應用程式可能無法正常運作。
|   **服務名稱**    |   UserManager
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>使用者設定檔服務         
| | |           
|---|---|   
|   **服務描述** |   這項服務負責載入和卸載使用者設定檔。 如果此服務已停止或停用，使用者將不再能夠成功登入或登出、 應用程式可能會遇到了使用者資料的問題和註冊要接收的設定檔的事件通知的元件不會接收它們。
|   **服務名稱**    |   ProfSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>虛擬磁碟             
| | |           
|---|---|   
|   **服務描述** |   提供磁碟、 磁碟區、 檔案系統與存放裝置陣列的管理服務。
|   **服務名稱**    |   vds
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   沒有指引
|   **註解**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>磁碟區陰影複製           
| | |           
|---|---|   
|   **服務描述** |   管理及執行磁碟區陰影複製用來備份和其他用途。 如果此服務停止時，備份將無法使用陰影複製，而且備份可能會失敗。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   VSS
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   沒有指引
|   **註解**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **服務描述** |   「 電子錢包 」 的用戶端所使用的主機物件
|   **服務名稱**    |   WalletService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Windows Audio            
| | |           
|---|---|       
|   **服務描述** |   管理 Windows 型程式的音訊。  如果此服務停止時，音訊裝置和效果將無法正常運作。  如果已停用這項服務，任何明確依存於其的服務將無法啟動
|   **服務名稱**    |   Audiosrv
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Windows 音訊的端點產生器           
| | |           
|---|---|
|   **服務描述** |   管理 Windows 音訊服務的音訊裝置。  如果此服務停止時，音訊裝置和效果將無法正常運作。  如果已停用這項服務，任何明確依存於其的服務將無法啟動
|   **服務名稱**    |   AudioEndpointBuilder
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Windows 生物識別服務            
| | |           
|---|---|   
|   **服務描述** |   Windows 生物識別服務可讓用戶端應用程式擷取、 比較、 操作和儲存生物特徵辨識資料但不直接存取任何生物識別硬體或樣本的能力。 服務會裝載於特殊權限的 SVCHOST 程序。
|   **服務名稱**    |   WbioSrvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Windows 相機框架伺服器         
| | |           
|---|---|       
|   **服務描述** |   可讓多個用戶端從相機的裝置存取視訊畫面。
|   **服務名稱**    |   FrameServer
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Windows 連線管理員           
| | |           
|---|---|   
|   **服務描述** |   會自動連接/中斷連接決策會根據電腦目前可用的網路連線能力選項，以及啟用群組原則設定為基礎的網路連線的管理。
|   **服務名稱**    |   Wcmsvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Windows Defender 網路檢查服務          
| | |           
|---|---|       
|   **服務描述** |   目標網路通訊協定中的已知和新發現弱點的入侵嘗試的協助防範
|   **服務名稱**    |   WdNisSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Windows Defender 服務         
| | |           
|---|---|       
|   **服務描述** |   協助保護使用者免於惡意程式碼和其他潛在垃圾的軟體
|   **服務名稱**    |   WinDefend
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation 使用者模式驅動程式架構           
| | |           
|---|---|   
|   **服務描述** |   建立及管理使用者模式驅動程式處理程序。 無法停止此服務。
|   **服務名稱**    |   wudfsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Windows 加密提供者主機服務     
| | |           
|---|---|   
|   **服務描述** |   Windows 加密提供者主機服務訊息代理程式的加密需要評估，並套用 EAS 原則的處理程序從協力廠商加密提供者相關功能。 停止這會危及 EAS 連線的郵件帳戶所建立的合規性檢查
|   **服務名稱**    |   WEPHOSTSVC
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Windows 錯誤報告服務          
| | |           
|---|---|       
|   **服務描述** |   可讓程式停止運作或回應時回報錯誤，並允許傳遞現有的解決方案。 也可讓記錄檔，以產生診斷和修復服務。 如果此服務已停止，錯誤報告可能無法正常運作，以及診斷服務和修復的結果可能不會顯示。
|   **服務名稱**    |   WerSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   會收集並傳送 MS 和協力廠商 Isv/Ihv 所使用的當機/停止回應資料。 資料用來診斷當機引發的錯誤，可能包括安全性錯誤。 也需要企業錯誤回報工具
|||         
            
<br />          

## <a name="windows-event-collector"></a>Windows 事件收集器          
| | |           
|---|---|   
|   **服務描述** |   此服務會管理來源支援 Ws-management 通訊協定的遠端事件的持續性的訂用帳戶。 這包括 Windows Vista 事件記錄檔、 硬體和 IPMI 啟用事件來源。 服務會儲存轉送在本機的事件記錄檔中的事件。 如果此服務已停止或停用無法建立事件訂用帳戶，而且無法接受轉送的事件。
|   **服務名稱**    |   Wecsvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   管理性方面，診斷收集 ETW 事件 （包括安全性事件）。  許多功能和協力廠商工具依賴此功能，包括安全性稽核工具
|||         
            
<br />          

## <a name="windows-event-log"></a>Windows 事件日誌            
| | |           
|---|---|       
|   **服務描述** |   此服務會管理事件和事件記錄檔。 它支援記錄事件，查詢訂閱事件、 封存事件記錄檔，和管理事件的中繼資料的事件。 它可以用 XML 和純文字格式顯示事件。 停止此服務可能會危及安全性和系統的可靠性。
|   **服務名稱**    |   EventLog
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Windows 防火牆         
| | |           
|---|---|   
|   **服務描述** |   Windows 防火牆可協助保護您的電腦存取您的電腦透過網際網路或網路時，防止未經授權的使用者。
|   **服務名稱**    |   MpsSvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Windows 字型快取服務      
| | |           
|---|---|   
|   **服務描述** |   藉由快取資料常使用的字型，可最佳化應用程式的效能。 如果尚未執行，應用程式會啟動此服務。 它可以停用，但這麼做會降低應用程式的效能。
|   **服務名稱**    |   FontCache
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Windows 映像擷取 (WIA)          
| | |           
|---|---|   
|   **服務描述** |   提供掃描器與數位相機的映像擷取服務
|   **服務名稱**    |   stisvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Windows 測試人員服務     
| | |           
|---|---|   
|   **服務描述** |   wisvc
|   **服務名稱**    |   wisvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   伺服器不支援試驗，因此它不是在伺服器上執行任何作業。 功能可以停用透過 GP 以及。
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **服務描述** |   新增、 修改及移除 Windows Installer （*.msi，*.msp） 套件的形式提供應用程式。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   msiserver
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Windows 授權管理員服務          
| | |           
|---|---|   
|   **服務描述** |   提供 Microsoft Store 的基礎結構支援。  此服務在要求時啟動，並停用然後透過 Microsoft Store 取得的內容將無法正確運作。
|   **服務名稱**    |   LicenseManager
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation       
| | |           
|---|---|       
|   **服務描述** |   提供有關作業系統、 裝置、 應用程式和服務的存取管理資訊一般介面和物件模型。 如果此服務停止時，大部分以 Windows 為基礎的軟體將無法正常運作。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   Winmgmt
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Windows 行動熱點服務          
| | |           
|---|---|       
|   **服務描述** |   提供與另一個裝置共用行動電話資料連線能力。
|   **服務名稱**    |   icssvc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Windows Modules Installer        
| | |           
|---|---|   
|   **服務描述** |   可讓安裝、 修改和移除 Windows 更新和選用元件。 如果此服務已停用，安裝或解除安裝的更新可能會失敗，此電腦的 Windows。
|   **服務名稱**    |   TrustedInstaller
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Windows 推播通知系統服務            
| | |           
|---|---|
|   **服務描述** |   此服務會在工作階段 0 中執行，並裝載通知平台和連線提供者用來處理裝置與 WNS 伺服器之間的連線。
|   **服務名稱**    |   WpnService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   若要停用 [確定]
|   **註解**    |   所需的動態磚和其他功能
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Windows 推播通知使用者服務          
| | |           
|---|---|   
|   **服務描述** |   此服務會裝載 Windows 通知平台可支援本機和推播通知。 支援的通知會並排顯示快顯和 raw。
|   **服務名稱**    |   WpnUserService
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   若要停用 [確定]
|   **註解**    |   使用者服務範本
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Windows 遠端管理 (Ws-management)            
| | |           
|---|---|   
|   **服務描述** |   Windows 遠端管理 (WinRM) 服務實作了 WS-Management 通訊協定來執行遠端管理。 WS-Management 是可用於管理遠端軟體及硬體的標準 Web 服務通訊協定。 WinRM 服務會接聽並處理網路上的 WS-Management 要求。 您必須使用 winrm.cmd 命令列工具或透過群組原則，為 WinRM 服務設定接聽程式，如此 WinRM 服務才能執行網路接聽工作。 WinRM 服務提供 WMI 資料的存取權及事件收集的功能。 這項服務必須在執行中，才能執行事件收集及訂閱事件。 WinRM 訊息使用 HTTP 及 HTTPS 進行傳輸。 WinRM 服務與 IIS 並無依存關係，但會預先設定成與 IIS 並用同一部機器上的同一個連接埠。  WinRM 服務會保留 /wsman URL 首碼。 為避免與 IIS 發生衝突，系統管理員應確定 IIS 上所代管的網站未使用 /wsman URL 首碼。
|   **服務名稱**    |   WinRM
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   所需的遠端管理
|||         
            
<br />          

##  <a name="windows-search"></a>Windows 搜尋      
| | |           
|---|---|       
|   **服務描述** |   提供的檔案、 電子郵件和其他內容的內容編製索引、 屬性快取，以及搜尋結果。
|   **服務名稱**    |   WSearch
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已停用
|   **註解**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Windows Time        
| | |           
|---|---|   
|   **服務描述** |   會維護所有用戶端上的日期和時間同步處理和網路中的伺服器。 如果此服務停止時，將無法使用日期和時間同步處理。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   W32Time
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引
|   **註解**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **服務描述** |   可讓偵測、 下載及安裝更新 Windows 和其他程式。 如果此服務已停用，這部電腦的使用者不能使用 Windows Update 或其自動更新功能和程式將無法使用 Windows Update Agent (WUA) API。
|   **服務名稱**    |   wuauserv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>WinHTTP Web Proxy 自動探索服務         
| | |           
|---|---|   
|   **服務描述** |   WinHTTP 實作的用戶端的 HTTP 堆疊，並提供開發人員使用 Win32 API 和 COM Automation 元件傳送的 HTTP 要求和接收的回應。 此外，WinHTTP 提供支援自動探索透過其實作的 Web Proxy 自動探索 (WPAD) 通訊協定的 proxy 設定。
|   **服務名稱**    |   WinHttpAutoProxySvc
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   使用網路堆疊的任何項目會對此服務的功能相依性。 許多組織依賴此資訊才能設定其內部網路的 HTTP proxy 路由。  未安裝，在內部源自網際網路的 HTTP 連接將會全部失敗。
|||         
            
<br />          

## <a name="wired-autoconfig"></a>有線自動設定         
| | |           
|---|---|       
|   **服務描述** |   有線自動設定 (DOT3SVC) 服務會負責執行 IEEE 802.1x 乙太網路介面上的驗證。 如果您目前的有線的網路部署強制執行 802.1x 驗證，就應該設定 DOT3SVC 服務來執行，建立第 2 層連線，及/或提供存取網路資源。 不會強制執行 802.1x 驗證的有線的網路會受到 DOT3SVC 服務。
|   **服務名稱**    |   dot3svc
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>WMI 效能配接器          
| | |           
|---|---|   
|   **服務描述** |   提供從 Windows Management Instrumentation (WMI) 提供者的效能程式庫資訊，在網路上的用戶端。 效能資料協助程式啟動時，只會執行這項服務。
|   **服務名稱**    |   wmiApSrv
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引       
|   **註解**    |   
|||         
            
<br />          

## <a name="workstation"></a>工作站          
| | |           
|---|---|   
|   **服務描述** |   建立並維護使用 SMB 通訊協定的遠端伺服器的用戶端網路連線。 如果此服務停止時，將無法使用這些連線。 如果已停用這項服務，任何明確依存於其的服務將無法啟動。
|   **服務名稱**    |   LanmanWorkstation
|   **安裝**    |   永遠都會安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引       
|   **註解**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live 的驗證管理員           
| | |           
|---|---|   
|   **服務描述** |   提供驗證和授權服務互動 Xbox Live。 如果此服務停止時，某些應用程式可能無法正常運作。
|   **服務名稱**    |   XblAuthManager
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   應該停用
|   **註解**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Xbox Live 的遊戲儲存          
| | |           
|---|---|   
|   **服務描述** |   此服務同步處理會儲存已啟用的遊戲，將資料儲存 Xbox live。  如果此服務停止時，將資料儲存的遊戲不會將上傳至，或從 Xbox Live 下載。
|   **服務名稱**    |   XblGameSave
|   **安裝**    |   只含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   應該停用
|   **註解**    |   此服務同步處理會儲存已啟用的遊戲，將資料儲存 Xbox live。  如果此服務停止時，將資料儲存的遊戲不會將上傳至，或從 Xbox Live 下載。
|||         
                
<br /> 
<br /> 

