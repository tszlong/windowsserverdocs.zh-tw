---
title: 快速更新傳遞 ISV 支援
description: Windows Server Update Service （WSUS）主題-獨立軟體廠商（ISV）如何使用 WSUS 來設定快速更新傳遞
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
ms.openlocfilehash: 0f5893d47219e9263ed7f35bee472848a47c6164
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868743"
---
# <a name="express-update-delivery-isv-support"></a>快速更新傳遞 ISV 支援

>適用於：Windows 10、Windows Server 2016

Windows 10 更新下載可能會很大，因為每個套件都包含所有先前發行的修正程式，以確保一致性和簡易性。  

自版本7起，Windows 已能夠使用稱為[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)的功能減少 Windows Update 下載的大小，而雖然消費者裝置預設支援，但 windows 10 企業版裝置需要 WINDOWS SERVER UPDATE SERVICES （WSUS）才能執行Express 的優勢。

## <a name="how-microsoft-supports-express"></a>Microsoft 支援「快速」的方式

- **在 WSUS 獨立的 Express**

    [所有支援的 WSUS 版本](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx)皆已提供快速更新傳遞。

- **直接連線到 Windows Update 的裝置上的 Express** 

    取用者裝置支援快速下載：他們會使用 Windows Update （WU）用戶端來掃描、下載及安裝更新。 在下載階段，WU 用戶端會要求 Express 套件，並下載適當的位元組範圍。

