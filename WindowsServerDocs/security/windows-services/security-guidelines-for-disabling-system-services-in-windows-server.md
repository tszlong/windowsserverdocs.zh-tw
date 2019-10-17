---
title: Windows Server 2016 中系統服務的安全性方針
description: 關於在包含桌面體驗的 Windows Server 2016 中停用服務的安全性方針
ms.custom: na
ms.prod: windows-server
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 175c4dbd23bac1822365ce80f05d69509d27321c
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935027"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>在包含桌面體驗的 Windows Server 2016 上停用系統服務的指引

適用於：Windows Server 2016

Windows 作業系統包含許多可提供重要功能的系統服務。 不同的服務有不同的預設啟動原則：有些預設會啟動 (自動)，有些會在需要時啟動 (手動)，有些預設會停用且必須先明確地啟用之後才能執行。 已針對每個服務仔細選擇這些預設值，以平衡一般客戶的效能、功能和安全性。

不過，有些企業客戶可能想要針對其 Windows 電腦和伺服器達成更專注於安全性的平衡，以將其攻擊面降至絕對最小值，因此可能希望完全停用其特定服務環境中不需要的所有服務。 對於那些客戶，Microsoft® 隨附如何基於此目的安全停用哪些服務的相關指引。

本指引僅適用於包含桌面體驗的 Windows Server 2016 (除非用來作為終端使用者的桌面取代項目)。 從 Windows Server 2019 開始，預設會設定這些方針。 系統上每個服務的分類方式如下：

-   **應該停用：** 專注於安全性的企業最有可能想要停用此服務並放棄其功能 (請參閱下列其他詳細資料)。
- **確定停用：** 此服務提供適用於部分企業而非所有企業的功能，而對於專注於安全性的企業，若未用到此服務，即可安全地停用它。
- **請勿停用：** 停用此服務將對重要功能產生影響，或是讓特定角色或功能無法正確運作。 因此，不應停用此服務。
-  **(沒有指引)：** 尚未完整評估停用這些服務的影響。 因此，不應變更這些服務的預設設定。


客戶可以設定他們的 Windows 電腦和伺服器，以使用其群組原則中的安全性範本或使用 PowerShell 自動化來停用所選取的服務。 在某些情況下，本指引會包含特定的群組原則設定作為停用服務本身的替代方案，以直接停用服務的功能。

Microsoft 建議客戶在包含桌面體驗的 Windows Server 2016 上停用下列服務及其各自排定的工作：

服務： 
1. Xbox Live Auth Manager
2. Xbox Live Game Save

排定的工作： 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(您也可以檢視附加的 Microsoft Excel 試算表，來存取本文詳述之所有服務的相關資訊：[在包含桌面體驗的 Windows Server 2016 上停用系統服務的指引](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx) \(英文\))

<br />

### <a name="disabling-services-not-installed-by-default"></a>停用預設未安裝的服務

Microsoft 建議不要套用原則來停用預設未安裝的服務。
-  如果安裝功能，通常就需要服務。 安裝服務或功能需要系統管理權限。 不允許功能安裝，而非服務啟動。
-  封鎖 Microsoft Windows 服務不會阻止系統管理員 (或者在某些情況下為非系統管理員) 安裝類似第三方對等項目，可能是具較高安全性風險的對等項目。
-  停用非預設 Windows 服務 (例如 W3SVC) 的基準或效能評定，將為一些稽核員給予該技術 (例如 IIS) 原本就不安全且不應使用的錯誤印象。
-  如果從未安裝功能 (和服務)，這只會大規模新增不必要的基準和驗證工作。

<br />
針對本文件列出的所有系統服務，下列這兩個表格均提供在包含桌面體驗的 Windows Server 2016 中啟用和停用系統服務的欄及 Microsoft 建議的說明： 

<br />

### <a name="explanation-of-columns"></a>欄說明

| | |
|---|---|
|**服務描述**|   服務的描述 (來自 sc.exe qdescription)。|
|**名稱** |服務的金鑰 (內部) 名稱|
|**安裝** |必須安裝：服務位於 Server Core 及包含桌面體驗的伺服器上  <br /> 僅包含桌面體驗：服務位於包含桌面體驗的 Windows Server 2016 上，但***不***在 Server Core 上 |
|**StartType**  |Windows Server 2016 上的服務啟動類型|
|**建議** |Microsoft 建議/通知在管理完善的典型企業部署中，在未將伺服器用來作為終端使用者桌面取代項目的情況下，於 Windows Server 2016 上停用此服務。|
|**註解** |其他說明|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Microsoft 建議的說明

| | |
|---|---|
|**請勿停用** |不應停用此服務|
|**確定停用**| 如果未使用服務所支援的功能，即可停用此服務。|
|**已經停用**|  此服務預設會停用；不需透過原則強制執行|
|**應該停用** |永遠都不應在管理完善的企業系統上啟用此服務。|

<br />

下列表格提供在包含桌面體驗的 Windows Server 2016 上停用系統服務的 Microsoft 指引：

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX 安裝程式 (AxInstSV)

| | |
|---|---|
|   **服務描述** |   提供使用者帳戶控制驗證以從網際網路安裝 ActiveX 控制項，並且能夠根據群組原則設定來管理 ActiveX 控制項安裝。 此服務會依需求啟動，而且如果停用，ActiveX 控制項的安裝將根據預設瀏覽器設定來動作。    |
|   **服務名稱**    |   AxInstSV    |
|   **安裝**    |   僅包含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  |   確定停用   |
|   **註解**    |   如果不需要功能，則確定停用 |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn 路由器服務   

| | |
|---|---|
|   **服務描述** |   路由傳送適用於本機 AllJoyn 用戶端的 AllJoyn 訊息。 如果停止此服務，沒有自己的繫結路由器的 AllJoyn 用戶端將無法執行。 |
|   **服務名稱**    |   AJRouter    |
|   **安裝**    |   僅包含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  | 沒有指引       |
|   **註解**    |       |
| | |

<br />

## <a name="app-readiness"></a>應用程式整備

| | |
|---|---|
**服務描述** |   備妥應用程式，供使用者第一次登入此電腦和新增應用程式時使用。
**服務名稱**    |   AppReadiness
**安裝**    |   僅包含桌面體驗
**StartType**   |   Manual
**建議**  |   請勿停用
**註解**    |   
| | |

<br />

##  <a name="application-identity"></a>應用程式識別碼

| | |       
|---|---|   
**服務描述** |   判斷並確定應用程式的身分識別。 停用此服務將讓 AppLocker 無法強制執行。
**服務名稱**    |   AppIDSvc
**安裝**    |   必須安裝
**StartType**   |   Manual
**建議**  |沒有指引    
**註解**    |   
|||     

<br />

##  <a name="application-information"></a>應用程式資訊 

| | |       
|---|---|   
|   **服務描述** |   以其他系統管理權限協助執行互動式應用程式。  使用者在執行想要的工作時可能需要這些權限。如果停止此服務，使用者將無法以其他管理權限來啟動應用程式。
|   **服務名稱**    |   Appinfo
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   支援與 UAC 相同的桌面提高權限
|||     

<br />

##  <a name="application-layer-gateway-service"></a>應用程式層閘道服務       

| | |           
|---|---|           
|   **服務描述** |   針對網際網路連線共用提供協力廠商通訊協定外掛程式的支援
|   **服務名稱**    |   ALG
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |沒有指引    
|   **註解**    |   
|||     

<br />

##  <a name="application-management"></a>應用程式管理      

| | |           
|---|---|       
|   **服務描述** |   針對透過群組原則部署的軟體處理安裝、移除和列舉要求。 如果停用此服務，使用者將無法安裝、移除或列舉透過群組原則部署的軟體。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   AppMgmt
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="appx-deployment-service-appxsvc"></a>AppX 部署服務 (AppXSVC)       

| | |           
|---|---|
|   **服務描述** |   提供用來部署市集應用程式的基礎結構支援。 此服務會依需求啟動，而且如果停用，市集應用程式將不會部署到系統且可能無法正常運作。
|   **服務名稱**    |   AppXSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="auto-time-zone-updater"></a>自動時區更新程式           

| | |           
|---|---|           
|   **服務描述** |   自動設定系統時區。
|   **服務名稱**    |   tzautoupdate
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="background-intelligent-transfer-service"></a>背景智慧型傳送服務          

| | |           
|---|---|   
|   **服務描述** |   使用閒置的網路頻寬，在背景中傳輸檔案。 如果停用此服務，則任何相依於 BITS (例如 Windows Update 或 MSN Explorer) 的應用程式都將無法自動下載程式和其他資訊。
|   **服務名稱**    |   BITS
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          


## <a name="background-tasks-infrastructure-service"></a>背景工作基礎結構服務      

| | |           
|---|---|   
|   **服務描述** |   Windows 基礎結構服務，控制哪些背景工作可以在系統上執行。
|   **服務名稱**    |   BrokerInfrastructure
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="base-filtering-engine"></a>基礎篩選引擎            

