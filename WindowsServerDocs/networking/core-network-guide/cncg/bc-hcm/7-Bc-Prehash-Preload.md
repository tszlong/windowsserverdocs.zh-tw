---
title: Prehash 和預先載入 Content 裝載快取伺服器（選擇性）
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash 和預先載入 Content 裝載快取伺服器 \(Optional\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以在本區段中使用的程序，prehash content 內容伺服器上，將 content 資料的套件，然後再 content 預先載入您裝載快取的伺服器。 

下列程序是選擇性的因為您不需要 content prehash 和預先載入您裝載快取的伺服器上。 

如果您未進行預先載入 content，資料會裝載快取自動新增為戶端 WAN 連接到下載。

>[!IMPORTANT]
>雖然這些程序集體選用的如果您要 content prehash 和預先載入您裝載快取的伺服器，則需要執行這兩個程序。

- [建立網頁和檔案 Content 與 #40; 內容伺服器資料套件選擇性和 #41;](8-Bc-Data-Packages.md)
  
- [匯入的資料套件裝載快取伺服器與 #40; 選擇性和 #41;](9-Bc-Import-Data.md)

若要繼續使用此快速入門，請查看[建立內容伺服器資料套件網頁和檔案內容與 #40; 選用和 #41;](8-Bc-Data-Packages.md).