---
title: 版本資訊-Windows Server 2019 中的重要問題
description: 摘要說明需要因應措施以避免發生當機，凸排安裝失敗，以及資料遺失的重要問題
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149888"
---
# 版本資訊-Windows Server 2019 中的重要問題

>適用於︰Windows Server 2019

這些版本資訊摘要說明 Windows Server 中最重要的問題&reg;2019年的作業系統，包括可避免或暫時解決問題，如果已知的方式。 設計變更、 新功能，並在此版本中的修正的相關資訊，請參閱[的 Windows Server 2019 中的新功能](whats-new-19.md)和特定功能小組的通知。 除非另行指定，每個報告的問題都適用於所有版本和 Windows Server 2019 的安裝選項。  

本文件將持續更新。 發現有需要採取因應措施的重大問題時，就會將它們加入，而在有新的因應措施和修正程式時也會加入。  
  
## 版本資訊
下列已知的問題會出現在 Windows Server 2019 中的。 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>標題</th>
      <th>說明</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>在伺服器設定期間安裝選項功能表已截斷德文文字</td>
      <td>桌面體驗安裝選項的描述在作業系統選取視窗標題為 「 選取您想要安裝時，作業系統 」，從德國伺服器媒體執行安裝程式時將會非常結尾會有遺失和不正確的字元句子。 以下是完整德文文字，因為它應該會出現。  
      <br/>
      <p><i>Durch diese 選項 wird 骰子 vollständige grafische Umgebung 莫 Windows installiert，wodurch zusätzlicher Speicherplatz verbraucht wird。 Sie kann hilfreich sein，wenn Sie 房間 Windows 桌面 verwenden möchten 順序 über eine 應用程式 verfügen、 die 骰子 grafische Umgebung benötigt。</i> </p>
      <p>這只會影響在公用可用性的 Windows Server 2019、 Windows Server 版本 1809，與 Microsoft HYPER-V Server 2019 發行德文媒體。</p></td>
    </tr>
    <tr>
      <td>Windows Server 在 Windows Server 版本 1809年安裝期間不正確的商標影像  </td>
      <td>在 Windows Server 版本 1809年的安裝程式體驗期間背景影像，在一些初始的螢幕上顯示的 Windows Server 2019 」。  做為 Windows server 版本 1709年及 1803，這應該只需說出的 Windows Server 」。  不有任何其他影響產品，在其他地方，也不會影響到 Windows Server 2019 產品。  在 Windows Server 版本 1809，僅適用於大量授權客戶存取大量授權服務中心安裝期間，問題是受限於此一個映像。  
      </td>
    </tr>
  </tbody>
</table>


### 著作權  
本文件是以原本的形式提供， 本文件中提供的資訊及檢視 (包括 URL 及其他網際網路網站參照) 如有變更，恕不另行通知。  

本文件不為您提供對任何 Microsoft 產品中的任何智慧財產的法定權利。 您可以複製本文件供內部參照之用。  

&copy;2019 Microsoft Corporation。 著作權所有，並保留一切權利。  

Microsoft、Active Directory、Hyper-V、Windows 及 WindowsServer 係 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。  

本項產品含有圖形篩選軟體，這個軟體有一部分是以 Independent JPEG Group 的作品為基礎。  


1.0  