-  **使用[商務用 Windows Update](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** 管理的企業裝置不需要任何變更，也能受益於快速更新傳遞支援。

## <a name="how-isvs-can-take-advantage-of-express"></a>Isv 可以如何利用 Express

Isv 可以使用 WSUS 和 WU 用戶端來支援快速更新傳遞。 Microsoft 建議執行下列三個步驟，並在下列各節中更詳細地討論。

1.  [**設定 WSUS**](#BKMK_1)

    需要 WSUS 伺服器，才能進行掃描 & 更新同步處理（如需其他資訊，請參閱[這裡](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx)）

2.  [**指定並填入 ISV 檔案快取**](#BKMK_2)

    建議使用 ISV 檔案快取來裝載更新內容，其中包括更新封包檔（.cab 檔）和快速套件（. .psf 檔）。

3.  [**設定 ISV 用戶端代理程式以指示 WU 用戶端作業**](#BKMK_3)

>[!NOTE]
>需要安裝 Windows 10 1607 版（或更新版本）2017（[KB3213986 （OS 組建14393.693））](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693)的累積更新。
    
   - ISV 用戶端代理程式會決定要核准的更新，以及下載和安裝更新的時間。
   - WU 用戶端會判斷要下載的位元組範圍，並起始下載要求

### <a name="BKMK_1"></a>步驟1：設定 WSUS

WSUS 作為介面來 Windows Update 和管理所有描述需要下載之 Express 套件的中繼資料。 如果您需要部署，請參閱[**Windows Server Update Services 3.0 SP2 的總覽**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx)。 部署 WSUS 之後，主要考慮是要將更新內容儲存在 WSUS 伺服器本機上。 設定 WSUS 時，建議您不要在本機儲存更新。 這會假設您已有軟體將這些套件部署到您的環境中。 如需有關如何設定 WSUS 本機存放裝置的詳細資訊，請參閱[**決定儲存更新的位置**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx)。

### <a name="BKMK_2"></a>步驟2：指定並填入 ISV 檔案快取 

#### <a name="specify-the-isv-file-cache"></a>指定 ISV 檔案快取

[設定[**服務提供者參考**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference)] 中詳述的新用戶端群組原則和行動裝置管理（MDM）設定會定義 ISV 檔案快取的位置。

| **名稱**                                              | **描述**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 設定更新的替代下載位置。 | 指定要從 Microsoft Update 裝載更新的替代內部網路伺服器。 接著，您可以使用此更新服務來自動更新網路上的電腦 |

設定 ISV 檔案快取的替代下載位置時，有兩個選項：

1. **指定 ISV HTTP 伺服器主機名稱**，也就是 isv 檔案快取
    
    此方法會設定 WU 用戶端，以對原則中指定的 HTTP 伺服器提出下載要求

2. **指定 localhost**
 
    這個方法會設定 WU 用戶端，將下載要求傳送至 localhost。 如此一來，ISV 用戶端代理程式就可以適當地處理這些要求和路由，以滿足下載要求。

> [!IMPORTANT]
> ISV 檔案快取需要下列各項：                                                          
> - 根據 RFC，伺服器必須符合 HTTP 1.1：<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> 具體而言，網頁伺服器需要支援                                                                                                                                                                                                                                      [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)和[**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)要求<br>                                                                                                                                                                                                                                                                                                  -部分範圍要求<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -請勿使用「傳輸-編碼：區塊」                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>填入 ISV 檔案快取

ISV 檔案快取必須填入與要安裝在受管理用戶端上的更新相關聯的檔案。 

**若要填入 ISV 檔案快取：**

1. 使用[WSUS api](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx)來存取 MU 服務的更新檔案路徑和檔案名。

    WSUS 伺服器上每個更新的中繼資料會在 Microsoft Update 上包含更新的檔案路徑和檔案名，如下所示（以粗體 Microsoft Update 主機名稱，後面接著檔案路徑 **<http://download.windowsupdate.com>** 和檔案名）：/c/msdownload/update/software/updt/2016/09/windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74

2. 從 Microsoft Update 下載檔案，並使用下列兩種方法的其中一種將檔案儲存在 ISV 檔案快取中： 

   - 使用與**MU 服務相同的資料夾路徑來**儲存檔案

   - 使用**ISV 定義的資料夾路徑**儲存檔案

     讓 HTTP 伺服器（或 localhost）重新導向 ISV 檔案位置的**HTTP GET**要求，其參照 MU 資料夾路徑和檔案名。

### <a name="BKMK_3"></a>步驟3：設定 ISV 用戶端代理程式以指示 WU 用戶端作業

ISV 用戶端代理程式會使用下列建議的工作流程，協調下載和安裝已核准的更新：

1.  ISV 用戶端代理程式會呼叫 WU 用戶端，以針對 WSUS 伺服器進行掃描

2.  掃描會將一組適用的更新傳回至 WU 用戶端

3.  ISV 用戶端會決定要核准、下載及安裝的更新

4.  ISV 用戶端代理程式會呼叫 WU 用戶端以下載核准的更新

5.  下載更新之後，ISV 用戶端代理程式會呼叫 WU 用戶端，以安裝已核准的更新。

如需使用 WU 用戶端掃描、下載及安裝更新的詳細資訊，請參閱[搜尋、下載及安裝更新](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx)。

### <a name="download-workflow-options"></a>下載工作流程選項

以下是從 ISV 檔案快取下載工作流程選項的兩個圖例：

![工作流程1](../../media/express-update-delivery-isv-support/image1.png)

![工作流程2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>快速下載的運作方式

- 對於支援「快速」的作業系統更新，服務上有儲存兩種版本的檔案裝載︰

  - **完整檔案版本**--基本上會取代更新二進位檔的本機版本

  - **Express 版本**--包含在裝置上修補現有二進位檔所需的差異。 

    完整檔案版本和 Express 版本都是在更新的中繼資料中參考，這在掃描階段已下載至用戶端。 

    **快速下載的運作方式如下：**

    WU 用戶端會先嘗試下載 Express，而在某些情況下，視需要切換回完整檔案（例如，如果流覽不支援位元組範圍要求的 proxy）。

  1. 當 WU 用戶端起始快速下載時， **wu 用戶端會先下載 stub**，這是 express 套件的一部分。

  2. **WU 用戶端會將此存根傳遞給 Windows installer，此安裝程式**會使用存根來執行本機清查，並比較裝置上的檔案差異與所提供的最新檔案版本所需的差異。

  3. **Windows installer 接著會要求 WU 用戶端下載**已判斷為必要的範圍。

  4. **WU 用戶端會下載這些範圍，並將它們傳遞至 Windows installer**，這會套用範圍，然後決定是否需要額外的範圍。 這會重複執行，直到 Windows installer 告訴 WU 用戶端已下載所有必要的範圍為止。

  到目前為止，下載已完成，並且可準備安裝更新。

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>傳遞優化如何減少頻寬耗用量

「傳遞優化」（DO）是一種自我組織分散式快取解決方案，適用于想要降低作業系統更新、作業系統升級及應用程式之頻寬耗用量的企業。 允許用戶端從替代來源（例如網路上的其他對等）與指定的下載位置（在此案例中為 ISV 檔案快取）下載這些元素。

根據預設，在 Windows 10 企業版和教育版中，只允許在組織自己的網路上進行點對點共用，但是您可以使用群組原則和行動裝置管理（MDM）設定，以不同的方式進行設定。

如需詳細資訊，請參閱[設定 Windows 10 更新的傳遞優化](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization)。