| | |           
|---|---|       
|   **服務描述** |   基礎篩選引擎 (BFE) 是一個服務，可管理防火牆與網際網路通訊協定安全性 (IPsec) 原則，以及實作使用者模式篩選。 停止或停用 BFE 服務將大幅降低系統的安全性。 它也將在 IPsec 管理和防火牆應用程式中導致無法預期的行為。
|   **服務名稱**    |   BFE
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="bluetooth-support-service"></a>藍芽支援服務            

| | |           
|---|---|   
|   **服務描述** |   藍牙服務支援遠端藍芽裝置的探索和關聯。  停止或停用此服務可能導致已安裝的藍芽裝置無法正常運作，並防止探索或關聯新的裝置。
|   **服務名稱**    |   bthserv
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   若未用到，則確定停用。 另一個停用機制： https://technet.microsoft.com/library/dd252791.aspx
|||         

<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           

| | |           
|---|---|   
|   **服務描述** |   此使用者服務適用於連線的裝置平台案例
|   **服務名稱**    |   CDPUserSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />          


##  <a name="certificate-propagation"></a>憑證傳播     

| | |           
|---|---|
|   **服務描述** |   將使用者憑證和根憑證從智慧卡複製到目前使用者的憑證存放區、偵測何時將智慧卡插入智慧卡讀卡機，並視需要安裝智慧卡隨插即用迷你驅動程式。
|   **服務名稱**    |   CertPropSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="client-license-service-clipsvc"></a>用戶端授權服務 (ClipSVC)        

| | |           
|---|---|   
|   **服務描述** |   提供 Microsoft Store 的基礎結構支援。 此服務會依需求啟動，如果停用，則使用 Microsoft Store 購買的應用程式將無法正確運作。
|   **服務名稱**    |   ClipSVC
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="cng-key-isolation"></a>CNG 金鑰隔離

| | |           
|---|---|   
|   **服務描述** |   CNG 金鑰隔離服務裝載於 LSA 處理序。 該服務可依據通用條件來隔離私密金鑰和相關聯密碼編譯作業的金鑰處理序。 該服務會以符合通用條件需求的安全處理序來儲存和使用長效金鑰。
|   **服務名稱**    |   KeyIso
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="com-event-system"></a>COM+ 事件系統       

| | |           
|---|---|       
|   **服務描述** |   支援系統事件通知服務 (SENS)，它可讓事件自動分散到訂閱的元件物件模型 (COM) 元件。 如果停止此服務，SENS 將會關閉且將無法提供登入及登出通知。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   EventSystem
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="com-system-application"></a>COM+ 系統應用程式     

| | |           
|---|---|       
|   **服務描述** |   管理以元件物件模型 (COM)+ 為基礎之元件的設定和追蹤。 如果停止此服務，大部分以 COM+ 為基礎的元件都將無法正確運作。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   COMSysApp
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="computer-browser"></a>電腦瀏覽器        

| | |           
|---|---|       
|   **服務描述** |   維護網路上更新的電腦清單，並將這份清單提供給指定為瀏覽器的電腦。 如果停止此服務，將不會更新或維護這份清單。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   瀏覽器
|   **安裝**    |   必須安裝
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="connected-devices-platform-service"></a>已連線的裝置平台服務       

| | |           
|---|---|       
|   **服務描述** |   此服務適用於「已連線的裝置」與 Universal Glass 案例
|   **服務名稱**    |   CDPSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="connected-user-experiences-and-telemetry"></a>已連線使用者體驗與遙測     

| | |           
|---|---|       
|   **服務描述** |   已連線使用者體驗與遙測服務可啟用支援應用程式內與已連線使用者體驗的功能。 此外，在 [意見反應與診斷] 下啟用診斷與使用方式隱私權選項設定時，此服務會管理診斷與使用方式資訊 (用於改進 Windows 平台的體驗與品質) 的事件導向收集與傳輸。
|   **服務名稱**    |   DiagTrack
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="contact-data"></a>連絡人資料        

| | |           
|---|---|       
|   **服務描述** |   為連絡人資料編製索引，以加快搜尋連絡人的速度。 如果您停止或停用此服務，則搜尋結果中可能會遺漏連絡人資訊。
|   **服務名稱**    |   PimIndexMaintenanceSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />          

## <a name="coremessaging"></a>CoreMessaging            

| | |           
|---|---|           
|   **服務描述** |   管理系統元件之間的通訊。
|   **服務名稱**    |   CoreMessagingRegistrar
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="credential-manager"></a>認證管理員           

| | |           
|---|---|       
|   **服務描述** |   讓使用者、應用程式及安全性服務套件能夠安全地儲存與擷取認證。
|   **服務名稱**    |   VaultSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="cryptographic-services"></a>密碼編譯服務           

| | |           
|---|---|       
|   **服務描述** |   提供三種管理服務：目錄資料庫服務：確認 Windows 檔案的簽章並允許安裝新程式；受保護的根目錄服務：從這部電腦新增及移除可信任的根憑證授權單位憑證；以及自動根憑證更新服務：從 Windows Update 擷取根憑證並啟用 SSL 之類的案例。 如果停止此服務，這些管理服務都將無法正常運作。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   CryptSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="data-sharing-service"></a>資料共用服務         

| | |           
|---|---|       
|   **服務描述** |   提供應用程式之間的資料代理。
|   **服務名稱**    |   DsSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          

| | |           
|---|---|       
|   **服務描述** |   DCP (資料收集與發佈) 服務支援第一方應用程式將資料上傳至雲端。
|   **服務名稱**    |   DcpSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="dcom-server-process-launcher"></a>DCOM 伺服器處理序啟動器         

| | |           
|---|---|       
|   **服務描述** |   DCOMLAUNCH 服務會啟動 COM 和 DCOM 伺服器，以回應物件啟用要求。 如果停止或停用此服務，使用 COM 或 DCOM 的程式將無法正常運作。 強烈建議您持續執行 DCOMLAUNCH 服務。
|   **服務名稱**    |   DcomLaunch
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |沒有指引    
|   **註解**    |   
|||         

<br />

##  <a name="device-association-service"></a>裝置關聯服務      

| | |           
|---|---|       
|   **服務描述** |   啟用系統與有線或無線裝置之間的配對。
|   **服務名稱**    |   DeviceAssociationService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />

##  <a name="device-install-service"></a>裝置安裝服務

| | |
|---|---|
|   **服務描述** |   讓電腦能夠利用少許或沒有任何的使用者輸入來識別及適應硬體變更。 停止或停用此服務將導致系統不穩定。
|   **服務名稱**    |   DeviceInstall
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引
|   **註解**    |
|||

<br />          

##  <a name="device-management-enrollment-service"></a>裝置管理註冊服務        

| | |           
|---|---|       
|   **服務描述** |   針對裝置管理執行裝置註冊活動
|   **服務名稱**    |   DmEnrollmentSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="device-setup-manager"></a>裝置設定管理員         

| | |           
|---|---|       
|   **服務描述** |   啟用裝置相關軟體的偵測、下載及安裝。 如果停用此服務，裝置可能會使用過期的軟體來設定且可能無法正確運作。
|   **服務名稱**    |   DsmSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="devquery-background-discovery-broker"></a>DevQuery 背景探索代理人         

| | |           
|---|---|           
|   **服務描述** |   讓應用程式能夠使用背景工作來探索裝置
|   **服務名稱**    |   DevQueryBroker
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="dhcp-client"></a>DHCP 用戶端          

| | |           
|---|---|       
|   **服務描述** |   為這部電腦註冊及更新 IP 位址和 DNS 記錄。 如果停止此服務，這部電腦將不會接收動態 IP 位址和 DNS 更新。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   Dhcp
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="diagnostic-policy-service"></a>診斷原則服務            

| | |           
|---|---|       
|   **服務描述** |   診斷原則服務能夠針對 Windows 元件進行問題偵測、疑難排解並提供解決方案。  如果停止此服務，診斷將不再運作。
|   **服務名稱**    |   DPS
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="diagnostic-service-host"></a>診斷服務裝載     

| | |           
|---|---|       
|   **服務描述** |   診斷原則服務會使用診斷服務裝載，來裝載需要在本機服務內容上執行的診斷。  如果停止此服務，則與其相依的任何診斷都將不再運作。
|   **服務名稱**    |   WdiServiceHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="diagnostic-system-host"></a>診斷系統裝載           

| | |           
|---|---|       
|   **服務描述** |   診斷原則服務會使用診斷系統裝載，來裝載需要在本機系統內容上執行的診斷。  如果停止此服務，則與其相依的任何診斷都將不再運作。
|   **服務名稱**    |   WdiSystemHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="distributed-link-tracking-client"></a>散佈式連結追蹤用戶端            

| | |           
|---|---|   
|   **服務描述** |   在某部電腦內或跨網路中的不同電腦上，維護 NTFS 檔案之間的連結。
|   **服務名稱**    |   TrkWks
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="distributed-transaction-coordinator"></a>分散式交易協調器     

| | |           
|---|---|   
|   **服務描述** |   協調跨越多個資源管理員的交易，例如資料庫、訊息佇列及檔案系統。 如果停止此服務，這些交易都將失敗。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   MSDTC
|   **安裝**    |   必須安裝
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
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   用戶端裝置上需要針對 Intune、MDM 和類似管理技術，以及針對統一寫入篩選器使用的服務。 不需針對伺服器使用。
|||         

<br />      

