---
title: 快速更新傳遞 ISV 支援
description: Windows Server Update Service (WSUS) 主題 - 獨立軟體廠商 (ISV) 如何使用 WSUS 來設定快速更新傳遞
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 13568bb320a3d70bfd6a70d2b9731b460be6f346
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948495"
---
# <a name="express-update-delivery-isv-support"></a>快速更新傳遞 ISV 支援

>適用於：Windows 10、Windows Server 2016

Windows 10 更新下載大小可能會很大，因為每個套件都包含所有先前發行的修正，以確保一致性與簡易性。  

從第 7 版開始，Windows 已可使用名為[快速 (Express)](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2) 的功能來減少 Windows Update 下載的大小，而雖然取用者裝置依預設可加以支援，但 Windows 10 企業版裝置仍需要 Windows Server Update Services (WSUS) 才能使用「快速 (Express)」。

## <a name="how-microsoft-supports-express"></a>Microsoft 支援「快速」的方式

- **單獨 WSUS 上的快速功能**

    快速更新傳遞已[適用於所有支援的 WSUS 版本](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx)。

- **直接連線至 Windows Update 的裝置上的快速功能** 

    取用者裝置支援快速下載：他們可使用 Windows Update (WU) 用戶端來掃描、下載及安裝更新。 在下載階段，WU 用戶端會要求「快速」套件，並下載適當的位元組範圍。

