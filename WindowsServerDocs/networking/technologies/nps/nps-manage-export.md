---
title: 匯入另一部伺服器匯出 NPS 設定
description: 您可以使用本主題以了解如何匯出 Windows Server 2016 中的網路原則伺服器設定。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5f28c317f1d58fd1889fb55d345463dc8a62999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816419"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>匯入另一部伺服器匯出 NPS 設定

適用於：Windows Server 2016

您可以匯出整個 NPS 設定，包括 RADIUS 用戶端和伺服器、 網路原則、 連線要求原則、 登錄，以及記錄組態，從一個匯入另一部 NPS 上的 NPS。 

您可以使用其中一個下列工具來匯出 NPS 設定：

- 在 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中，您可以使用 Netsh，或您可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008 中，使用 Netsh。

>[!IMPORTANT]
>如果來源 NPS 資料庫的版本號碼高於目的地 NPS 資料庫的版本號碼，請不要使用此程序。 您可以檢視的顯示方式的 NPS 資料庫的版本號碼**netsh nps 顯示組態**命令。

因為 NPS 設定不會加密在匯出的 XML 檔案中，透過網路傳送可能會造成安全性風險，因此在將 XML 檔案從來源伺服器移到目的地伺服器時採取預防措施。 比方說，將檔案加密的密碼保護的封存檔案，再移動檔案。 此外，將檔案儲存在安全的位置，以防止惡意使用者存取它。

>[!NOTE]
>如果來源 NPS 上設定 SQL Server 記錄，並不會將 SQL Server 記錄設定匯出至 XML 檔案。 匯入另一部 NPS 上的檔案之後，您必須手動設定 SQL Server 記錄。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>匯出和匯入使用 Windows PowerShell 的 NPS 設定

如需 Windows Server 2012 和更新版本的作業系統版本，您可以匯出 NPS 設定使用 Windows PowerShell。

匯出 NPS 設定的命令語法如下所示。 

    Export-NpsConfiguration -Path <filename>

下表列出的參數**匯出 NpsConfiguration**在 Windows PowerShell cmdlet。 以粗體顯示的參數都是必要項目。

|參數|描述|
|---------|-----------|
|路徑|指定您要匯出 NPS 設定 XML 檔案的位置與名稱。|

**系統管理認證**

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="export-example"></a>匯出範例 

在下列範例中，會匯出至 XML 檔案位於本機的磁碟機上的 NPS 設定。 若要執行此命令，以系統管理員身分執行 Windows PowerShell，在來源 NPS 上，輸入下列命令，，然後按 Enter。

`Export-NpsConfiguration –Path c:\config.xml` 

如需詳細資訊，請參閱 <<c0> [ 匯出 NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。

您已匯出 NPS 設定之後，將 XML 檔案複製到目的地伺服器。

匯入目的地伺服器上的 NPS 設定的命令語法如下所示。

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>匯入範例

下列命令會從名為 C:\Npsconfig.xml 到 NPS 的檔案，匯入設定。 若要執行此命令，以系統管理員身分執行 Windows PowerShell，目的地 NPS 上，輸入下列命令，，然後按 Enter。

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

如需詳細資訊，請參閱 <<c0> [ 匯入 NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>匯出和匯入使用 Netsh 的 NPS 設定

您可以使用網路殼層\(Netsh\)若要使用匯出 NPS 設定**netsh nps 匯出**命令。

當**netsh nps 匯入**執行命令時，NPS 會自動重新整理更新的組態設定。 您不需要停止執行在目的地電腦上的 NPS **netsh nps 匯入**命令，不過如果 NPS 主控台或 NPS MMC 嵌入式管理單元開啟組態匯入期間，伺服器組態的變更不可見之前您重新整理檢視。 

>[!NOTE]
>當您使用**netsh nps 匯出**命令時，您必須提供的命令參數**exportPSK**值**是**。 此參數與值時明確陳述您瞭解您要匯出 NPS 設定，，和匯出的 XML 檔案包含未加密共用的密碼的 RADIUS 用戶端和遠端 RADIUS 伺服器群組的成員。

**系統管理認證**

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>若要將 NPS 設定複製到另一部 NPS 使用 Netsh 命令

1. 在來源 NPS 中，開啟**命令提示字元**，型別**netsh**，然後按 Enter 鍵。

2. 在**netsh**提示中輸入**nps**，然後按 Enter 鍵。 

3. 在**netsh nps**提示中輸入**匯出的檔案名稱 =**」*path\file.xml*" **exportPSK = YES**，其中*路徑*是您要將 NPS 設定檔案中，儲存的資料夾位置以及*檔案*是您想要儲存的 XML 檔案的名稱。 按 Enter 鍵。 

這會儲存組態設定\(包括登錄設定\)XML 檔案中。 路徑可以是相對或絕對的或它可以是通用命名慣例\(UNC\)路徑。 您按下 Enter 之後，會出現訊息，指出是否已成功匯出至檔案。

4. 將您建立的檔案複製到目的地 NPS。

5. 在目的地 NPS 的命令提示字元，輸入**netsh nps 匯入的檔案名稱 =**"*path\file.xml*"，然後按 Enter 鍵。 出現訊息，指出是否已成功從 XML 檔案匯入。

如需 netsh 的詳細資訊，請參閱[網路殼層 (Netsh)](../netsh/netsh.md)。