##  <a name="dns-client"></a>DNS 用戶端      

| | |           
|---|---|       
|   **服務描述** |   DNS 用戶端服務 (dnscache) 會快取網域名稱系統 (DNS) 名稱，並會為此電腦註冊完整的電腦名稱。 如果服務已停止，會繼續解析 DNS 名稱。 但將不會快取 DNS 名稱查詢的結果，而且將不會註冊電腦的名稱。 如果已停用該服務，所有明確依賴該服務的所有服務，將無法啟動。
|   **服務名稱**    |   Dnscache
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="downloaded-maps-manager"></a>已下載的地圖管理員     

| | |           
|---|---|   
|   **服務描述** |   適用於應用程式存取已下載地圖的 Windows 服務。 要存取已下載地圖的應用程式會依需求啟動此服務。 停用此服務將讓應用程式無法存取地圖。
|   **服務名稱**    |   MapsBroker
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   停用中斷依賴服務的應用程式；如果應用程式未依賴它，則確定停用
|||         

<br />          

## <a name="embedded-mode"></a>內嵌模式            

| | |           
|---|---|       
|   **服務描述** |   內嵌模式服務可啟用與背景應用程式相關的案例。  停用此服務將防止背景應用程式啟用。
|   **服務名稱**    |   embeddedmode
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="encrypting-file-system-efs"></a>加密檔案系統 (EFS)

| | |                   
|---|---|           
|   **服務描述** | 提供核心檔案加密技術，以用來在 NTFS 檔案系統磁碟區上儲存加密的檔案。 如果停止或停用此服務，應用程式將無法存取加密的檔案。            
|   **服務名稱**  |  EFS            
|   **安裝**  |  必須安裝           
|   **StartType**   |  Manual           
|   **建議**  | 沒有指引           
|   **註解**   |
|||                 

<br />  

## <a name="enterprise-app-management-service"></a>企業應用程式管理服務            

| | |           
|---|---|       
|   **服務描述** |   啟用企業應用程式管理。
|   **服務名稱**    |   EntAppSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="extensible-authentication-protocol"></a>可延伸的驗證通訊協定           

| | |           
|---|---|   
|   **服務描述** |   可延伸的驗證通訊協定 (EAP) 服務會在 802.1x 有線和無線、VPN 及網路存取保護 (NAP) 之類案例中提供網路驗證。  EAP 也會在驗證程序期間，提供網路存取用戶端 (包括無線及 VPN 用戶端) 所使用的應用程式開發介面 (API)。  如果您停用此服務，這部電腦將無法存取需要 EAP 驗證的網路。
|   **服務名稱**    |   EapHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="function-discovery-provider-host"></a>功能探索提供者裝載         

| | |           
|---|---|       
|   **服務描述** |   FDPHOST 服務會裝載功能探索 (FD) 網路探索提供者。 這些 FD 提供者提供簡單服務探索通訊協定 (SSDP) 和 Web 服務 - 探索 (WS-D) 通訊協定的網路探索服務。 停止或停用 FDPHOST 服務，將會在使用 FD 時，停用這些通訊協定的網路探索。 無法使用此服務時，使用 FD 及依賴這些探索通訊協定的網路服務將找不到網路裝置或資源。
|   **服務名稱**    |   fdPHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="function-discovery-resource-publication"></a>功能探索資源發佈      

| | |           
|---|---|       
|   **服務描述** |   發佈這部電腦及連結至此電腦的資源，如此便可在網路上找到它們。  如果停止此服務，將不再發佈網路資源，而網路上的其他電腦將無法找到它們。
|   **服務名稱**    |   FDResPub
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="geolocation-service"></a>地理位置服務          

| | |           
|---|---|       
|   **服務描述** |   此服務會監視目前的系統位置並管理地理柵欄 (具有關聯事件的地理位置)。  如果關閉此服務，應用程式將無法用來或接收地理位置或地理柵欄的通知。
|   **服務名稱**    |   lfsvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   停用中斷依賴服務的應用程式；如果應用程式未依賴它，則確定停用
|||         

<br />          

##  <a name="group-policy-client"></a>群組原則用戶端     

| | |           
|---|---|       
|   **服務描述** |   此服務負責透過群組原則元件來套用系統管理員為電腦與使用者所設定的設定。 如果停用服務，將不會套用設定，而且將無法透過群組原則來管理應用程式與元件。 如果停用此服務，相依於群組原則元件的所有元件或應用程式可能都無法運作。
|   **服務名稱**    |   gpsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          


## <a name="human-interface-device-service"></a>人性化介面裝置服務           

| | |           
|---|---|       
|   **服務描述** |   啟用並維護鍵盤、遙控器及其他多媒體裝置上之常用按鈕的使用。 建議您讓此服務繼續執行。
|   **服務名稱**    |   hidserv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="hv-host-service"></a>HV 主機服務     

| | |           
|---|---|   
|   **服務描述** |   提供適用於 Hyper-V Hypervisor 的介面，以便為主機作業系統提供每個分割區的效能計數器。
|   **服務名稱**    |   HvHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   適用於客體 VM 的效能加強程式。 除了明確填入 VM 之外，目前並未使用，但將用於應用程式防護
|||         

<br />          

## <a name="hyper-v-data-exchange-service"></a>Hyper-V 資料交換服務        

| | |           
|---|---|   
|   **服務描述** |   提供一種機制，以便在虛擬機器及實體電腦上執行之作業系統間交換資料。
|   **服務名稱**    |   vmickvpexchange
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />      

## <a name="hyper-v-guest-service-interface"></a>Hyper-V 客體服務介面          

| | |           
|---|---|   
|   **服務描述** |   提供適用於 Hyper-V 主機的介面，以便與虛擬機器內執行的特定服務互動。
|   **服務名稱**    |   vmicguestinterface
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Hyper-V 客體關機服務           

| | |           
|---|---|       
|   **服務描述** |   提供一種機制，以從實體電腦上的管理介面關閉這部虛擬機器的作業系統。
|   **服務名稱**    |   vmicshutdown
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />

## <a name="hyper-v-heartbeat-service"></a>Hyper-V 活動訊號服務
| | |
|---|---|
|   **服務描述** |   定期報告活動訊號，藉以監視這部虛擬機器的狀態。 此服務可協助您識別已停止回應的執行中虛擬機器。
|   **服務名稱**    |   vmicheartbeat
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||

<br />          

## <a name="hyper-v-powershell-direct-service"></a>Hyper-V PowerShell Direct 服務            

| | |           
|---|---|       
|   **服務描述** |   提供一種機制，以便在沒有虛擬網路的情況下，透過 VM 工作階段搭配 PowerShell 來管理虛擬機器。
|   **服務名稱**    |   vmicvmsession
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Hyper-V 遠端桌面虛擬服務            

| | |           
|---|---|       
|   **服務描述** |   提供一個平台，以便在虛擬機器及實體電腦上執行的作業系統之間進行通訊。
|   **服務名稱**    |   vmicrdv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />          

## <a name="hyper-v-time-synchronization-service"></a>Hyper-V 時間同步化服務         

| | |           
|---|---|       
|   **服務描述** |   將這部虛擬機器的系統時間與實體電腦的系統時間同步。
|   **服務名稱**    |   vmictimesync
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Hyper-V 磁碟區陰影複製要求者         

| | |           
|---|---|           
|   **服務描述** |   協調使用磁碟區陰影複製服務所需的通訊，以便將應用程式與資料從實體電腦上的作業系統備份到這部虛擬機器上。
|   **服務名稱**    |   vmicvss
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   請參閱 HvHost
|||         

<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>IKE 和 AuthIP IPsec 金鑰處理模組          

| | |           
|---|---|       
|   **服務描述** |   IKEEXT 服務會裝載網際網路金鑰交換 (IKE) 和已驗證網際網路通訊協定 (AuthIP) 金鑰處理模組。 這些金鑰處理模組會用來在網際網路通訊協定安全性 (IPsec) 中進行驗證及金鑰交換。 停止或停用 IKEEXT 服務將會停用與對等電腦進行的 IKE 和 AuthIP 金鑰交換。 IPsec 通常會設定為使用 IKE 或 AuthIP；因此，停止或停用 IKEEXT 服務可能導致 IPsec 失敗，並且可能危害系統的安全性。 強烈建議您持續執行 IKEEXT 服務。
|   **服務名稱**    |   IKEEXT
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |    
|||         

<br />          

## <a name="interactive-services-detection"></a>互動式服務偵測           

| | |           
|---|---|   
|   **服務描述** |   針對互動式服務啟用使用者輸入的使用者通知，這樣就能在互動式服務所建立的對話方塊出現時存取它們。 如果停止此服務，新互動式服務對話方塊的通知將不再運作，而且可能無法存取互動式服務對話方塊。 如果停用此服務，新互動式服務對話方塊的通知及存取都將不再運作。
|   **服務名稱**    |   UI0Detect
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />  

## <a name="internet-connection-sharing-ics"></a>網際網路連線共用 (ICS)            

| | |           
|---|---|           
|   **服務描述** |   為家用網路或小型辦公室網路提供網路位址轉譯、定址、名稱解析和/或入侵防範服務。
|   **服務名稱**    |   SharedAccess
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   用來作為 WiFi 熱點用戶端需要，而在 Miracast 投影兩端也需要。 ICS 可以使用下列 GPO 設定來封鎖：「禁止在您的 DNS 網域網路中使用網際網路連線共用」
|||         

