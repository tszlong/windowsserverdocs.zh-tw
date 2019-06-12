---
title: 快速更新傳遞 ISV 支援
description: Windows Server Update Service (WSUS) 主題-如何獨立軟體廠商 (ISV) 可以設定 Express 使用 WSUS 的更新傳遞
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 7331418c1926958da07c94bca9ff9f871134f3fa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439877"
---
# <a name="express-update-delivery-isv-support"></a>快速更新傳遞 ISV 支援

>適用於：Windows 10、windows Server 2016

Windows 10 更新的下載項目可能很大，因為每個套件包含所有先前發行的修正，以確保一致性和簡易性。  

第 7 版，因為 Windows 已經能夠減少大小的 Windows Update 下載使用稱為[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)，而且雖然消費型裝置都支援它依預設，Windows 10 企業版裝置需要 Windows Server Update若要善用 Express services (WSUS)。

## <a name="how-microsoft-supports-express"></a>Microsoft 支援「快速」的方式

- **WSUS 獨立上的 sql express**

    快速更新傳遞已[用於所有支援的 WSUS 版本](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx)。

- **直接連線到 Windows Update 的裝置上的 sql express** 

    消費性裝置支援 Express 下載： 他們使用 Windows Update (WU) 用戶端掃描、 下載及安裝更新。 下載階段 WU 用戶端會要求 Express 封裝，並下載適當的位元組範圍。

