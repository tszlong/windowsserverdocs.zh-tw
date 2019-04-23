---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: 讓用戶端找出下一個最近的網域控制站
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880359"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>讓用戶端找出下一個最近的網域控制站

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果您有執行 Windows Server 2008 或更新版本的網域控制站時，您可以讓它可以執行 Windows Vista 的用戶端電腦或更新版本或 Windows Server 2008 或更新版本才可啟用更有效率地找到網域控制站**試用下一步最接近的站台**群組原則設定。 這項設定可以協助簡化網路流量，特別是在有許多的分公司與站台的大型企業提升網域控制站定位程式 （DC 定位程式）。

這個新的設定可能會影響設定站台連結成本，因為它會影響網域控制站位於的順序。 對於具有許多中樞站台和分公司的企業，您可以確保，用戶端容錯移轉至下一個最接近的中樞站台最接近的中樞站台中找不到網域控制站，大幅減少在網路上的 Active Directory 流量。

一般的最佳作法是，您應該簡化您的站台拓撲，以及站台連結成本是盡量如果您啟用**嘗試下一個最接近的網站**設定。 在企業中具有許多的中樞站台，這可簡化您進行處理，在一個站台的用戶端需要容錯移轉至另一個站台中的網域控制站的情況下的任何方案。

根據預設，**嘗試下一個最接近的網站**未啟用設定。 不啟用此設定時，DC 定位程式會使用下列演算法來尋找網域控制站：

- 嘗試尋找網域控制站位於相同站台。
- 如果沒有網域控制站位於相同的站台，嘗試尋找網域中的任何網域控制站。

> [!NOTE]
> 這是在舊版的 Active Directory 中的 DC 定位程式使用相同的演算法。 如需詳細資訊，請參閱文章[如何 Active Directory 的運作方式的 DNS 支援](https://go.microsoft.com/fwlink/?LinkId=108587)。

如果您啟用**嘗試下一個最接近的網站**設定，DC 定位程式會使用下列演算法來尋找網域控制站：

- 嘗試尋找網域控制站位於相同站台。
- 如果沒有網域控制站位於相同的站台，請嘗試在下一個最接近的網站尋找網域控制站。 網站是接近其是否比另一個站台成本較高站台連結成本較低站台連結。
- 如果沒有網域控制站功能在下一個最接近的網站，嘗試尋找網域中的任何網域控制站。

**嘗試下一個最接近的網站**設定協同運作，能使用自動站台涵蓋範圍。 例如，如果下一個最接近的網站不有任何網域控制站，DC 定位程式會嘗試尋找網域控制站，可針對該站台執行自動站台涵蓋範圍。

根據預設，DC 定位程式不會考慮包含唯讀網域控制站 (RODC)，它會決定下一個最接近的網站時的任何網站。 此外，用戶端會從執行版本早於 Windows Server 2008 的網域控制站，取得回應，DC 定位程式行為時，然後設定未啟用時相同。

例如，假設站台拓撲有四個網站，在下圖中的站台連結值。 在此範例中，所有網域控制站都是執行 Windows Server 2008 或更新版本的可寫入網域控制站。

![讓用戶端找到 dc](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

當**嘗試下一個最接近的網站**如果 Site_B 中的用戶端電腦嘗試找出網域控制站在此範例中，啟用群組原則設定，它會先嘗試自己 Site_B 中尋找網域控制站。 如果沒有在 Site_B，它會嘗試尋找 Site_A 中的網域控制站。

如果未啟用此設定，用戶端會嘗試尋找網域控制站 Site_A、 Site_C 或 Site_D 中，如果沒有網域控制站位於 Site_B。

> [!NOTE]
> **嘗試下一個最接近的網站**設定協同運作，能使用自動站台涵蓋範圍。 例如，如果下一個最接近的網站不有任何網域控制站，DC 定位程式會嘗試尋找網域控制站，可針對該站台執行自動站台涵蓋範圍。

若要套用**嘗試下一個最接近的網站**設定，您可以建立群組原則物件 (GPO)，並將它連結至適當的物件中，為您的組織，或您可以修改預設網域原則，就會影響執行 Windows 的所有用戶端Vista 或更新版本和 Windows Server 2008 或更新版本的網域中。 如需有關如何設定**嘗試下一個最接近的網站**設定，請參閱[允許用戶端中下一個最接近的網站尋找網域控制站](https://technet.microsoft.com/library/cc772592.aspx)。