<br />          

## <a name="ip-helper"></a>IP 協助程式            

| | |           
|---|---|       
|   **服務描述** |   使用 IPv6 轉換技術 (6to4、ISATAP、連接埠 Proxy 及 Teredo) 和 IP-HTTPS 來提供通道連線。 如果停止此服務，電腦將不具備這些技術所提供的增強連線優點。
|   **服務名稱**    |   iphlpsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          


##  <a name="ipsec-policy-agent"></a>IPSec 原則代理程式      

| | |           
|---|---|       
|   **服務描述** |   網際網路通訊協定安全性 (IPsec) 支援網路層級的對等驗證、資料來源驗證、資料完整性、資料機密性 (加密)，以及重新執行保護。  此服務會強制執行透過 IP 安全性原則嵌入式管理單元或 " netsh ipsec" 命令列工具建立的 IPsec 原則。  如果您停止此服務，當您的原則需要使用 IPsec 連線時，可能會發生網路連線問題。  此外，在停止此服務時，也無法使用 Windows 防火牆的遠端管理。
|   **服務名稱**    |   PolicyAgent
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />

##  <a name="kdc-proxy-server-service-kps"></a>KDC Proxy 伺服器服務 (KPS)      

| | |           
|---|---|       
|   **服務描述** |   KDC Proxy 伺服器服務會在 Edge Server 上執行，以將 Kerberos 通訊協定訊息 Proxy 處理到公司網路上的網域控制站。
|   **服務名稱**    |   KPSSVC
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         

<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>用於分散式交易協調器的 KtmRm            

| | |           
|---|---|       
|   **服務描述** |   協調分散式交易協調器 (MSDTC) 與核心交易管理員 (KTM) 之間的交易。 如果不需要這樣做，建議您讓此服務維持停止狀態。 如果需要這樣做，MSDTC 與 KTM 都將自動啟動此服務。 如果停用此服務，任何與核心資源管理員互動的 MSDTC 交易都將失敗，而且明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   KtmRm
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />

##  <a name="link-layer-topology-discovery-mapper"></a>連結階層拓撲探索對應程式        

| | |       
|---|---|       
|   **服務描述** |   建立網路對應，其中包含電腦和裝置拓撲 (連線能力) 資訊，以及描述每部電腦和裝置的中繼資料。  如果停用此服務，網路對應將無法正常運作。
|   **服務名稱**    |   lltdsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   如果網路對應上沒有任何相依性，則確定停用
|||         

<br />

## <a name="local-session-manager"></a>本機工作階段管理員                    

| | |                   
|---|---|   
|   **服務描述** |   管理本機使用者工作階段的核心 Windows 服務。 停止或停用此服務將導致系統不穩定。    
|   **服務名稱**    |   LSM |
|   **安裝**    |   必須安裝    |
|   **StartType**   |   自動   |
|   **建議**  | 沒有指引   
|   **註解**    |   
|||                 

<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Microsoft (R) 診斷集線器標準收集器         

| | |           
|---|---|           
|   **服務描述** |   管理本機使用者工作階段的核心 Windows 服務。 停止或停用此服務將導致系統不穩定。
|   **服務名稱**    |   LSM
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />

## <a name="microsoft-account-sign-in-assistant"></a>Microsoft 帳戶登入小幫手
| | |
|---|---|
|   **服務描述** |   可讓使用者透過 Microsoft 帳戶身分識別服務登入。 如果停止此服務，使用者將無法使用他們的 Microsoft 帳戶登入電腦。
|   **服務名稱**    |   wlidsvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   Microsoft 帳戶在 Windows Server 上為 N/A
|||

<br />          

##  <a name="microsoft-app-v-client"></a>Microsoft App-V 用戶端      

| | |           
|---|---|       
|   **服務描述** |   管理 App-V 使用者和虛擬應用程式
|   **服務名稱**    |   AppVClient
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Microsoft iSCSI 啟動器服務            

| | |           
|---|---|       
|   **服務描述** |   管理從這部電腦至遠端 iSCSI 目標裝置的 Internet SCSI (iSCSI) 工作階段。 如果停止此服務，這部電腦將無法登入或存取 iSCSI 目標。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   MSiSCSI
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   我們的診斷資料指出這適用於用戶端以及伺服器上。 停用此服務沒有任何好處。
|||         

<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           

| | |           
|---|---|   
|   **服務描述** |   針對用於驗證使用者關聯身分識別提供者的密碼編譯金鑰提供處理序隔離。 如果停用此服務，將無法使用及管理這些金鑰，包括電腦登入及應用程式與網站的單一登入。 此服務會自動啟動和停止。 建議您不要重新設定此服務。
|   **服務名稱**    |   NgcSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   為伺服器上不支援之 PIN/Hello 登入的必要項
|||         

<br />          

## <a name="microsoft-passport-container"></a>Microsoft Passport 容器         

| | |           
|---|---|       
|   **服務描述** |   管理用來向身分識別提供者驗證使用者的本機使用者身分識別金鑰，以及 TPM 虛擬智慧卡。 如果停用此服務，將無法存取本機使用者身分識別金鑰與 TPM 虛擬智慧卡。 建議您不要重新設定此服務。
|   **服務名稱**    |   NgcCtnrSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Microsoft 軟體陰影複製提供者          

| | |           
|---|---|       
|   **服務描述** |   管理磁碟區陰影複製服務所採用之以軟體為基礎的磁碟區陰影複製。 如果停止此服務，就無法管理以軟體為基礎的磁碟區陰影複製。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   swprv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="microsoft-storage-spaces-smp"></a>Microsoft 儲存空間 SMP         

| | |           
|---|---|       
|   **服務描述** |   裝載適用於 Microsoft 儲存空間管理提供者的服務。 如果停止或停用此服務，將無法管理儲存空間。
|   **服務名稱**    |   smphost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   如果沒有此服務，儲存管理 API 就會失敗。 範例："Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage"。
|||         

<br />          

## <a name="nettcp-port-sharing-service"></a>Net.Tcp 連接埠共用服務         

| | |           
|---|---|       
|   **服務描述** |   提供透過 net.tcp 通訊協定共用 TCP 連接埠的能力。
|   **服務名稱**    |   NetTcpPortSharing
|   **安裝**    |   必須安裝
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="netlogon"></a>Netlogon         

| | |           
|---|---|           
|   **服務描述** |   在這部電腦與網域控制站之間維持一個安全通道，以用來驗證使用者和服務。 如果停止此服務，電腦可能就不會驗證使用者和服務，而且網域控制站無法註冊 DNS 記錄。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   Netlogon
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="network-connection-broker"></a>網路連線代理人            

| | |           
|---|---|       
|   **服務描述** |   允許 Microsoft Store 應用程式從網際網路接收通知的代理人連線。
|   **服務名稱**    |   NcbService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

##  <a name="network-connections"></a>網路連線         

| | |           
|---|---|   
|   **服務描述** |   管理 [網路] 和 [撥號連線] 資料夾中的物件，您可以在其中檢視區域網路和遠端連線。
|   **服務名稱**    |   Netman
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="network-connectivity-assistant"></a>網路連線助理      

| | |           
|---|---|       
|   **服務描述** |   提供 UI 元件的 DirectAccess 狀態通知
|   **服務名稱**    |   NcaSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />  

##  <a name="network-list-service"></a>網路清單服務        

| | |           
|---|---|   
|   **服務描述** |   識別電腦已連線的網路、收集並儲存這些網路的屬性，並在這些屬性變更時通知應用程式。
|   **服務名稱**    |   netprofm
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="network-location-awareness"></a>網路定位知悉           

| | |           
|---|---|       
|   **服務描述** |   收集並儲存網路的設定資訊，且在修改此資訊時通知程式。 如果停止此服務，設定資訊可能就無法使用。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   NlaSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="network-setup-service"></a>網路設定服務       

| | |           
|---|---|       
|   **服務描述** |   網路設定服務會管理網路驅動程式的安裝，並允許設定低階網路設定。  如果停止此服務，可能就會取消所有進行中的驅動程式安裝。
|   **服務名稱**    |   NetSetupSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="network-store-interface-service"></a>網路儲存介面服務      

| | |           
|---|---|   
|   **服務描述** |   此服務可將網路通知 (例如新增/刪除介面等) 傳遞給使用者模式的用戶端。 停止此服務將導致網路連線中斷。 如果停用此服務，明確依存於此服務的所有其他服務都將無法啟動。
|   **服務名稱**    |   nsi
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="offline-files"></a>離線檔案            

| | |           
|---|---|       
|   **服務描述** |   離線檔案服務能在離線檔案快取上執行維護活動、回應使用者登入和登出事件、實作公用 API 內部，以及將有趣的事件分派給對離線檔案活動及快取狀態的變更感興趣的使用者。
|   **服務名稱**    |   CscService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="optimize-drives"></a>最佳化磁碟機          

| | |           
|---|---|   
|   **服務描述** |   將儲存磁碟機上的檔案最佳化，有助於提高電腦執行效率。
|   **服務名稱**    |   defragsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />

