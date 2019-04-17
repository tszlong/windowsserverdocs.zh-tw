---
title: Prehashing 和預先載入 Content 裝載快取伺服器（選擇性）
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing 和預先載入 Content 裝載快取伺服器（選擇性）

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序強迫建立內容資訊-稱為 hashes-BranchCache 式網頁和檔案伺服器上。 您也可以插入套件，可以將它們傳輸到遠端裝載快取的伺服器，檔案與 web 伺服器收集資料。  這提供您預先載入 content 遠端裝載快取的伺服器上，有可用的第一次 client 存取資料的能力。  
  
您必須成員的**系統管理員**，或相當於執行此程序。  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash content 和預先載入裝載快取的伺服器上 content  
  
1.  登入的檔案或網頁伺服器可包含您想要預先載入，資料，並找出您想要載入一或多個遠端裝載快取的伺服器的檔案和資料夾。  
  
2.  系統管理員身分執行 Windows PowerShell。 每個資料夾和檔案，請執行`Publish-BCFileContent`命令或`Publish-BCWebContent`命令，而定內容伺服器 hash 代觸發程序，並新增至資料套件資料類型。  
  
3.  所有的資料已新增至資料套件之後，匯出使用`Export-BCCachePackage`命令製作資料套件檔案。  
  
4.  使用您選擇檔案的傳輸技術的資料套件檔案移至遠端裝載快取的伺服器。  FTP、SMB、HTTP、DVD 及可移植硬碟的所有可用傳輸。  
  
5.  使用匯入遠端裝載快取的伺服器上的資料套件檔案`Import-BCCachePackage`命令。  
  

