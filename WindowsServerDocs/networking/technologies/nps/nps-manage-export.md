---
title: 匯出 NPS 設定以在另一部伺服器上匯入
description: 您可以使用本主題來瞭解如何在 Windows Server 2016 中匯出網路原則伺服器設定。
manager: brianlic
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a7f6de6b4ee0e442aed5f718c7025a6eb2e70388
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995469"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>匯出 NPS 設定以在另一部伺服器上匯入

適用於：Windows Server 2016

您可以從一個 NPS 匯入整個 NPS 設定（包括 RADIUS 用戶端和伺服器、網路原則、連線要求原則、登錄和記錄設定），以便在另一個 NPS 上匯入。

使用下列其中一個工具來匯出 NPS 設定：

- 在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中，您可以使用 Netsh，也可以使用 Windows PowerShell。
- 在 Windows Server 2008 R2 和 Windows Server 2008 中，使用 Netsh。

> [!IMPORTANT]
> 如果來源 NPS 資料庫的版本號碼高於目的地 NPS 資料庫的版本號碼，請勿使用此程式。 您可以從**netsh NPS show config**命令的顯示，來查看 NPS 資料庫的版本號碼。

由於 NPS 設定不會在匯出的 XML 檔案中加密，因此透過網路傳送會造成安全性風險，因此，請在將 XML 檔案從來源伺服器移至目的地伺服器時採取預防措施。 例如，將檔案新增至已加密且受密碼保護的封存檔案，然後再移動檔案。 此外，將檔案儲存在安全的位置，以防止惡意使用者存取該檔案。

> [!NOTE]
> 如果在來源 NPS 上設定 SQL Server 記錄，SQL Server 記錄設定不會匯出至 XML 檔案。 在您將檔案匯入另一個 NPS 之後，您必須手動設定 SQL Server 記錄。

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>使用 Windows PowerShell 匯出和匯入 NPS 設定

針對 Windows Server 2012 和更新版本的作業系統版本，您可以使用 Windows PowerShell 匯出 NPS 設定。

用於匯出 NPS 設定的命令語法如下所示。

```powershell
Export-NpsConfiguration -Path <filename>
```

下表列出 Windows PowerShell 中**import-npsconfiguration** Cmdlet 的參數。 需要粗體的參數。

|參數|描述|
|---------|-----------|
|Path|指定您要匯出 NPS 設定之 XML 檔案的名稱和位置。|

**系統管理認證**

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="export-example"></a>匯出範例

在下列範例中，NPS 設定會匯出至位於本機磁片磁碟機上的 XML 檔案。 若要執行此命令，請以系統管理員身分在來源 NPS 上執行 Windows PowerShell，輸入下列命令，然後按 Enter。

```powershell
Export-NpsConfiguration –Path c:\config.xml
```

如需詳細資訊，請參閱[Export-import-npsconfiguration](/powershell/module/nps/export-npsconfiguration?view=win10-ps)。

匯出 NPS 設定之後，請將 XML 檔案複製到目的地伺服器。

在目的地伺服器上匯入 NPS 設定的命令語法如下所示。

```powershell
Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]
```

### <a name="import-example"></a>匯入範例

下列命令會從名為 C:\Npsconfig.xml 的檔案將設定匯入到 NPS。 若要執行此命令，請以系統管理員身分在目的地 NPS 上執行 Windows PowerShell，輸入下列命令，然後按 Enter。

```powershell
Import-NpsConfiguration -Path "C:\Npsconfig.xml"
```

如需詳細資訊，請參閱[import-npsconfiguration](/powershell/module/nps/import-npsconfiguration?view=win10-ps)。

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>使用 Netsh 匯出和匯入 NPS 設定

您可以使用 (Netsh) 的網路介面，使用**netsh NPS export**命令匯出 NPS 設定。

執行**netsh nps import**命令時，nps 會自動以更新的設定進行重新整理。 您不需要在目的地電腦上停止 NPS 來執行**netsh NPS import**命令，不過，如果 nps 主控台或 nps mmc 嵌入式管理單元在設定匯入期間已開啟，則在您重新整理此視圖之前，不會顯示伺服器設定的變更。

> [!NOTE]
> 當您使用**netsh nps export**命令時，您必須提供**ExportPSK**值為**YES**的命令參數。 此參數和值明確指出您要匯出 NPS 設定，而匯出的 XML 檔案包含 RADIUS 用戶端和遠端 RADIUS 伺服器群組成員的未加密共用密碼。

**系統管理認證**

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>使用 Netsh 命令將 NPS 設定複製到另一個 NPS

1. 在來源 NPS 上，開啟 [**命令提示**字元]，輸入**netsh**，然後按 enter。

2. 在**netsh**提示字元中，輸入**nps**，然後按 enter。

3. 在**netsh nps**提示字元中，輸入**export filename =**"*path\file.xml*" **exportPSK = YES**，其中*path*是您要儲存 nps 設定檔的資料夾位置，而*file*是您想要儲存的 XML 檔案名。 按 Enter 鍵。

    這會儲存設定 (包括 XML 檔案中) 的登錄設定。 路徑可以是相對或絕對路徑，也可以是 (UNC) 路徑的通用命名慣例。 按下 Enter 之後，會出現一則訊息，指出匯出至檔案是否成功。

4. 將您建立的檔案複製到目的地 NPS。

5. 在目的地 NPS 的命令提示字元中，輸入**netsh NPS import filename =**"*path\file.xml*"，然後按 enter。 此時會出現一則訊息，指出是否已成功從 XML 檔案匯入。

## <a name="additional-references"></a>其他參考資料

- [網路殼層 (Netsh)](../netsh/netsh.md)