## <a name="performance-counter-dll-host"></a>效能計數器 DLL主機         

| | |           
|---|---|       
|   **服務描述** |   允許遠端使用者和 64 位元處理序查詢 32 位元 DLL 所提供的效能計數器。 如果停止此服務，則只有本機使用者和 32 位元處理序可以查詢 32 位元 DLL 所提供的效能計數器。
|   **服務名稱**    |   PerfHost
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         

<br />          

## <a name="performance-logs--alerts"></a>效能記錄及警示            

| | |           
|---|---|   
|   **服務描述** |   效能記錄及警示會根據預先設定的排程參數，從本機或遠端電腦收集效能資料，然後將資料寫入到記錄或觸發警示。 如果停止此服務，將不會收集效能資訊。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   pla
|   **安裝**    |   必須安裝
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
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   由新式 VoIP 應用程式所使用
|||         

<br />          

##      <a name="plug-and-play"></a>隨插即用       

| | |           
|---|---|   
|   **服務描述** |   讓電腦能夠利用少許或沒有任何的使用者輸入來識別及適應硬體變更。 停止或停用此服務將導致系統不穩定。
|   **服務名稱**    |   PlugPlay
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="portable-device-enumerator-service"></a>可攜式裝置列舉程式服務           

| | |           
|---|---|       
|   **服務描述** |   針對抽取式大型存放裝置強制執行群組原則。 讓 Windows Media Player 及影像匯入精靈等應用程式能夠使用抽取式大型存放裝置來傳輸並同步處理內容。
|   **服務名稱**    |   WPDBusEnum
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="power"></a>電源            

| | |           
|---|---|       
|   **服務描述** |   管理電源原則與電源原則通知傳遞。
|   **服務名稱**    |   電源
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="print-spooler"></a>列印多工緩衝處理器            

| | |           
|---|---|   
|   **服務描述** |   此服務會多工緩衝處理列印工作，並處理與印表機的互動。  如果您關閉此服務，將無法列印或看見您的印表機。
|   **服務名稱**    |   多工緩衝處理器
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |   如果不是列印伺服器或 DC，則確定停用
|   **註解**    |   在網域控制站上，安裝 DC 角色會將執行緒新增至負責執行列印剪除的多工緩衝處理器服務：從 Active Directory 中移除過時的列印佇列物件。  如果多工緩衝處理器服務未在每個網站中至少一個 DC 上執行，則 AD 就無法移除不再存在的舊佇列。 https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         

<br />          

##  <a name="printer-extensions-and-notifications"></a>印表機擴充功能與通知        

| | |           
|---|---|       
|   **服務描述** |   此服務會開啟自訂印表機對話方塊，以及處理來自遠端列印伺服器或印表機的通知。 如果您關閉此服務，將無法查看印表機擴充功能或通知。
|   **服務名稱**    |   PrintNotify
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   如果不是列印伺服器，則確定停用
|   **註解**    |   
|||         

<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>問題報告及解決方案控制台支援     

| | |           
|---|---|   
|   **服務描述** |   此服務提供對檢視、傳送及刪除 [問題報告及解決方案] 控制台之系統層級問題報告的支援。
|   **服務名稱**    |   wercplsupport
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="program-compatibility-assistant-service"></a>程式相容性助理服務     

| | |           
|---|---|       
|   **服務描述** |   此服務可對程式相容性助理 (PCA) 提供支援。  PCA 會監視使用者所安裝和執行的程式，並偵測已知的相容性問題。 如果停止此服務，PCA 將無法正常運作。
|   **服務名稱**    |   PcaSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

##  <a name="quality-windows-audio-video-experience"></a>高品質 Windows 音訊/視訊體驗      

| | |           
|---|---|   
|   **服務描述** |   高品質 Windows 音訊/視訊體驗 (qWave) 是一種架構在 IP 家用網路上，適用於音訊/視訊 (AV) 串流處理應用程式的網路平台。 qWave 可確保 AV 應用程式的網路服務品質 (QoS)，進而增強 AV 串流處理的效能與可靠性。 它提供許可控制、執行階段監視與強制處理、應用程式意見反應，以及流量優先順序等機制。
|   **服務名稱**    |   QWAVE
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   用戶端 QoS 服務
|||         

<br />          

##      <a name="radio-management-service"></a>Radio Management Service        

| | |           
|---|---|   
|   **服務描述** |   無線電波管理與飛航模式服務
|   **服務名稱**    |   RmSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="remote-access-auto-connection-manager"></a>Remote Access Auto Connection Manager            

| | |           
|---|---|   
|   **服務描述** |   每當程式參照遠端 DNS 或 NetBIOS 名稱或位址時，建立遠端網路的連線。
|   **服務名稱**    |   RasAuto
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="remote-access-connection-manager"></a>遠端存取連線管理員         

| | |           
|---|---|   
|   **服務描述** |   管理從這部電腦到網際網路或其他遠端網路的撥號及虛擬私人網路 (VPN) 連線。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   RasMan
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="remote-desktop-configuration"></a>遠端桌面設定         

| | |           
|---|---|   
|   **服務描述** |   遠端桌面設定服務 (RDCS) 負責處理所有遠端桌面服務和遠端桌面相關的設定及工作階段維護活動 (需要系統內容)。 這些包括每一工作階段暫存資料夾、RD 佈景主題與 RD 憑證。
|   **服務名稱**    |   SessionEnv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   
|||         

<br />          

## <a name="remote-desktop-services"></a>遠端桌面服務          

| | |           
|---|---|   
|   **服務描述** |   允許使用者以互動方式連線到遠端電腦。 遠端桌面及遠端桌面工作階段主機伺服器均相依於此服務。  若要避免從遠端使用這部電腦，請取消選取 [系統屬性] 控制台項目之 [遠端] 索引標籤上的核取方塊。
|   **服務名稱**    |   TermService
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   
|||         

<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>遠端桌面服務使用者模式連接埠重新導向器        

| | |           
|---|---|   
|   **服務描述** |   允許將印表機/磁碟機/連接埠重新導向以進行 RDP 連線
|   **服務名稱**    |   UmRdpService
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   在連線的伺服器端上支援重新導向。
|||         

<br />          

## <a name="remote-procedure-call-rpc"></a>遠端程序呼叫 (RPC)          

| | |           
|---|---|   
|   **服務描述** |   RPCSS 服務是適用於 COM 與 DCOM 伺服器的服務控制管理員。 此服務會針對 COM 與 DCOM 伺服器執行物件啟用要求、物件輸出程式解析，以及分散式記憶體回收。 如果停止或停用此服務，使用 COM 或 DCOM 的程式將無法正常運作。 強烈建議您持續執行 RPCSS 服務。
|   **服務名稱**    |   RpcSs
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>遠端程序呼叫 (RPC) 尋找程式             

| | |               
|---|---|   
|   **服務描述** |   在 Windows 2003 及更早的 Windows 版本中，遠端程序呼叫 (RPC) 尋找程式服務會管理 RPC 名稱服務資料庫。 在 Windows Vista 及更新的 Windows 版本中，此服務不提供任何功能，只是為了應用程式相容性而保留。   |
|   **服務名稱**    |   RpcLocator  |
|   **安裝**    |   僅包含桌面體驗    |
|   **StartType**   |   Manual  |
|   **建議**  | 沒有指引   |
|   **註解**    |       |
|||             

<br />              

## <a name="remote-registry"></a>遠端登錄          

| | |           
|---|---|   
|   **服務描述** |   讓遠端使用者能夠修改這部電腦上的登錄設定。 如果停止此服務，則只有這部電腦上的使用者能夠修改登錄。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   RemoteRegistry
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   
|||         

<br />          

##  <a name="resultant-set-of-policy-provider"></a>原則結果組提供者            

| | |           
|---|---|       
|   **服務描述** |   提供網路服務來處理要求，以模擬在各種情況下，將群組原則設定套用至目標使用者或電腦，並計算原則結果組設定。
|   **服務名稱**    |   RSoPProv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |沒有指引    
|   **註解**    |   
|||         

<br />          

## <a name="routing-and-remote-access"></a>路由及遠端存取            

| | |           
|---|---|   
|   **服務描述** |   為區域及廣域網路環境中的公司提供路由服務。
|   **服務名稱**    |   RemoteAccess
|   **安裝**    |   必須安裝
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   已經停用
|||         

<br />          

## <a name="rpc-endpoint-mapper"></a>RPC 端點對應程式          

| | |           
|---|---|   
|   **服務描述** |   解析 RPC 介面識別碼以傳輸端點。 如果停止或停用此服務，則使用遠端程序呼叫 (RPC) 服務的程式將無法正常運作。
|   **服務名稱**    |   RpcEptMapper
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="secondary-logon"></a>次要登入     

| | |           
|---|---|       
|   **服務描述** |   能夠以備用認證來啟動處理序。 如果停止此服務，將無法使用此類型的登入存取。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   seclogon
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>安全通訊端通道通訊協定服務            

