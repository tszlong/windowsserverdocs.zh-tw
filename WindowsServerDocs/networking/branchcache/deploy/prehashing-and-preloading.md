---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866999"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)

>適用於：Windows Server （半年通道），Windows Server 2016

若要強制建立內容的資訊-也稱為 啟用 BranchCache 的 Web 和檔案伺服器上的 雜湊集，您可以使用此程序。 您也可以收集檔案和 web 伺服器上的資料分成可以傳輸到遠端的託管快取伺服器的封裝。  這會讓您提供 預先載入遠端的託管快取伺服器上的內容，使資料可供第一次的用戶端存取的能力。  
  
您必須是隸屬**系統管理員**，或同等權限才能執行此程序。  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>預先雜湊，並預先載入託管快取伺服器上的內容  
  
1.  登入的檔案或包含您想要預先載入，資料的 Web 伺服器，並找出您想要載入一或多個遠端的託管快取伺服器的檔案和資料夾。  
  
2.  系統管理員身分執行 Windows PowerShell。 每個資料夾和檔案，請執行下列其中一`Publish-BCFileContent`命令或`Publish-BCWebContent`命令，視內容的伺服器，以觸發雜湊產生功能，並將資料加入至資料封裝的類型而定。  
  
3.  所有資料都加入至資料封裝之後，會將它匯出使用`Export-BCCachePackage`命令，以產生資料的封裝檔案。  
  
4.  使用您選擇的檔案傳輸技術，資料封裝檔案移至遠端的託管快取伺服器。  FTP、 SMB、 HTTP、 DVD 和可攜式硬碟則是所有可行的傳輸。  
  
5.  使用匯入資料的封裝檔案，在遠端的託管快取伺服器上`Import-BCCachePackage`命令。  
  

