---
title: 在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)
description: 瞭解如何在啟用 BranchCache 的 Web 和檔案伺服器上強制建立內容資訊（也稱為雜湊）。
manager: brianlic
ms.topic: how-to
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: d903338f58cd7a01967902243f79f4eb28e58882
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97942504"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>在託管快取伺服器上預先雜湊處理並預先載入內容 (選擇性)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來強制在啟用 BranchCache 的 Web 和檔案伺服器上建立內容資訊（也稱為雜湊）。 您也可以將檔案和網頁伺服器上的資料收集到可傳送至遠端託管快取伺服器的封裝中。  這可讓您預先載入遠端託管快取伺服器上的內容，讓資料可供第一個用戶端存取。

您必須是系統 **管理員** 的成員，或等同于執行此程式。

### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash 內容並預先載入託管快取伺服器上的內容

1.  登入包含您想要預先載入之資料的檔案或網頁伺服器，並找出您想要在一或多部遠端託管快取伺服器上載入的資料夾和檔案。

2.  以系統管理員身分執行 Windows PowerShell。 針對每個資料夾和檔案，執行 `Publish-BCFileContent` 命令或 `Publish-BCWebContent` 命令（視內容伺服器的類型而定），以觸發雜湊產生和將資料加入至資料封裝。

3.  將所有資料新增至資料封裝之後，請使用 `Export-BCCachePackage` 命令來產生資料封裝檔案，以將資料匯出。

4.  使用您選擇的檔案傳輸技術，將資料封裝檔案移至遠端託管快取伺服器。  FTP、SMB、HTTP、DVD 和便攜硬碟都是可行的傳輸。

5.  使用命令，在遠端託管快取伺服器上匯入資料封裝檔案 `Import-BCCachePackage` 。


