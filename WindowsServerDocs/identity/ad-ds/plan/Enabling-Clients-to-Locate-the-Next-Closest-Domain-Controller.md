---
description: 深入瞭解：讓用戶端找出下一個最接近的網域控制站
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: 讓用戶端找出下一個最近的網域控制站
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 1907cf1dd857c66cc2d1bd4ce300edaf4e58b7d5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046046"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>讓用戶端找出下一個最近的網域控制站

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您有執行 Windows Server 2008 或更新版本的網域控制站，您可以啟用 [ **嘗試下一個最接近的網站]** 群組原則設定，讓執行 windows Vista 或更新版本或 Windows Server 2008 或更新版本的用戶端電腦可以更有效率地找到網域控制站。 這項設定可協助簡化網路流量，以改善網域控制站定位器 (DC 定位器) ，尤其是在具有許多分公司和網站的大型企業中。

這項新設定可能會影響您設定站台連結成本的方式，因為它會影響網域控制站的位置。 對於有許多中樞網站和分公司的企業，您可以藉由確保用戶端在最接近的中樞網站找不到網域控制站時，容錯移轉至下一個最接近的中樞網站，以大幅降低網路上的 Active Directory 流量。

一般最佳作法是，如果您啟用 [ **嘗試下一個最接近的網站]** 設定，就應該盡可能簡化您的網站拓撲和站台連結成本。 在具有許多中樞網站的企業中，這可以簡化任何您所做的計畫，以處理某個網站中的用戶端需要容錯移轉至另一個網站中的網域控制站的情況。

依預設，不會啟用 [ **嘗試下一個最接近的網站]** 設定。 當設定未啟用時，DC 定位器會使用下列演算法來找出網域控制站：

- 嘗試在相同的網站中尋找網域控制站。
- 如果沒有任何網域控制站可在相同的網站中使用，請嘗試尋找網域中的任何網域控制站。

> [!NOTE]
> 這與在舊版 Active Directory 中使用 DC 定位器的演算法相同。 如需詳細資訊，請參閱 [Active Directory 的 DNS 支援如何運作](/previous-versions/windows/it-pro/windows-server-2003/cc759550(v=ws.10))的文章。

如果您啟用 [ **嘗試下一個最接近的網站]** 設定，DC 定位器會使用下列演算法找出網域控制站：

- 嘗試在相同的網站中尋找網域控制站。
- 如果沒有任何網域控制站可在相同的網站中使用，請嘗試在下一個最接近的網站中尋找網域控制站。 如果網站的網站連結成本低於網站連結成本較高的其他網站，則網站會更接近。
- 如果下一個最接近的網站沒有可用的網域控制站，請嘗試尋找網域中的任何網域控制站。

[ **嘗試下一個最接近的網站]** 設定可以與自動網站涵蓋範圍協調。 例如，如果下一個最接近的網站沒有網域控制站，DC 定位器就會嘗試尋找為該網站執行自動網站涵蓋範圍的網域控制站。

根據預設，DC 定位程式不會將包含唯讀網域控制站的任何網站視為 (RODC) 在判斷下一個最接近的網站時。 此外，當用戶端從執行 Windows Server 2008 之前版本的網域控制站取得回應時，DC 定位器行為會與 [設定未啟用] 時相同。

例如，假設網站拓撲有四個網站，且下圖中有站台連結值。 在此範例中，所有網域控制站都是執行 Windows Server 2008 或更新版本的可寫入網域控制站。

![啟用用戶端以找出 dc](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

在此範例中啟用 [ **嘗試下一個最接近的網站]** 群組原則設定時，如果 Site_B 中的用戶端電腦嘗試找出網域控制站，它會先嘗試在自己的 Site_B 中尋找網域控制站。 如果 Site_B 中沒有任何可用的，它會嘗試在 Site_A 中尋找網域控制站。

如果未啟用此設定，用戶端會嘗試尋找 Site_A、Site_C 或 Site_D 中的網域控制站（若 Site_B 中沒有可用的網域控制站）。

> [!NOTE]
> [ **嘗試下一個最接近的網站]** 設定可以與自動網站涵蓋範圍協調。 例如，如果下一個最接近的網站沒有網域控制站，DC 定位器就會嘗試尋找為該網站執行自動網站涵蓋範圍的網域控制站。

若要套用 [ **嘗試下一個最接近的網站]** 設定，您可以 (GPO) 建立群組原則物件，並將它連結到您組織的適當物件，也可以修改預設網域原則，使其影響執行 Windows Vista 或更新版本的所有用戶端，以及網域中的 windows Server 2008 或更新版本。 如需有關如何設定 [ **嘗試下一個最接近的網站]** 設定的詳細資訊，請參閱 [讓用戶端在下一個最接近的網站中找出網域控制站](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772592(v=ws.10))。
