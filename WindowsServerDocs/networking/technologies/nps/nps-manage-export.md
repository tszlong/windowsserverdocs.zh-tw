---
title: 在另一部伺服器匯入匯出 NPS 伺服器設定
description: 您可以使用本主題以了解如何匯出在 Windows Server 2016 的網路原則伺服器設定。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 141a99e930672d8403315cb6804290d184ef3007
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="export-an-nps-server-configuration-for-import-on-another-server"></a>在另一部伺服器匯入匯出 NPS 伺服器設定

適用於：Windows Server 2016

您可以將匯出整個 NPS 設定，包括戶端 RADIUS 伺服器、網路原則、連接要求原則、登錄，並登入設定，從在另一部 NPS 伺服器匯入一個 NPS 伺服器。 

您可以使用其中一項下列工具來匯出 NPS 設定：

- Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012，您可以使用 Netsh，或您可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008、使用 Netsh。

>[!IMPORTANT]
>如果來源 NPS 資料庫具有更高版本號碼比目的 NPS 資料庫的版本號碼，請使用此程序。 您可以檢視 NPS 資料庫中顯示的版本號碼**netsh nps 顯示設定**命令。

因為 NPS 伺服器設定未加密匯出 XML 檔案，在傳送該網路可能的安全性風險，因此時帶上預防措施 XML 檔案的來源伺服器前往目的地的伺服器。 例如，檔案中加入檔案已加密，密碼受保護的保存日前檔案。 此外，請將檔案儲存在安全的位置，以防止惡意使用者存取。

>[!NOTE]
>如果來源 NPS 伺服器上設定 SQL Server 登入，則是不會 XML 檔案匯出 SQL Server 登入設定。 匯入另一個 NPS 伺服器上的檔案之後，您必須手動設定 SQL Server 登入。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>匯出與匯入 NPS 設定，請使用 Windows PowerShell

適用於 Windows Server 2012 和較新的作業系統版本，您可以匯出使用 Windows PowerShell NPS 設定。

以下是命令語法匯出 NPS 設定。 

    Export-NpsConfiguration -Path <filename>

下表列出參數適用於**匯出-NpsConfiguration**在 Windows PowerShell cmdlet。 是必要的參數以粗體顯示。

|參數|描述|
|---------|-----------|
|路徑|指定您要匯出 NPS 伺服器設定 XML 檔案的位置與名稱。|

**管理認證**

若要完成此程序，您必須是系統管理員群組成員。

### <a name="export-example"></a>匯出範例 

以下的範例、NPS 設定匯出到本機磁碟機上 XML 檔案。 若要執行這個命令時，以系統管理員身分執行來源 NPS 伺服器上的 Windows PowerShell、輸入下列命令，並按下 Enter。

`Export-NpsConfiguration –Path c:\config.xml` 

如需詳細資訊，請查看[匯出-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。

您有匯出 NPS 設定之後，將 XML 檔案複製到目的伺服器。

匯入 NPS 設定目的伺服器上的命令語法如下所示。

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>匯入範例

下列命令從匯入到 NPS C:\Npsconfig.xml 名為的檔案。 若要執行這個命令時，目的 NPS 伺服器以系統管理員身分執行 Windows PowerShell、輸入下列命令，並按下 Enter。

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

如需詳細資訊，請查看[NpsConfiguration 匯入的](https://technet.microsoft.com/library/jj872750.aspx)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>匯出與匯入 NPS 設定使用 Netsh

您可以使用網路殼層 \(Netsh\) 匯出 NPS 伺服器設定使用**netsh nps 匯出**命令。

當**netsh nps 匯入**命令的執行、NPS 的更新的設定自動更新。 您不需要停止 NPS 目的地電腦上執行**netsh nps 匯入**命令，但是如果 NPS 主機或 NPS MMC 嵌入式管理單元，開放設定匯入期間伺服器設定的變更不見直到您重新整理檢視。 

>[!NOTE]
>當您使用**netsh nps 匯出**命令時，會要求您提供命令的參數**exportPSK**的值**[是]**。 此參數和值明確狀態，您知道，您要匯出 NPS 伺服器設定，並包含匯出之的 XML 檔案加密共用的密碼 RADIUS 戶端和遠端 RADIUS 伺服器群組成員。

**管理認證**

若要完成此程序，您必須是系統管理員群組成員。

### <a name="to-copy-an-nps-server-configuration-to-another-nps-server-using-netsh-commands"></a>NPS 伺服器設定複製到另一個 NPS 伺服器使用 Netsh 命令

1. 在來源 NPS 伺服器，請打開**命令提示字元**，輸入**netsh**，然後按 Enter 鍵。

2. 在**netsh**提示，輸入**nps**，然後按 Enter 鍵。 

3. 在**netsh nps**提示，請輸入**匯出檔案名稱 =**」*path\file.xml*] **exportPSK = 是**，其中*路徑*是您想要儲存 NPS 伺服器設定檔的資料夾位置和*檔案*是您想要儲存 XML 檔案的名稱。 按 Enter。 

這會將設定儲存 \(including registry settings\) XML 檔案。 路徑可以相關或完全一致，或通用命名規格 \(UNC\) 路徑。 按下 Enter 後，會顯示訊息表示是否成功匯出檔案。

4. 將您建立的檔案複製到目的 NPS 伺服器。

5. 目的地 NPS 伺服器上命令提示字元中，輸入**netsh nps 匯入檔案名稱 =**]*path\file.xml*」，然後按 Enter 鍵。 就會顯示訊息表示是否 XML 檔案從匯入成功。

如需 netsh 的詳細資訊，請查看[網路 Shell (Netsh)](../netsh/netsh.md)。

