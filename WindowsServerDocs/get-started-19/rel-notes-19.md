---
title: 版本資訊-Windows Server 2019 的重要問題
description: 摘要說明需要因應措施以避免損毀，還是懸置安裝失敗，以及資料遺失的重要問題
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810736"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>版本資訊-Windows Server 2019 的重要問題

>適用於：Windows Server 2019

這些版本資訊摘要說明在 Windows Server 2019 作業系統，包括如何避免或解決問題，如果已知中最重要的問題。 設計變更、 新功能，並在此版本中修正的相關資訊，請參閱[What's New in Windows Server 2019](whats-new-19.md)和特定功能小組的通知。 除非另外指定，否則每個回報的問題適用於所有版本和安裝選項的 Windows Server 2019。  

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。  

## <a name="release-notes"></a>版本資訊

下列已知的問題會在 Windows Server 2019。

| 標題         | 描述                            |
| -----         | -----------                            |
| 在伺服器安裝期間安裝選項 功能表已截斷德文文字 | 當從德文伺服器媒體執行安裝程式，在作業系統選取視窗標題，[選取您想要安裝作業系統] 桌面體驗安裝選項的描述就會有遺漏及不正確字元結尾處句子。 這是它應該會出現完整德文的文字。<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>這只會影響德文發表公用可用性的 Windows Server 2019、 Windows Server、 版本 1809 和 Microsoft HYPER-V Server 2019 的媒體。|
| Windows Server 的 Windows Server 版 1809年安裝期間不正確的商標影像 | 適用於 Windows Server，版本 1809，安裝程式體驗期間背景上的映像一些初始畫面所示&quot;Windows Server 2019&quot;。  因為 Windows server，版本 1709年和 1803，這應該只說&quot;Windows Server&quot;。  沒有其他影響在產品中，其他地方，並不會影響 Windows Server 2019 產品。  在 Windows server 版 1809，只能存取大量授權服務中心的大量授權客戶可以使用安裝期間，問題在於限於這一個映像。<br/> |

### <a name="copyright"></a>著作權

本文件是以原本的形式提供， 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。  

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件，但僅限內部參考之用途。

&copy; 2019 Microsoft Corporation. 保留一切權利。  

Microsoft、Active Directory、Hyper-V、Windows 及 Windows Server 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。  

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。  


1.0  
