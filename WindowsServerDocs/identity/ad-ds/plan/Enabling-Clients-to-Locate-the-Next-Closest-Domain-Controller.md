---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: 讓用戶端找出下一個最近的網域控制站
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 75e31a435e8d8411fbe4db242e6d31fd7676fe4e
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624246"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>讓用戶端找出下一個最近的網域控制站

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的網域控制站執行 Windows Server 2008 或更新版本，您可以藉由啟用 [**嘗試下一個最接近的網站]** 群組原則設定，讓執行 windows Vista 或更新版本或 Windows Server 2008 或更新版本的用戶端電腦更有效率地找出網域控制站。 這項設定藉由協助簡化網路流量（特別是在有許多分公司和網站的大型企業），藉此改善網域控制站定位器（DC 定位程式）。

這項新設定可能會影響您設定站台連結成本的方式，因為它會影響網域控制站的位置順序。 對於具有許多中樞網站和分公司的企業，您可以藉由確保用戶端在最接近的中樞網站中找不到網域控制站時，容錯移轉至下一個最接近的中樞網站，藉以大幅減少網路上 Active Directory 流量。

一般的最佳作法是，如果您啟用 [**嘗試下一個最接近的網站]** 設定，則應該盡可能簡化您的網站拓朴和站台連結成本。 在有許多中樞網站的企業中，這可以簡化任何您所做的計畫，以便處理某個網站中的用戶端需要故障切換至另一個網站中的網域控制站。

預設不會啟用 [**嘗試下一個最接近的網站]** 設定。 當設定未啟用時，DC 定位程式會使用下列演算法來尋找網域控制站：

- 嘗試在相同的網站中尋找網域控制站。
- 如果相同網站中沒有可用的網域控制站，請嘗試尋找網域中的任何網域控制站。

> [!NOTE]
> 這是 DC 定位器在舊版 Active Directory 中使用的相同演算法。 如需詳細資訊，請參閱[Active Directory 的 DNS 支援的運作方式](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759550(v=ws.10))一文。

如果您啟用 [**嘗試下一個最接近的網站**] 設定，DC 定位程式會使用下列演算法來尋找網域控制站：

- 嘗試在相同的網站中尋找網域控制站。
- 如果相同網站中沒有可用的網域控制站，請嘗試在下一個最接近的網站中尋找網域控制站。 如果網站的網站連結成本低於網站連結成本較高的另一個網站，網站就會更接近。
- 如果下一個最接近的網站中沒有可用的網域控制站，請嘗試尋找網域中的任何網域控制站。

[**嘗試下一個最接近的網站]** 設定會與自動網站涵蓋範圍協調運作。 例如，如果下一個最接近的網站沒有網域控制站，DC 定位程式會嘗試找出執行網站自動網站涵蓋範圍的網域控制站。

根據預設，DC 定位程式在判斷下一個最接近的網站時，不會考慮包含唯讀網域控制站（RODC）的任何網站。 此外，當用戶端從執行 Windows Server 2008 之前版本的網域控制站取得回應時，DC 定位程式的行為會與未啟用 [設定] 時相同。

例如，假設網站拓撲具有四個網站，其中包含下圖中的站台連結值。 在此範例中，所有網域控制站都是執行 Windows Server 2008 或更新版本的可寫入網域控制站。

![讓用戶端找出 dc](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

在此範例中啟用 [**嘗試下一個最接近的網站群組原則]** 設定時，如果 Site_B 中的用戶端電腦嘗試找出網域控制站，它會先嘗試在自己的 Site_B 中尋找網域控制站。 如果 Site_B 中沒有可用的，它會嘗試在 Site_A 中尋找網域控制站。

如果未啟用此設定，則用戶端會嘗試尋找 Site_A、Site_C 或 Site_D 中的網域控制站（如果 Site_B 中沒有可用的網域控制站）。

> [!NOTE]
> [**嘗試下一個最接近的網站]** 設定會與自動網站涵蓋範圍協調運作。 例如，如果下一個最接近的網站沒有網域控制站，DC 定位程式會嘗試找出執行網站自動網站涵蓋範圍的網域控制站。

若要套用 [**嘗試下一個最接近的網站]** 設定，您可以建立群組原則物件（GPO），並將它連結到您組織的適當物件，也可以修改預設網域原則，讓它影響網域中執行 Windows Vista 或更新版本和 Windows Server 2008 或更新版本的所有用戶端。 如需有關如何設定 [**嘗試下一個最接近的網站]** 設定的詳細資訊，請參閱在[下一個最接近的網站中啟用用戶端以尋找網域控制站](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772592(v=ws.10))。