| | |               
|---|---|       
|   **服務描述** |   針對安全通訊端通道通訊協定 (SSTP) 提供使用 VPN 連線到遠端電腦的支援。 如果停用此服務，使用者將無法使用 SSTP 來存取遠端伺服器。    |
|   **服務名稱**    |   SstpSvc |
|   **安裝**    |   必須安裝    |
|   **StartType**   |   Manual  |
|   **建議**  |   請勿停用  |
|   **註解**    |   停用中斷 RRAS   |
|||             

<br />              

## <a name="security-accounts-manager"></a>安全性帳戶管理員            

| | |           
|---|---|       
|   **服務描述** |   啟動此服務即會告知其他服務，安全性帳戶管理員 (SAM) 已準備好接受要求。  停用此服務將無法通知系統中的其他服務 SAM 已經就緒，從而導致那些服務無法正確啟動。 此服務不應停用。
|   **服務名稱**    |   SamSs
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 請勿停用
|   **註解**    |   
|||         

<br />          

## <a name="sensor-data-service"></a>感應器資料服務  

| | |           
|---|---|   
|   **服務描述** |   傳遞來自各種感應器的資料
|   **服務名稱**    |   SensorDataService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />  

## <a name="sensor-monitoring-service"></a>感應器監視服務            

| | |           
|---|---|       
|   **服務描述** |   監視各種感應器，以公開資料並隨著系統與使用者狀態進行調整。  如果停止或停用此服務，顯示器亮度將無法隨著光線狀況進行調整。 停止此服務可能也會影響其他系統功能與特性。
|   **服務名稱**    |   SensrSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />

## <a name="sensor-service"></a>感應器服務

| | |
|---|---|
|   **服務描述** |   用來管理不同感應器功能的感應器服務。 管理感應器的簡易裝置方向 (SDO) 與記錄。 載入 SDO 感應器以報告裝置方向變更。  如果停止或停用此服務，將不會載入 SDO 感應器，因此將不會發生自動旋轉。 來自感應器的記錄收集也將停止。
|   **服務名稱**    |   SensorService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |
|||
  
<br />          

## <a name="server"></a>Server           

| | |           
|---|---|   
|   **服務描述** |   針對這部電腦支援網路上的檔案、列印和具名管道共用。 如果停止此服務，將無法使用這些功能。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   LanmanServer
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   對於遠端管理、IPC$、SMB 檔案共用的必要項
|||         

<br />          

## <a name="shell-hardware-detection"></a>殼層硬體偵測             

| | |           
|---|---|       
|   **服務描述** |   提供自動播放硬體事件的通知。
|   **服務名稱**    |   ShellHWDetection
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="smart-card"></a>智慧卡           

| | |           
|---|---|   
|   **服務描述** |   管理這部電腦讀取智慧卡的存取權。 如果停止此服務，這部電腦將無法讀取智慧卡。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   SCardSvr
|   **安裝**    |   必須安裝
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

## <a name="smart-card-device-enumeration-service"></a>智慧卡裝置列舉服務                    

| | |               
|---|---|       
|   **服務描述** |   為指定工作階段可存取的所有智慧卡讀卡機，建立軟體裝置節點。 如果停用此服務，WinRT API 將無法列舉智慧卡讀卡機。   |
|   **服務名稱**    |   ScDeviceEnum    |
|   **安裝**    |   必須安裝    |
|   **StartType**   |   Manual  |
|   **建議**  |   確定停用   |
|   **註解**    |   幾乎是專供 WinRT 應用程式使用的必要項    |
|||             

<br />              

## <a name="smart-card-removal-policy"></a>智慧卡移除原則        

| | |           
|---|---|       
|   **服務描述** |   允許將系統設定為在智慧卡移除時，鎖定使用者桌面。
|   **服務名稱**    |   SCPolicySvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="snmp-trap"></a>SNMP 陷阱            

| | |           
|---|---|       
|   **服務描述** |   接收由本機或遠端簡易網路管理通訊協定 (SNMP) 代理程式所產生的陷阱訊息，並將該訊息轉送給在這部電腦上執行的 SNMP 管理程式。 如果停止此服務，這部電腦上以 SNMP 為基礎的程式將不會收到 SNMP 陷阱訊息。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   SNMPTRAP
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="software-protection"></a>軟體保護             

| | |           
|---|---|       
|   **服務描述** |   能夠針對 Windows 及 Windows 應用程式，下載、安裝及強制執行數位授權。 如果停用此服務，作業系統及已授權的應用程式可能會以通知模式執行。 強烈建議您不要停用軟體保護服務。
|   **服務名稱**    |   sppsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="special-administration-console-helper"></a>特殊的管理主控台協助程式        

| | |           
|---|---|   
|   **服務描述** |   允許系統管理員使用緊急管理服務，遠端存取命令提示字元。
|   **服務名稱**    |   sacsvr
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="spot-verifier"></a>錯誤檢查器            

| | |           
|---|---|   
|   **服務描述** |   驗證可能發生的檔案系統損毀。
|   **服務名稱**    |   svsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="ssdp-discovery"></a>SSDP 探索           

| | |           
|---|---|   
|   **服務描述** |   探索使用 SSDP 探索通訊協定且已連上網路的裝置及服務，例如 UPnP 裝置。 此外，還會宣告在本機電腦上執行的 SSDP 裝置及服務。 如果停止此服務，將無法探索以 SSDP 為基礎的裝置。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   SSDPSRV
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="state-repository-service"></a>狀態存放庫服務         

| | |           
|---|---|   
|   **服務描述** |   為應用程式模型提供必要的基礎結構支援。
|   **服務名稱**    |   StateRepository
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="still-image-acquisition-events"></a>靜止影像擷取事件

| | |           
|---|---|   
|   **服務描述** |   啟動與靜態影像擷取事件相關聯的應用程式。
|   **服務名稱**    |   WiaRpc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />  

## <a name="storage-service"></a>儲存空間服務          

| | |           
|---|---|       
|   **服務描述** |   提供為儲存空間設定與外部儲存空間擴充啟用服務的功能
|   **服務名稱**    |   StorSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="storage-tiers-management"></a>儲存層管理        

| | |           
|---|---|   
|   **服務描述** |   在系統中的所有階層式儲存空間上，將儲存層中資料的放置方式最佳化。
|   **服務名稱**    |   TieringEngineService
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="superfetch"></a>Superfetch          

| | |           
|---|---|       
|   **服務描述** |   維護並改進一段時間後的系統效能。
|   **服務名稱**    |   SysMain
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="sync-host"></a>同步主機            

| | |           
|---|---|       
|   **服務描述** |   此服務會將郵件、連絡人、行事曆與各種其他使用者資料同步。 若此服務未執行，相依於此功能的郵件與其他應用程式都將無法正常運作。
|   **服務名稱**    |   OneSyncSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />          

## <a name="system-event-notification-service"></a>系統事件通知服務            

| | |           
|---|---|       
|   **服務描述** |   監視系統事件，並通知 COM+ 事件系統的訂閱者有關這些事件的內容。
|   **服務名稱**    |   SENS
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="system-events-broker"></a>系統事件代理人             

| | |           
|---|---|       
|   **服務描述** |   協調 WinRT 應用程式的背景工作執行。 如果停止或停用此服務，則可能不會觸發背景工作。
|   **服務名稱**    |   SystemEventsBroker
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   儘管其描述表示它僅適用於 WinRT 應用程式，但工作排程器、代理人基礎結構服務及其他內部元件仍然需要它。
|||         

<br />          

## <a name="task-scheduler"></a>工作排程器           

| | |           
|---|---|   
|   **服務描述** |   讓使用者能夠在這部電腦上設定和排程自動化工作。 此服務也會裝載多個 Windows 系統重要工作。 如果停止或停用此服務，這些工作將不會在其排定的時間執行。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   排程
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="tcpip-netbios-helper"></a>TCP/IP NetBIOS 協助程式            

| | |           
|---|---|   
|   **服務描述** |   提供對 NetBIOS over TCP/IP (NetBT) 服務及網路上用戶端之 NetBIOS 名稱解析的支援，因而讓使用者能夠共用檔案、列印及登入至網路。 如果停止此服務，可能就無法使用這些功能。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   lmhosts
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="telephony"></a>Telephony           

| | |           
|---|---|       
|   **服務描述** |   為程式提供電話語音 API (TAPI) 支援，以便在本機電腦上，以及透過 LAN 在同樣執行此服務的伺服器上，控制電話語音裝置。
|   **服務名稱**    |   TapiSrv
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   停用中斷 RRAS
|||         

<br />          

## <a name="themes"></a>佈景主題           

| | |           
|---|---|
|   **服務描述** |   提供使用者體驗佈景主題管理。
|   **服務名稱**    |   佈景主題
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   若停用此服務，便無法設定協助工具佈景主題
|||         

<br />  

## <a name="tile-data-model-server"></a>磚資料模型伺服器           

| | |           
|---|---|   
|   **服務描述** |   適用於磚更新的磚伺服器。
|   **服務名稱**    |   tiledatamodelsvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   如果停用此服務，則開始功能表會中斷
|||         

<br />          

##  <a name="time-broker"></a>計時代理人     

| | |           
|---|---|       
|   **服務描述** |   協調 WinRT 應用程式的背景工作執行。 如果停止或停用此服務，則可能不會觸發背景工作。
|   **服務名稱**    |   TimeBrokerSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   儘管其描述表示它僅適用於 WinRT 應用程式，但工作排程器、代理人基礎結構服務及其他內部元件仍然需要它。
|||         