-  **使用[商務用 Windows Update](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** 管理的企業裝置不需要任何變更，也能受益於快速更新傳遞支援。

## <a name="how-isvs-can-take-advantage-of-express"></a>如何 Isv 可以利用 Express

Isv 可以使用 WSUS 和 WU 用戶端來支援快速更新傳遞。 Microsoft 建議下列三個步驟，請在下列各節更詳細地討論：

1.  [**將 WSUS 設定**](#BKMK_1)

    WSUS 伺服器是必要的掃描和更新同步處理 (其他資訊，請參閱[此處](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**指定並填入 ISV 檔案快取**](#BKMK_2)

    ISV 檔案快取，建議的更新內容，其中包含更新封包檔 （.cab 檔案），並快速封裝 （.psf 檔案）。

3.  [**將設定 ISV 用戶端代理程式以導向 WU 用戶端操作**](#BKMK_3)

>[!NOTE]
>需要累計更新的 Windows 10 版本 1607年版 （或之後） 中年 1 月 2017年 ([KB3213986 (OS 組建 14393.693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693)安裝。
    
   - ISV 用戶端代理程式可讓您判斷哪些更新核准，以及當進行下載並安裝更新
   - WU 用戶端會判斷要下載的位元組範圍，並起始下載要求

### <a name="BKMK_1"></a>步驟 1:將 WSUS 設定

WSUS 會做為 Windows update 的介面，並管理說明需要下載的快速套件的所有中繼資料。 如果您需要部署，請參閱[**概觀的 Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx)。 一旦部署 WSUS，主要考量在於要將 WSUS 伺服器上本機儲存更新內容。 在設定 WSUS 時，我們建議不在本機儲存更新。 這是假設您已將這些封裝的部署導向您的環境中的軟體。 如需有關如何設定 WSUS 的本機儲存體的詳細資訊，請參閱[**決定在何處存放更新**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx)。

### <a name="BKMK_2"></a>步驟 2:指定並填入 ISV 檔案快取 

#### <a name="specify-the-isv-file-cache"></a>指定 ISV 檔案快取

新用戶端群組原則和行動裝置管理 (MDM) 設定中詳述[**組態服務提供者參考**](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference)定義 ISV 檔案快取的位置。

| **名稱**                                              | **描述**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 設定更新的其他下載位置。 | 指定替代的內部網路伺服器主機的更新，從 Microsoft Update。 您接著可以使用此更新服務來自動更新您的網路上的 電腦 |

ISV 檔案快取的其他下載位置在設定時，有兩個選項：

1. **指定的 ISV HTTP 伺服器主機名稱**，這是 ISV 檔案快取
    
    這個方法會設定要在原則中所指定的 HTTP 伺服器的下載要求 WU 用戶端

2. **指定 localhost**
 
    這個方法會設定 WU 用戶端進行下載到 localhost 的要求。 這可讓 ISV 用戶端代理程式，來處理這些要求和視需要完成的下載要求的路由。

> [!IMPORTANT]
> ISV 檔案快取需要下列項目：                                                          
> - 伺服器必須是根據 RFC 相容的 HTTP 1.1: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> 具體來說，web 伺服器需要支援                                                                                                                                                                                                                                      [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)並[**取得**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)要求<br>                                                                                                                                                                                                                                                                                                  部分範圍要求<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -保持連線<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -請勿使用"-Transfer-encoding： 區塊 」                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>填入 ISV 檔案快取

ISV 檔案快取必須填入受管理用戶端上安裝的更新相關聯的檔案。 

**若要填入 ISV 檔案快取：**

1. 使用[WSUS Api](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx)存取更新的檔案路徑和檔案名稱 MU 服務。

    WSUS 伺服器上的每個更新的中繼資料可包含更新的檔案路徑和檔案名稱，在 Microsoft Update，如下所示 (Microsoft Update 中的主機名稱粗體，後面接著檔案路徑和檔案名稱): **<http://download.windowsupdate.com>** c/msdownload/更新 /software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. 從 Microsoft Update 下載檔案，並將它們儲存在 ISV 檔案快取中使用這兩種方法的其中一個： 

   - 使用的儲存區檔案**與相同的資料夾路徑，MU 服務**

   - 使用的儲存區檔案**ISV 定義的資料夾路徑**

     有 HTTP 伺服器 （或 localhost） 重新導向**HTTP GET**參考 MU 資料夾路徑和檔案名稱，到 ISV 檔案位置的要求。

### <a name="BKMK_3"></a>步驟 3:將設定 ISV 用戶端代理程式以導向 WU 用戶端操作

下載並使用下列建議的工作流程的已核准更新的安裝，用來協調 ISV 用戶端代理程式：

1.  ISV 用戶端代理程式會呼叫 WU 用戶端至 WSUS 伺服器會掃描

2.  掃描傳回 WU 用戶端適用的更新集

3.  ISV 用戶端會判斷要核准、 下載及安裝的更新

4.  ISV 用戶端代理程式呼叫 WU 用戶端下載核准的更新

5.  ISV 用戶端代理程式在下載更新後, 呼叫 WU 用戶端安裝已核准的更新

請參閱[搜尋、 下載，並安裝更新](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387102(v=vs.85).aspx)其他有關使用 WU 用戶端來掃描的詳細資訊，請下載並安裝更新。

### <a name="download-workflow-options"></a>下載工作流程選項

以下是兩個圖的下載工作流程選項，從 ISV 檔案快取：

![工作流程 1](../../media/express-update-delivery-isv-support/image1.png)

![工作流程 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>快速下載的運作方式

- 對於支援「快速」的作業系統更新，服務上有儲存兩種版本的檔案裝載︰

  - **完整版**-基本上取代的更新二進位檔案的本機版本

  - **Express 版本**-包含差異需要修補現有的二進位檔，在裝置上。 

    完整版和 Express 版本中已掃描階段的一部分下載到用戶端更新的中繼資料參考。 

    **快速下載的運作方式，如下所示：**

    WU 用戶端會嘗試下載 Express 第一次，並在某些情況下 fall 回到完整檔案如有需要 （例如，如果要透過不支援位元組範圍要求的 proxy）。

  1. 當 WU 用戶端起始 Express 下載**WU 用戶端會先下載虛設常式**，這是 Express 封裝的一部分。

  2. **WU 用戶端會將此虛設常式傳遞給 Windows 安裝程式**，會使用虛設常式來執行本機的清查，並比較在裝置上使用需要取得最新版本所提供之檔案的內容檔案的差異。

  3. **Windows 安裝程式接著就會要求 WU 用戶端下載範圍**判斷其為必要。

  4. **WU 用戶端下載這些範圍，並將它們傳遞給 Windows 安裝程式**，這適用於範圍，然後決定 是否需要其他範圍。 這會重複直到 Windows 安裝程式會告訴 WU 用戶端已下載所有必要的範圍。

  到目前為止，下載已完成，並且可準備安裝更新。

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>傳遞最佳化如何降低頻寬耗用量

傳遞最佳化 (DO) 為自治組織的分散式快取解決方案，適用於想要降低頻寬耗用，作業系統更新、 作業系統升級和應用程式的企業。 可讓用戶端下載這些項目，從替代來源 （例如在網路上其他對等） 搭配指定的下載位置 （ISV 檔案快取在此案例中）。

依預設，在 Windows 10 企業版和教育，請勿允許對等共用，在組織自己的網路上，但您可以設定它以不同的方式使用 群組原則和行動裝置管理 (MDM) 設定。

請參閱[設定傳遞最佳化適用於 Windows 10 更新](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization)如需有關執行。
