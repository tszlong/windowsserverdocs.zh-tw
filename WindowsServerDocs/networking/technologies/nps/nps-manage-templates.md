---
title: 管理 NPS 範本
description: 本主題提供如何建立、適用於、匯出與匯入 NPS 範本適用於 Windows Server 2016 中的網路原則伺服器上的指示操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>管理 NPS 範本

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用的網路原則伺服器 \(NPS\) 範本建立設定項目，例如撥號使用者服務遠端驗證 \(RADIUS\) 戶端或共用的密碼，您可以在本機伺服器 NPS 及匯出其他 NPS 伺服器上使用重複使用。 

管理範本提供 NPS 主機，您可以建立、修改、delete、複製，且檢視 NPS 範本使用中的節點。 NPS 範本專為減少的時間和成本所需設定 NPS 一或多個的伺服器上。

下列 NPS 範本類型是適用於管理範本 \] 中的設定。

- **共用密碼**。 這個範本輸入可讓您指定共用的密碼，您可以選取重複使用（範本 NPS 主機的適當位置中）時設定 RADIUS 戶端與伺服器。 

- **RADIUS 戶端**。 這個範本輸入可讓您進行 RADIUS client 設定，您可以在適當的位置在 NPS 主控台中，選取範本重複使用。

- **遠端 RADIUS 伺服器]**。 此範本可讓您設定遠端 RADIUS 伺服器，您可以在適當的位置在 NPS 主控台中，選取範本重複使用。 

- **IP 篩選器**。 此範本可讓您建立網際網路通訊協定第 4 (IPv4) 和「網際網路通訊協定第 6 \(IPv6\) 篩選器，您可以重新使用 \（來選取範本 NPS console\ 的適當位置中）時的網路原則設定。

## <a name="create-an-nps-template"></a>建立 NPS 範本

設定範本與不同直接設定 NPS 伺服器。 建立範本不會影響 NPS 伺服器的功能。 它是只有當您選取適當的位置在 NPS 主控台範本和套用範本範本影響 NPS 伺服器功能。 

例如，如果您設定在主機 NPS RADIUS client **RADIUS 戶端與伺服器**，您改變 NPS 伺服器設定，並需要設定 NPS 與您的網路存取伺服器其中一個步驟。 \ (下一步就是設定網路的存取伺服器通訊具有 NPS \(NAS\)。\) 

不過，如果您設定新的**RADIUS 戶端**範本 NPS 在主機中的**管理範本**而非建立新的 RADIUS client 在**RADIUS 戶端與伺服器**，您已建立範本，但尚未您有未變更 NPS 伺服器功能。 若要變更的 NPS server 功能，您必須從 NPS 主控台正確的位置套用範本。

下列程序如何建立新的範本指示。

資格在**系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-create-an-nps-template"></a>若要建立 NPS 範本


1. NPS 伺服器，在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**。 NPS 主控台開啟。 

2. 在 NPS 主控台中，展開**管理範本**，例如用滑鼠右鍵按一下範本類型，**RADIUS 戶端**，然後按一下 [**新**。

3. 新範本屬性對話方塊，您可以使用設定範本。

## <a name="apply-an-nps-template"></a>適用於 NPS 範本

您可以使用您已經建立範本**管理範本**藉由瀏覽至您可以在此套用範本 NPS 主機的位置。 例如，如果您想要套用共用機密範本 RADIUS client 設定，您可以使用下列程序。

資格在**系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-apply-an-nps-template"></a>若要套用 NPS 範本

1. NPS 伺服器，在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**。 NPS 主控台開啟。

2. 在 [NPS 主控台中，展開**RADIUS 戶端與伺服器**，然後展開**RADIUS 戶端**。

3.的**RADIUS 戶端**，在詳細資料窗格中，以滑鼠右鍵按一下您想要套用 NPS 範本，然後按一下 [的 RADIUS client**屬性**。

4. 在屬性] 對話方塊方塊 RADIUS client，在**選取現有的共用機密範本**，選取您想要從清單中範本套用範本。

## <a name="export-or-import-nps-templates"></a>匯出或匯入 NPS 範本

您可以匯出範本，以便使用其他 NPS 伺服器，或您可以匯入到範本**管理範本**在本機電腦上使用。 

資格在**系統管理員**，或相當於，才能完成此程序最小值。

### <a name="to-export-or-import-nps-templates"></a>若要匯出或匯入 NPS 範本

1. 若要匯出 NPS 範本，在 NPS 主控台中，以滑鼠右鍵按一下**管理範本**，然後按一下 [**檔案匯出範本**。

2. 匯入 NPS 範本，在 NPS 主控台中，以滑鼠右鍵按一下**管理範本**，然後按一下 [**匯入電腦的範本**或**匯入範本，從檔案**。


