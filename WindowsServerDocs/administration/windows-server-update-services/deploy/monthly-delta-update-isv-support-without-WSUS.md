---
title: 每月的差異更新 ISV 支援，而不需要 WSUS
description: Windows Server Update Service (WSUS) 主題-如何獨立軟體廠商 (ISV) 可以暫時使用每月的差異更新而不是 WSUS Express 更新傳遞，以減少封裝大小
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 272d3865bbe1a9853f5349c5e878155351525ef0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439947"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>每月的差異更新 ISV 支援，而不需要 WSUS

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

Windows 10 更新的下載項目可能很大，因為每個套件包含所有先前發行的修正，以確保一致性和簡易性。  

第 7 版，因為 Windows 已經能夠減少大小的 Windows Update 下載使用稱為[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)，而且雖然消費型裝置都支援它依預設，Windows 10 企業版裝置需要 Windows Server Update若要善用 Express services (WSUS)。 如果您有可用的 WSUS，請參閱[Express 更新傳遞 ISV 支援](express-update-delivery-ISV-support.md)。 我們建議使用它來啟用快速更新傳遞。 

如果您目前沒有 WSUS 安裝，但您需要以較小更新的套件大小在過渡期間，您可以使用每月的差異更新。 差異更新但不是太一樣 WSUS Express 更新傳遞，請以本質上，減少封裝大小。 我們建議您部署 WSUS Express update，儘可能最大封裝大小降低為。 以下是比較差異、 累計和 Express 下載大小適用於 Windows 10 版本 1607年的圖表：

![下載大小比較](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>什麼是每月的差異更新？

有兩種變化的每月的安全性更新：差異並加裝累計。

每月的差異更新為新的並更新封裝大小為過渡性解決方案的 Isv 並沒有可幫助減少的 WSUS。

>[!IMPORTANT]
>**差異更新可供 Windows 10 及版本 1607 （年度更新版）、 版本 1703 (Creators Update)，版本 1709 (Fall Creators Update) 的服務。** 針對版本 1709年版之後，您必須實作支援的部署基礎結構[Express 更新傳遞](express-update-delivery-ISV-support.md)繼續利用累加式更新。

藉由使用每月的差異更新，套件只會包含一個月的更新。 每月累計包含該更新版本中，所有更新，導致每個月的成長的大型檔案。 在每個月，也稱為 「 更新星期二 」。 第二個星期二發行 Delta 和每月更新 下表比較差異和累計更新：

|                    | 每月**差異**更新                                                                                                                                                                                                       | 每月**累計**更新                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **範圍**          | 使用單一更新**當月只有新的修正**                                                                                                                                                                           | 單一的更新與該月份和所有前幾個月的所有新的修正                                                                                                                                                   |
| **應用程式**    | 只可以套用上個月的更新時套用 （累計或差異）                                                                                                                                           | 可以套用在任何時間                                                                                                                                                                                                |
| **傳遞**       | **發行至 Windows Update Catalog 只**位置下載它以搭配其他工具或程序。 不提供來連線到 Windows Update 的電腦                                                         | 發行至 Windows Update （其中所有取用者的電腦將會安裝它）、 WSUS 和 Windows 的更新類別目錄                                                                                                                |

差異和重試累計有相同的 KB 數，具有相同的分類，並在同一時間發行。 在目錄中，可能是更新標題或 msu 的名稱，則可以分辨更新：

- 2017-02 *\***差異更新**\**  的 x64 型系統 (KB1234567) 的 Windows 10 版本 1607年
- 2017-02 *\***累計更新**\**  的 x86 型系統 (KB1234567) 的 Windows 10 版本 1607年                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>使用差異的每月更新的時機

如果用戶端裝置更新的大小會產生問題，建議使用具有上個月的更新和累計更新進度落後的裝置上的裝置上的差異更新。 如此一來，所有的裝置只需要單一的更新，即可將其最新狀態。 因為您必須根據狀態的裝置是組織中的不同更新部署，這會需要整體的更新管理程序，在小型的調整：

![下載大小比較](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>避免在同一個月份的差異和累計更新的部署

因為差異更新和累計更新可用的同時，務必了解會發生什麼事如果您將部署在同一個月份的兩個更新。

如果您核准並部署相同版本的差異和累計更新時，您不只會產生額外的網路流量因為兩者都將會下載至電腦上，但您可能無法在重新啟動後重新開機至 Windows 電腦。

如果不小心安裝差異和累計更新，而且不會再開機電腦，您可以復原下列步驟：

1. 開機到 WinRE 命令提示字元
2. 列出處於擱置狀態的封裝：

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **範例**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. 開啟文字檔案，其中您經由管道輸送**取得套件**。 如果您看到任何安裝擱置中的修補程式，請執行**移除封裝**針對每個封裝名稱：
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **範例**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >請勿移除暫止的修補程式解除安裝。

>[!IMPORTANT]
>**差異更新可供 Windows 10 及版本 1607 （年度更新版）、 版本 1703 (Creators Update)，版本 1709 (Fall Creators Update) 的服務。** 針對版本 1709年版之後，您必須實作支援的部署基礎結構[Express 更新傳遞](express-update-delivery-ISV-support.md)繼續利用累加式更新。