-  **使用[商務用 Windows Update](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** 進行管理的企業裝置不需要變更任何設定，也能受益於快速更新傳遞支援。

## <a name="how-isvs-can-take-advantage-of-express"></a>ISV 可如何使用快速功能

ISV 可以使用 WSUS 和 WU 用戶端來支援快速更新傳遞。 Microsoft 建議執行下列三個步驟，以下各節將會詳加討論：

1.  [**設定 WSUS**](#BKMK_1)

    需要使用 WSUS 伺服器來掃描和更新同步處理 (可在[這裡](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx)找到其他資訊)

2.  [**指定並填入 ISV 檔案快取**](#BKMK_2)

    建議使用 ISV 檔案快取來裝載更新內容，其中包括更新封包檔案 (.cab 檔案) 和「快速」套件 (.psf 檔案)。

3.  [**設定 ISV 用戶端代理程式以指示 WU 用戶端作業**](#BKMK_3)

>[!NOTE]
>需要安裝 2017 年 1 月 (或之後) 的 Windows 10 版本 1607 累計更新 ([KB3213986 (作業系統組建 14393.693)](https://support.microsoft.com/help/4009938/january-10-2017-kb3213986-os-build-14393-693)。
    
   - ISV 用戶端代理程式會決定要核准的更新，以及下載和安裝更新的時間
   - WU 用戶端會決定要下載的位元組範圍，並起始下載要求

### <a name="BKMK_1"></a>步驟 1：設定 WSUS

WSUS 會作為 Windows Update 的介面，並管理所有對需要下載的「快速」套件進行說明的中繼資料。 如果您需要部署，請參閱 [**Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx) 的概觀。 部署 WSUS 後，主要考量將是是否要將更新內容儲存在 WSUS 伺服器的本機上。 在設定 WSUS 時，建議您不要將更新儲存於本機。 若您這麼做，系統會假設您已讓軟體將這些套件的部署導入環境中。 如需如何設定 WSUS 本機儲存的詳細資訊，請參閱[**決定要將更新儲存在何處**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx)。

### <a name="BKMK_2"></a>步驟2：指定並填入 ISV 檔案快取 

#### <a name="specify-the-isv-file-cache"></a>指定 ISV 檔案快取

[**設定服務提供者參考**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference)中詳述的新用戶端群組原則和行動裝置管理 (MDM) 設定，會定義 ISV 檔案快取的位置。

| **名稱**                                              | **描述**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 設定更新的替代下載位置。 | 指定替代的內部網路伺服器來裝載 Microsoft Update 的更新。 然後，您可以使用此更新服務自動更新網路上的電腦 |

設定 ISV 檔案快取的替代下載位置時，有兩個選項可供使用：

1. **指定 ISV HTTP 伺服器主機名稱**，也就是 ISV 檔案快取
    
    此方法會設定 WU 用戶端，以對原則中指定的 HTTP 伺服器提出下載要求

2. **指定 localhost**
 
    此方法會設定 WU 用戶端，以對 localhost 提出下載要求。 如此，ISV 用戶端代理程式就可以適當處理這些要求和路由，以因應下載要求。

> [!IMPORTANT]
> ISV 檔案快取有下列需求：                                                          
> - 根據 RFC，伺服器必須與 HTTP 1.1 相容：<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> 具體而言，Web 伺服器必須支援 [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) 和 [**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm) 要求<br>                                                                                                                                                                                                                                                                                                  - 部分範圍要求<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   - 保持連線<br>                                                                                                                                                                                                                                                                                                                                                                                                                            - 請勿使用 "Transfer-Encoding:chunked"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>填入 ISV 檔案快取

ISV 檔案快取必須填入與要安裝在受管理用戶端上的更新相關聯的檔案。 

**若要填入 ISV 檔案快取：**

1. 使用 [WSUS API](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) 存取 MU 服務更新的檔案路徑和檔案名稱。

    WSUS 伺服器上每個更新的中繼資料都會包含更新在 Microsoft Update 上的檔案路徑和檔案名稱，如下所示 (以粗體顯示的 Microsoft Update 主機名稱，後面接著檔案路徑和檔案名稱)： **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. 從 Microsoft Update 下載檔案，並使用下列兩種方法之一將檔案儲存在 ISV 檔案快取中： 

   - 使用**與 MU 服務相同的資料夾路徑**來儲存檔案

   - 使用 **ISV 定義的資料夾路徑**來儲存檔案

     讓 HTTP 伺服器 (或 localhost) 將 **HTTP GET** 要求 (參考 MU 資料夾路徑和檔案名稱) 重新導向至 ISV 檔案位置。

### <a name="BKMK_3"></a>步驟 3：設定 ISV 用戶端代理程式以指示 WU 用戶端作業

ISV 用戶端代理程式會使用下列建議的工作流程，為已核准的更新協調下載和安裝作業：

1.  ISV 用戶端代理程式呼叫 WU 用戶端，以對 WSUS 伺服器進行掃描

2.  掃描將一組適用的更新傳回至 WU 用戶端

3.  ISV 用戶端決定要核准、下載及安裝哪些更新

4.  ISV 用戶端代理程式呼叫 WU 用戶端，以下載核准的更新

5.  下載更新之後，ISV 用戶端代理程式會呼叫 WU 用戶端，以安裝核准的更新

如需使用 WU 用戶端來掃描、下載及安裝更新的詳細資訊，請參閱[搜尋、下載及安裝更新](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx)。

### <a name="download-workflow-options"></a>下載工作流程選項

以下兩個圖例說明從 ISV 檔案快取下載的工作流程選項：

![工作流程 1](../../media/express-update-delivery-isv-support/image1.png)

![工作流程 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>快速下載的運作方式

- 對於支援「快速」的作業系統更新，服務上有儲存兩種版本的檔案裝載︰

  - **完整版** -- 基本上會取代更新二進位檔的本機版本

  - **快速版** -- 包含修補裝置上現有二進位檔的差異。 

    更新的中繼資料中都會參照完整版和快速版，這已在掃描階段下載至用戶端。 

    **快速下載的運作方式如下︰**

    WU 用戶端會先嘗試下載快速版，並在特定情況下，於必要時回復為下載完整版 (例如，若是經由不支援位元組範圍要求的 Proxy)。

  1. 當 WU 用戶端起始快速下載時，**WU 會先下載虛設常式**，這是快速套件的一部分。

  2. **WU 用戶端會將此虛設常式傳送給 Windows 安裝程式**，安裝程式會使用虛設常式進行本機清查，並將裝置上的檔案差異與取得所提供的最新檔案版本所需的內容相比較。

  3. **Windows 安裝程式隨後會要求 WU 用戶端下載已判定為必要的範圍**。

  4. **WU 用戶端會下載這些範圍，並將其傳遞至 Windows 安裝程式**以套用該範圍，然後判斷是否需要額外的範圍。 此步驟會重複直到 Windows 安裝程式告知 WU 用戶端所有必要範圍皆已下載完畢為止。

  至此，下載即告完成，且更新已可供安裝。

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>傳遞最佳化如何減少頻寬耗用量

「傳遞最佳化 (DO)」是一個自我組織的分散式快取解決方案，適用於想要對作業系統更新、作業系統升級及應用程式降低頻寬耗用量的企業。 DO 可讓用戶端除了從指定的下載位置 (在此案例中為 ISV 檔案快取) 下載這些元素以外，還可從替代來源 (例如網路上的其他對等) 下載。

根據預設，在「Windows 10 企業版」和「教育版」中，DO 只允許在組織本身的網路上進行對等共用，但您可以使用「群組原則」和行動裝置管理 (MDM) 設定進行不同的設定。

如需 DO 的詳細資訊，請參閱[設定 Windows 10 更新的傳遞最佳化](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization)。