<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>觸控式鍵盤與手寫面板服務         

| | |           
|---|---|   
|   **服務描述** |   啟用觸控式鍵盤和手寫面板及筆跡功能
|   **服務名稱**    |   TabletInputService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>更新 Windows Update 的 Orchestrator 服務           

| | |           
|---|---|       
|   **服務描述** |   管理 Windows 更新。 如果停止，您的裝置將無法下載並安裝最新的更新。
|   **服務名稱**    |   UsoSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   V1607 中遺漏了服務描述；Windows Update (包括 WSUS) 相依於此服務。
|||         

<br />          

## <a name="upnp-device-host"></a>UPnP 裝置主機         

| | |           
|---|---|   
|   **服務描述** |   允許在這部電腦上裝載 UPnP 裝置。 如果停止此服務，任何裝載的 UPnP 裝置都將停止運作且無法新增其他裝載的裝置。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   upnphost
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="user-access-logging-service"></a>使用者存取記錄服務          

| | |           
|---|---|   
|   **服務描述** |   此服務會記錄本機伺服器上所安裝產品與角色的唯一用戶端存取要求 (形式為 IP 位址和使用者名稱)。 當系統管理員需要將伺服器軟體的用戶端需求量化以進行離線用戶端存取使用權 (CAL) 管理時，可以透過 Powershell 查詢此資訊。 如果停用此服務，將不會記錄用戶端要求，也無法透過 Powershell 查詢加以擷取。 停止此服務將不會影響歷史資料的查詢 (如需刪除歷史資料的步驟，請參閱支援文件)。 本機系統管理員必須參閱其 Windows Server 授權條款，以決定對伺服器軟體進行適當授權所需的 CAL 數目；使用 UAL 服務和資料並不會變更此義務。
|   **服務名稱**    |   UALSVC
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="user-data-access"></a>使用者資料存取        

| | |           
|---|---|   
|   **服務描述** |   為應用程式提供對結構化使用者資料 (包括連絡人資訊、行事曆、訊息與其他內容) 的存取權。 如果您停止或停用此服務，使用此資料的應用程式可能無法正確運作。
|   **服務名稱**    |   UserDataSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />          

## <a name="user-data-storage"></a>使用者資料儲存            

| | |           
|---|---|       
|   **服務描述** |   處理結構化使用者資料 (包括連絡人資訊、行事曆、訊息與其他內容) 的儲存。 如果您停止或停用此服務，使用此資料的應用程式可能無法正確運作。
|   **服務名稱**    |   UnistoreSvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />          

## <a name="user-experience-virtualization-service"></a>使用者體驗虛擬化服務           

| | |           
|---|---|       
|   **服務描述** |   提供對應用程式和 OS 設定漫遊的支援
|   **服務名稱**    |   UevAgentService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

##  <a name="user-manager"></a>使用者管理員        

| | |           
|---|---|   
|   **服務描述** |   使用者管理員提供多使用者互動所需的執行階段元件。  如果停止此服務，某些應用程式可能無法正確運作。
|   **服務名稱**    |   UserManager
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="user-profile-service"></a>使用者設定檔服務         

| | |           
|---|---|   
|   **服務描述** |   此服務會負責載入和卸載使用者設定檔。 如果停止或停用此服務，使用者將無法順利登入或登出、應用程式可能會在取得使用者的資料時發生問題，而已註冊來接收設定檔事件通知的元件將無法接收通知。
|   **服務名稱**    |   ProfSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="virtual-disk"></a>虛擬磁碟             

| | |           
|---|---|   
|   **服務描述** |   提供磁碟、磁碟區、檔案系統與儲存空間陣列的管理服務。
|   **服務名稱**    |   vds
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   沒有指引
|   **註解**    |   
|||         

<br />          

## <a name="volume-shadow-copy"></a>磁碟區陰影複製           

| | |           
|---|---|   
|   **服務描述** |   管理及實作用於備份和其他用途的磁碟區陰影複製。 如果停止此服務，陰影複製將無法用於備份，且備份可能會失敗。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   VSS
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   沒有指引
|   **註解**    |   
|||         

<br />          

##  <a name="walletservice"></a>WalletService           

| | |           
|---|---|   
|   **服務描述** |   裝載電子錢包用戶端所使用的物件
|   **服務名稱**    |   WalletService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="windows-audio"></a>Windows 音訊            

| | |           
|---|---|       
|   **服務描述** |   管理 Windows 程式的音訊。  如果停止此服務，音訊裝置及效果都將無法正常運作。  如果停用此服務，明確依賴此服務的任何服務都將無法啟動
|   **服務名稱**    |   Audiosrv
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="windows-audio-endpoint-builder"></a>Windows 音訊端點產生器           

| | |           
|---|---|
|   **服務描述** |   管理 Windows 音訊服務的音訊裝置。  如果停止此服務，音訊裝置及效果都將無法正常運作。  如果停用此服務，明確依賴此服務的任何服務都將無法啟動
|   **服務名稱**    |   AudioEndpointBuilder
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="windows-biometric-service"></a>Windows 生物特徵辨識服務            

| | |           
|---|---|   
|   **服務描述** |   Windows 生物特徵辨識服務讓用戶端應用程式能夠擷取、比較、操縱及儲存生物特徵辨識資料，而不需直接存取任何生物特徵辨識硬體或樣本。 此服務裝載於具有特殊權限的 SVCHOST 處理序中。
|   **服務名稱**    |   WbioSrvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="windows-camera-frame-server"></a>Windows 相機框架伺服器         

| | |           
|---|---|       
|   **服務描述** |   讓多個用戶端能夠存取來自相機裝置的視訊框架。
|   **服務名稱**    |   FrameServer
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="windows-connection-manager"></a>Windows 連線管理員           

| | |           
|---|---|   
|   **服務描述** |   依據 PC 目前可用的網路連線選項來進行自動連線/中斷連線決策，並且能夠依據群組原則設定來管理網路連線。
|   **服務名稱**    |   Wcmsvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-defender-network-inspection-service"></a>Windows Defender 網路檢查服務          

| | |           
|---|---|       
|   **服務描述** |   協助抵禦以網路通訊協定中已知與新發現的弱點為目標而發動的入侵攻擊
|   **服務名稱**    |   WdNisSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引    
|   **註解**    |   
|||         

<br />          

## <a name="windows-defender-service"></a>Windows Defender 服務         

| | |           
|---|---|       
|   **服務描述** |   協助使用者防制惡意程式碼及其他潛在的垃圾軟體
|   **服務名稱**    |   WinDefend
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows 驅動程式基礎 - 使用者模式驅動程式架構           

| | |           
|---|---|   
|   **服務描述** |   建立並管理使用者模式驅動程式處理序。 此服務無法停止。
|   **服務名稱**    |   wudfsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-encryption-provider-host-service"></a>Windows 加密提供者主機服務     

| | |           
|---|---|   
|   **服務描述** |   Windows 加密提供者主機服務會將來自協力廠商加密提供者的加密相關功能，仲介給需要評估並套用 EAS 原則的處理序。 停止此服務，將會破壞已連線郵件帳戶所建立的 EAS 合規性檢查
|   **服務名稱**    |   WEPHOSTSVC
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-error-reporting-service"></a>Windows 錯誤報告服務          

| | |           
|---|---|       
|   **服務描述** |   允許在程式停止運作或回應時報告錯誤，並允許傳遞現有的解決方案。 此外，也允許產生用於診斷與修復服務的記錄。 如果停止此服務，錯誤報告可能就無法正常運作，而且可能無法顯示診斷服務及修復的結果。
|   **服務名稱**    |   WerSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   收集並傳送 MS 和協力廠商 ISV/IHV 所使用的當機/停止回應資料。 此資料可用來診斷當機引發的錯誤，其中可能包括安全性錯誤。 也需要用於企業錯誤報告
|||         

<br />          

## <a name="windows-event-collector"></a>Windows 事件收集器          

| | |           
|---|---|   
|   **服務描述** |   此服務會管理持續訂閱來自支援 WS-Management 通訊協定之遠端來源的事件。 這包括 Windows Vista 事件記錄檔、硬體，以及啟用 IPMI 的事件來源。 此服務會將轉送的事件儲存於本機事件記錄檔。 如果停止或停用此服務，將無法建立事件訂閱，也無法接受轉送的事件。
|   **服務名稱**    |   Wecsvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   基於管理與診斷目的來收集 ETW 事件 (包括安全性事件)。  許多功能和協力廠商工具均依賴此功能，包括安全性稽核工具
|||         

<br />          

## <a name="windows-event-log"></a>Windows 事件日誌            

| | |           
|---|---|       
|   **服務描述** |   此服務會管理事件與事件記錄檔。 它支援記錄事件、查詢事件、訂閱事件、封存事件記錄檔，以及管理事件中繼資料。 它可以使用 XML 與純文字格式來顯示事件。 停止此服務可能會危害系統的安全性與可靠性。
|   **服務名稱**    |   EventLog
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-firewall"></a>Windows 防火牆         

