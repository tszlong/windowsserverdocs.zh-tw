---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量。
manager: brianlic
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 45adc982b7ced1a6cc40f978feee81202f822b0c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937527"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式來強制在啟用 BranchCache 的 Web 和檔案伺服器上建立內容資訊（也稱為雜湊）。 您也可以將檔案和 web 伺服器上的資料收集到可以傳輸到遠端託管快取伺服器的封裝中。  這讓您能夠在遠端託管快取伺服器上預先載入內容，讓資料可供第一次用戶端存取。

您必須是系統**管理員**的成員或同等許可權，才能執行此程式。

### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>預先雜湊內容並預先載入託管快取伺服器上的內容

1.  登入包含您要預先載入之資料的檔案或網頁伺服器，並找出您想要在一或多部遠端託管快取伺服器上載入的資料夾和檔案。

2.  以系統管理員身分執行 Windows PowerShell。 針對每個資料夾和檔案，執行 `Publish-BCFileContent` 命令或 `Publish-BCWebContent` 命令（視內容伺服器的類型而定），以觸發雜湊產生並將資料加入至資料封裝。

3.  將所有資料都新增至資料封裝之後，請使用命令將其匯出， `Export-BCCachePackage` 以產生資料封裝檔案。

4.  使用您選擇的檔案傳輸技術，將資料封裝檔案移至遠端託管快取伺服器。  FTP、SMB、HTTP、DVD 和便攜硬碟都是可行的傳輸。

5.  使用命令，在遠端託管快取伺服器上匯入資料封裝檔案 `Import-BCCachePackage` 。


