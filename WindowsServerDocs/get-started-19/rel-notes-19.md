---
title: 版本資訊 - Windows Server 2019 的重要問題
description: 摘要說明需要因應措施以避免損毀、停止回應、安裝失敗和資料遺失的重要問題
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810736"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>版本資訊 - Windows Server 2019 的重要問題

>適用於：Windows Server 2019

這些版本資訊摘要說明 Windows Server 2019 作業系統中最重要的問題，包括可避免或暫時解決問題 (如果已知) 的方式。 如需這個版本的設計變更、新功能及修正資訊，請參閱 [Windows Server 2019 新功能](whats-new-19.md)和特定功能小組的通知。 除非另有說明，否則這其中每個回報的問題都適用於 Windows Server 2019 所有版本和安裝選項。  

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。  

## <a name="release-notes"></a>版本資訊

下列是 Windows Server 2019 的已知問題。

| 標題         | 描述                            |
| -----         | -----------                            |
| 伺服器安裝期間的安裝選項功能表已截斷德文文字 | 從德文伺服器媒體執行安裝程式時，在標題為 [選取您要安裝的作業系統] 的作業系統選取視窗上，桌面體驗安裝選項描述的句子結尾處將會有遺漏和不正確字元。 以下是應該出現的完整德文文字。<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>這只會影響 Windows Server 2019、Windows Server 1809 版和 Microsoft Hyper-V Server 2019 公開運作時所發行的德文媒體。|
| Windows Server 1809 版安裝期間的 Windows Server 商標影像不正確 | 在 Windows Server 1809 版的安裝體驗期間，某些初始畫面上的背景影像顯示 &quot;Windows Server 2019&quot;。  如同 Windows Server 1709 和 1803 版，這應該只顯示 &quot;Windows Server&quot;。  這不會對產品中的任何其他地方造成其他影響，也不會影響 Windows Server 2019 產品。  此問題僅限於 Windows Server 1809 版安裝期間的這個影像，並只適用於存取大量授權服務中心的大量授權客戶。<br/> |

### <a name="copyright"></a>著作權

本文件是以原本的形式提供， 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。  

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件，但僅限內部參考之用途。

&copy; 2019 Microsoft Corporation. 保留一切權利。  

Microsoft、Active Directory、Hyper-V、Windows 及 Windows Server 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。  

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。  


1.0  
