---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: "設定效能監視"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>設定效能監視

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS 包含自己專用的正在可協助您監視聯盟伺服器和聯盟伺服器 proxy 電腦的效能。 若要使用效能監視器監視您 AD FS 伺服器的效能，很有用建立新的資料收集設定，並將 AD FS 計數器新增到該檢視。 下列程序告訴您如何設定效能監視 AD fs。  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>若要設定效能 AD FS 使用「效能監視器的監視  
  
1.  在**[開始]**畫面中，輸入**效能監視器**，然後按 ENTER 鍵。  
  
2.  在主控台中，展開**收集的資料集**，right\ 按**使用者定義**，指向 [**新**，，然後按一下**資料收集設定**。  
  
    [建立新的資料收集設定精靈會出現。  
  
3.  在**建立新的資料收集設定**的**名稱**輸入新的資料收集設定名稱 \ (例如「AD FS 效能」\)，按一下 [**手動建立 \(Advanced\)**，，然後按一下 [**下一步**。  
  
4.  類型的資料包括，確認**建立資料登**已選取，然後按一下 [資料類型下列核取方塊：**計數器**、**事件追蹤資料**、**系統設定的資訊**。  
  
5.  效能計數器，展開**AD FS**在**可用的計數器**清單，然後再按**新增**。  
  
    AD FS 效能計數器應該會出現在**新增計數器**清單中。  
  
6.  當系統提示您將事件追蹤提供者時，按一下**新增**，請選取**AD FS 事件**並**AD FS 追蹤**的提供者的清單。  
  
7.  當系統提示您新增登錄監視，請按一下**下一步**。  
  
8.  當系統提示您指定要儲存的效能資料的位置時，您可以接受預設的位置 \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*，然後按**下一步**。  
  
9. 當系統提示您建立的資料收集設定時，選取 [**儲存，並關閉**，然後按一下 [**完成**。  
  
    新的資料收集設定會出現在主控台在**使用者定義的**節點。  
  
10. 使用 AD FS 效能計數器使用下列步驟：  
  
    -   若要開始使用廣告 FS\ 相關計數器監視效能，設定您新增 right\ 按一下資料收集 \ (例如「AD FS 效能」\)，，然後按一下 [ **[開始]**。  
  
    -   若要建立的報告，以檢視效能監視結果，right\ 按一下 [資料收集設定您新增 \ (例如「AD FS 效能」\)，然後按一下 [**最新的報告**。  
  
    -   若要結束擷取的效能資料，讓您可以檢視最新的報告，right\ 按一下 [資料收集設定您新增 \ (例如「AD FS 效能」\)，然後按一下**停止**。  
  
    新增最新的報告，會自動編號 \（000001\ 開始）在**Report\\User 定義***\\ < data\_collector\_set >*節點樹上的主機。  
  
## <a name="ad-fs-performance-counters"></a>AD FS 效能計數器  
下表列出計數器 AD FS 效能，並告訴您如何很有幫助的監視與聯盟伺服器或聯盟伺服器 proxy 相關的活動。  
  
|計數器|描述|用於： 
|-----------|---------------|------------------- 
|權杖要求|監視權杖要求傳送給包括 SSOAuth 權杖要求聯盟伺服器數目。|聯盟伺服器 
|權杖 Requests\ 秒|監視權杖要求傳送給包括秒 SSOAuth 權杖要求聯盟伺服器數目。|聯盟伺服器  
|聯盟中繼資料要求|監視輸入聯盟中繼資料要求傳送給聯盟伺服器數目。|聯盟伺服器  
|聯盟中繼資料 Requests\ 秒|監視傳入聯盟中繼資料秒要求傳送給聯盟伺服器的數目。|聯盟伺服器  
|成品解析度要求|監視傳入聯盟中繼資料秒要求傳送給聯盟伺服器的數目。|聯盟伺服器  
|成品解析度 Requests\ 秒|監視成品解析度端點秒聯盟伺服器傳送請求數目。|聯盟伺服器  
|Proxy 要求|監視輸入要求傳送給聯盟 proxy 伺服器的號碼。|聯盟的 Proxy 伺服器  
|Proxy Requests\ 秒|監視傳入秒要求傳送給聯盟 proxy 伺服器的數目。|聯盟的 Proxy 伺服器  
|Proxy MEX 要求|監視傳入 WS\-中繼資料交換 \(MEX\) 要求傳送給聯盟 proxy 伺服器的數目。|聯盟的 Proxy 伺服器 
|Proxy MEX Requests\ 秒|監視傳入 MEX 秒要求傳送給聯盟 proxy 伺服器的數目。|聯盟的 Proxy 伺服器  
  