| | |           
|---|---|   
|   **服務描述** |   Windows 防火牆可遏止未經授權的使用者透過網際網路或網路存取您的電腦，從而達到保護您電腦的目的。
|   **服務名稱**    |   MpsSvc
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="windows-font-cache-service"></a>Windows 字型快取服務      

| | |           
|---|---|   
|   **服務描述** |   透過快取常用的字型資料，將應用程式效能最佳化。 如果此服務尚未執行，應用程式將會啟動它。 您也可以停用此服務，不過，這樣做將會降低應用程式效能。
|   **服務名稱**    |   FontCache
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-image-acquisition-wia"></a>Windows Image Acquisition (WIA)          

| | |           
|---|---|   
|   **服務描述** |   為掃描器與數位相機提供影像擷取服務
|   **服務名稱**    |   stisvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

##  <a name="windows-insider-service"></a>Windows 測試人員服務     

| | |           
|---|---|   
|   **服務描述** |   wisvc
|   **服務名稱**    |   wisvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   伺服器不支援正式發行前小眾測試，因此它不會在伺服器上執行任何操作。 功能也可透過 GP 停用。
|||         

<br />          

##  <a name="windows-installer"></a>Windows Installer       

| | |           
|---|---|
|   **服務描述** |   新增、修改及移除以 Windows Installer (\*.msi、\*.msp) 套件形式提供的應用程式。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   msiserver
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-license-manager-service"></a>Windows 授權管理員服務          

| | |           
|---|---|   
|   **服務描述** |   提供 Microsoft Store 的基礎結構支援。  此服務會依需求啟動，而且如果停用，則透過 Microsoft Store 取得的內容將無法正常運作。
|   **服務名稱**    |   LicenseManager
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-management-instrumentation"></a>Windows Management Instrumentation       

| | |           
|---|---|       
|   **服務描述** |   提供通用介面和物件模型，以存取有關作業系統、裝置、應用程式及服務的管理資訊。 如果停止此服務，則大多數的 Windows 軟體都將無法正常運作。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   Winmgmt
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

##  <a name="windows-mobile-hotspot-service"></a>Windows 行動熱點服務          

| | |           
|---|---|       
|   **服務描述** |   提供與其他裝置共用行動數據連線的能力。
|   **服務名稱**    |   icssvc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   
|||         

<br />          

## <a name="windows-modules-installer"></a>Windows 模組安裝程式        

| | |           
|---|---|   
|   **服務描述** |   能夠安裝、修改及移除 Windows 更新和選用元件。 如果停用此服務，則針對這部電腦安裝或解除安裝 Windows 更新可能會失敗。
|   **服務名稱**    |   TrustedInstaller
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="windows-push-notifications-system-service"></a>Windows 推播通知系統服務            

| | |           
|---|---|
|   **服務描述** |   此服務會在工作階段 0 執行並裝載通知平台與連線提供者，其會處理裝置與 WNS 伺服器之間的連線。
|   **服務名稱**    |   WpnService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   自動
|   **建議**  |   確定停用
|   **註解**    |   對於動態磚和其他功能的必要項
|||         

<br />      

## <a name="windows-push-notifications-user-service"></a>Windows 推播通知使用者服務          

| | |           
|---|---|   
|   **服務描述** |   此服務會裝載 Windows 通知平台，此平台提供對本機與推播通知的支援。 支援的通知為磚、快顯通知與原始通知。
|   **服務名稱**    |   WpnUserService
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   確定停用
|   **註解**    |   使用者服務範本
|||         

<br />

## <a name="windows-remote-management-ws-management"></a>Windows 遠端管理 (WS-Management)
| | |
|---|---|
|   **服務描述** |   Windows 遠端管理 (WinRM) 服務實作了 WS-Management 通訊協定來執行遠端管理。 WS-Management 是可用於管理遠端軟體及硬體的標準 Web 服務通訊協定。 WinRM 服務會接聽並處理網路上的 WS-Management 要求。 您必須使用 winrm.cmd 命令列工具或透過群組原則，為 WinRM 服務設定接聽程式，如此 WinRM 服務才能執行網路接聽工作。 WinRM 服務提供 WMI 資料的存取權及事件收集的功能。 這項服務必須在執行中，才能執行事件收集及訂閱事件。 WinRM 訊息使用 HTTP 及 HTTPS 進行傳輸。 WinRM 服務與 IIS 並無依存關係，但會預先設定成與 IIS 並用同一部機器上的同一個連接埠。  WinRM 服務會保留 /wsman URL 首碼。 為避免與 IIS 發生衝突，系統管理員應確定 IIS 上所代管的網站未使用 /wsman URL 首碼。
|   **服務名稱**    |   WinRM
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  |   請勿停用
|   **註解**    |   遠端管理的必要項
|||

<br />          

##  <a name="windows-search"></a>Windows 搜尋      

| | |           
|---|---|       
|   **服務描述** |   提供檔案、電子郵件和其他內容的內容索引、屬性快取及搜尋結果。
|   **服務名稱**    |   WSearch
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   已停用
|   **建議**  |   已經停用
|   **註解**    |   
|||         

<br />          

##  <a name="windows-time"></a>Windows Time        

| | |           
|---|---|   
|   **服務描述** |   讓網路中所有用戶端與伺服器的日期和時間維持同步。 如果此服務停止，將無法使用日期和時間同步處理。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   W32Time
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引
|   **註解**    |   
|||         

<br />          

## <a name="windows-update"></a>Windows Update           

| | |           
|---|---|       
|   **服務描述** |   能夠偵測、下載及安裝 Windows 和其他程式的更新。 如果停用此服務，這部電腦的使用者將無法使用 Windows Update 或其自動更新功能，而且程式將無法使用 Windows Update 代理程式 (WUA) API。
|   **服務名稱**    |   wuauserv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>WinHTTP Web Proxy 自動探索服務         

| | |           
|---|---|   
|   **服務描述** |   WinHTTP 會實作用戶端 HTTP 堆疊，並為開發人員提供 Win32 API 及 COM 自動化元件來傳送 HTTP 要求及接收回應。 此外，WinHTTP 會透過實作 Web Proxy 自動探索 (WPAD) 通訊協定，來提供自動探索 Proxy 設定的支援。
|   **服務名稱**    |   WinHttpAutoProxySvc
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  |   請勿停用
|   **註解**    |   任何使用網路堆疊的項目都會對此服務具有功能相依性。 許多組織都依賴此服務來設定其內部網路的 HTTP Proxy 路由。  如果沒有此服務，則源自內部且連至網際網路的 HTTP 連線全部都將失敗。
|||         

<br />          

## <a name="wired-autoconfig"></a>有線自動設定服務         

| | |           
|---|---|       
|   **服務描述** |   有線自動設定 (DOT3SVC) 服務會負責在乙太網路介面上執行 IEEE 802.1X 驗證。 如果目前的有線網路部署會強制執行 802.1X 驗證，則應該設定執行 DOT3SVC 服務，以建立第 2 層連線及/或提供網路資源的存取。 未強制執行 802.1X 驗證的有線網路則不受 DOT3SVC 服務所影響。
|   **服務名稱**    |   dot3svc
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  | 沒有指引   
|   **註解**    |   
|||         

<br />          

## <a name="wmi-performance-adapter"></a>WMI 效能配接器          

| | |           
|---|---|   
|   **服務描述** |   將來自 Windows Management Instrumentation (WMI) 提供者的效能程式庫資訊提供給網路上的用戶端。 此服務只有在啟用效能資料協助程式時才會執行。
|   **服務名稱**    |   wmiApSrv
|   **安裝**    |   必須安裝
|   **StartType**   |   Manual
|   **建議**  | 沒有指引       
|   **註解**    |   
|||         

<br />          

## <a name="workstation"></a>工作站          

| | |           
|---|---|   
|   **服務描述** |   使用 SMB 通訊協定來建立並維護連到遠端伺服器的用戶端網路連線。 如果停用此服務，這些連線都將無法使用。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。
|   **服務名稱**    |   LanmanWorkstation
|   **安裝**    |   必須安裝
|   **StartType**   |   自動
|   **建議**  | 沒有指引       
|   **註解**    |   
|||         

<br />

## <a name="xbox-live-auth-manager"></a>Xbox Live Auth Manager           

| | |           
|---|---|   
|   **服務描述** |   提供驗證和授權服務以便與 Xbox Live 互動。 如果停止此服務，某些應用程式可能無法正確運作。
|   **服務名稱**    |   XblAuthManager
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   應該停用
|   **註解**    |   
|||         

<br />          

## <a name="xbox-live-game-save"></a>Xbox Live Game Save          

| | |           
|---|---|   
|   **服務描述** |   此服務會針對 Xbox Live 中已啟用儲存功能的遊戲同步處理儲存資料。  如果停止此服務，遊戲儲存資料將不會上傳至 Xbox Live 或從中下載。
|   **服務名稱**    |   XblGameSave
|   **安裝**    |   僅包含桌面體驗
|   **StartType**   |   Manual
|   **建議**  |   應該停用
|   **註解**    |   此服務會針對 Xbox Live 中已啟用儲存功能的遊戲同步處理儲存資料。  如果停止此服務，遊戲儲存資料將不會上傳至 Xbox Live 或從中下載。
|||         

<br /> 
<br /> 

