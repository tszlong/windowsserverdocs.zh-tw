---
title: 管理軟體定義網路的憑證
description: 您可以使用本主題來瞭解如何在 Windows Server 2019 和 2016 Datacenter 中部署軟體定義網路 (SDN) 時，管理網路控制站 Northbound 和 Southbound 通訊的憑證。
manager: grcusanz
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: bcba64a74a2414cb239257161de50bf99acbad62
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716344"
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理軟體定義網路的憑證

>適用於：Windows Server 2019、Windows Server 2016

您可以使用本主題來瞭解如何在 \( \) Windows Server 2019 或 2016 Datacenter 中部署軟體定義的網路 SDN，以及使用 System Center Virtual Machine Manager \( SCVMM \) 作為 SDN 管理用戶端時，管理網路控制站 Northbound 和 Southbound 通訊的憑證。

>[!NOTE]
>如需網路控制站的總覽資訊，請參閱 [網路控制](../technologies/network-controller/Network-Controller.md)站。

如果您不是使用 Kerberos 保護網路控制站通訊，則可以使用 x.509 憑證來進行驗證、授權和加密。

Windows Server 2019 和 2016 Datacenter 中的 SDN 支援自我 \- 簽署和憑證授權單位單位 \( CA \) 簽署的 x.509 憑證。 本主題提供逐步指示，說明如何建立這些憑證，並將其套用至安全的網路控制卡 Northbound 通道和網路裝置，例如軟體 Load Balancer \( SLB \) 。
.
當您使用以憑證 \- 為基礎的驗證時，您必須在使用下列方法的網路控制卡節點上註冊一個憑證。

1. 使用 \( \) 網路控制卡節點與管理用戶端（例如 System Center Virtual Machine Manager）之間的安全通訊端層 SSL，加密 Northbound 通訊。
2. 網路控制站節點與 Southbound 裝置和服務（例如 Hyper-v 主機和軟體負載平衡器 SLBs）之間的驗證 \( \) 。

## <a name="creating-and-enrolling-an-x509-certificate"></a>建立和註冊 x.509 憑證

您可以建立並註冊自我簽署的 \- 憑證，或 CA 所發行的憑證。

>[!NOTE]
>當您使用 SCVMM 部署網路控制站時，必須指定在網路控制站服務範本設定期間用來加密 Northbound 通訊的 x.509 憑證。

憑證設定必須包含下列值。

- **RestEndPoint** 文字方塊的值必須是網路控制站的完整功能變數名稱（ \( FQDN） \) 或 IP 位址。
- **RestEndPoint** 值必須符合 x.509 憑證的主體名稱 \( 一般名稱（CN） \) 。

### <a name="creating-a-self-signed-x509-certificate"></a>建立自我 \- 簽署的 X.509 憑證

您可以 \( \) 針對單一 \- 節點和 \- 網路控制站的多重節點部署，依照下列步驟，使用以密碼保護的私密金鑰來建立自我簽署的 x.509 憑證，並將其匯出。

當您建立自我 \- 簽署憑證時，您可以使用下列指導方針。

- 您可以針對 DnsName 參數使用網路控制站 REST 端點的 IP 位址，但不建議這麼做，因為它需要網路控制站節點全都位於單一管理子網（ \( 例如在單一機架上）\)
- 針對多個節點 NC 部署，您指定的 DNS 名稱將會成為網路控制站叢集 DNS 主機的 FQDN，而系統會 \( 自動建立記錄。\)
- 若為單一節點網路控制站部署，DNS 名稱可以是網路控制站的主機名稱，後面接著完整的功能變數名稱。

#### <a name="multiple-node"></a>多個節點

您可以使用 [new-selfsignedcertificate](/powershell/module/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我 \- 簽署的憑證。

**語法**

```powershell
New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")
```

**使用方式範例**

```powershell
New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")
```

#### <a name="single-node"></a>單一節點

您可以使用 [new-selfsignedcertificate](/powershell/module/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我 \- 簽署的憑證。

**語法**

```powershell
New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")
```

**使用方式範例**

```powershell
New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")
```

### <a name="creating-a-ca-signed-x509-certificate"></a>建立 CA \- 簽署的 X.509 憑證

若要使用 CA 建立憑證，您必須已部署 \( \) 具有 Active Directory 憑證服務 AD CS 的公開金鑰基礎結構 PKI \( \) 。

>[!NOTE]
>您可以使用協力廠商 Ca 或工具（例如 openssl）來建立與網路控制卡搭配使用的憑證，但本主題中的指示僅適用于 AD CS。 若要瞭解如何使用協力廠商 CA 或工具，請參閱您所使用之軟體的說明文件。

使用 CA 建立憑證包含下列步驟。

1. 您或貴組織的網域或安全性系統管理員設定憑證範本
2. 您或貴組織的網路控制站系統管理員或 SCVMM 系統管理員會向 CA 要求新憑證。

#### <a name="certificate-configuration-requirements"></a>憑證設定需求

當您在下一個步驟中設定憑證範本時，請確定您設定的範本包含下列必要元素。

1. 憑證主體名稱必須是 Hyper-v 主機的 FQDN
2. 憑證必須放在本機電腦個人存放區中， (My – cert： \ localmachine\my) 
3. 憑證必須同時具有伺服器驗證 (EKU： 1.3.6.1.5.5.7.3.1) 和用戶端驗證 (EKU： 1.3.6.1.5.5.7.3.2) 應用程式原則。

>[!NOTE]
>如果 \( hyper-v 主機上的 Personal My-cert： \ localmachine\my \) 憑證存放區 \- 有一個以上的 X.509 憑證具有主體名稱 (CN) 作為主機的完整功能變數名稱 \( FQDN \) ，請確定 SDN 使用的憑證有額外的自訂增強金鑰使用方法屬性與 OID 1.3.6.1.4.1.311.95.1.1.1。 否則，網路控制卡與主機之間的通訊可能無法正常執行。

#### <a name="to-configure-the-certificate-template"></a>設定憑證範本

>[!NOTE]
>在執行此程式之前，您應該在 [憑證範本] 主控台中檢查憑證需求和可用的憑證範本。 您可以修改現有的範本，或建立現有範本的複本，然後修改範本的複本。 建議您建立現有範本的複本。

1. 在安裝 AD CS 的伺服器上，按一下伺服器管理員中的 [ **工具**]，然後按一下 [ **憑證授權單位** 單位]。 [憑證授權單位單位] Microsoft Management Console \( MMC \) 開啟。
2. 在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下 [ **憑證範本**]，然後按一下 [ **管理**]。
3. [憑證範本] 主控台隨即開啟。 所有的憑證範本都會顯示在詳細資料窗格中。
4. 在詳細資料窗格中，按一下您要複製的範本。
5.  按一下 [ **動作** ] 功能表，然後按一下 [ **複製範本**]。 [範本 **屬性** ] 對話方塊隨即開啟。
6.  在 [範本 **屬性** ] 對話方塊的 [ **主體名稱** ] 索引標籤上，按一下 **[在要求中提供**]。 \(這是網路控制卡 SSL 憑證所需的設定。\)
7.  在 [範本 **屬性** ] 對話方塊的 [ **要求處理** ] 索引標籤上，確定已選取 [ **允許匯出私密金鑰** ]。 此外，請確定已選取 [簽章] **和 [加密** ] 目的。
8.  在 [範本 **屬性** ] 對話方塊的 [ **擴充** 功能] 索引標籤上，選取 [ **金鑰使用** 方法]，然後按一下 [ **編輯**]。
9.  在 [簽章 **] 中，** 確定已選取 [ **數位簽章** ]。
10.  在 [範本 **屬性** ] 對話方塊的 [ **擴充** 功能] 索引標籤上，選取 [ **應用程式原則**]，然後按一下 [ **編輯**]。
11.  在 [ **應用程式原則**] 中，確定已列出 [ **用戶端驗證** ] 和 [ **伺服器驗證** ]。
12.  將憑證範本的複本儲存為唯一的名稱，例如 **網路控制器範本**。

#### <a name="to-request-a-certificate-from-the-ca"></a>向 CA 要求憑證

您可以使用 [憑證] 嵌入式管理單元來要求憑證。 您可以要求處理憑證要求的 CA 系統管理員已預先設定並提供任何類型的憑證。

若要完成此程式，至少需要 **使用者** 或本機系統 **管理員** 的群組成員資格。

1. 開啟電腦的 [憑證] 嵌入式管理單元。
2. 在主控台樹中，按一下 [**憑證 \( 本機 \) 電腦**]。 選取 [ **個人** ] 憑證存放區。
3. 在 [ **動作** ] 功能表上，指向 [所有工作]<strong>，然後按一下 [要求新憑證</strong> ] 以啟動 [憑證註冊嚮導]。 按一下 [下一步] 。
4. 選取 **您的系統管理員** 憑證註冊原則所設定的，然後按 **[下一步]**。
5.  \( 根據您在上一節中設定的 CA 範本，選取 Active Directory 的註冊原則 \) 。
6. 展開 [ **詳細資料** ] 區段，並設定下列專案。
   1. 請確定 **金鑰使用** 方式同時包含 <strong>數位簽章 * * 和 * * 金鑰加密</strong>。
   2. 確定 **應用程式原則** 同時包含 **伺服器驗證** \( 1.3.6.1.5.5.7.3.1 \) 和 **用戶端驗證** \( 1.3.6.1.5.5.7.3.2 \) 。
7. 按一下 **[屬性]**。
8. 在 [ **主體** ] 索引標籤的 [ **主體名稱**] 的 [ **類型**] 中，選取 [ **一般名稱**]。 在 [值] 中，指定 **網路控制站 REST 端點**。
9. 按一下 [套用]，然後按一下 [確定]。
10. 按一下 **\[註冊\]**。

在 [憑證] MMC 中，按一下 [個人] 存放區，以查看您已從 CA 註冊的憑證。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>匯出憑證並將其複製到 SCVMM 程式庫

建立自我 \- 簽署或 CA \- 簽署的憑證之後，您必須以 .pfx 格式匯出具有私密金鑰的憑證，而在憑證嵌入式管理單元中，則 \( \) 不需要使用私 \( 用金鑰（以 64 .cer 格式 \) ）。

然後，您必須將兩個匯出的檔案複製到您在匯入 NC 服務範本時指定的 **ServerCertificate.cr** 和 **NCCertificate.cr** 資料夾。

1. 開啟 (certlm.msc 的 [憑證] 嵌入式管理單元) ，然後在本機電腦的個人憑證存儲中尋找憑證。
2. 以滑鼠右鍵 \- 按一下憑證，按一下 [ **所有** 工作]，然後按一下 [ **匯出**]。 [憑證匯出精靈] 隨即開啟。 按一下 [下一步] 。
3. 選取 [ **是**，匯出私密金鑰] 選項，按 **[下一步]**。
4. 選擇 [ **個人資訊交換-PKCS #12 (]。PFX)** 並接受預設值，以在可能的情況下 **包含憑證路徑中的所有憑證** 。
5. 針對您要匯出的憑證指派使用者/群組和密碼，然後按 **[下一步]**。
6. 在 [要匯出的檔案] 頁面上，瀏覽您要放置匯出檔案的位置，並指定其名稱。
7. 同樣地，匯出中的憑證。CER 格式。 注意︰若要匯出為 .CER 格式，請取消核取 [是，匯出私密金鑰] 選項。
8. 將 .PFX 複製到 ServerCertificate.cr 資料夾。
9. 將 .CER 檔案複製到 NCCertificate.cr 資料夾。

當您完成時，請重新整理 SCVMM 程式庫中的這些資料夾，並確定您已複製這些憑證。 繼續進行網路控制站服務範本設定和部署。

## <a name="authenticating-southbound-devices-and-services"></a>驗證 Southbound 裝置和服務

網路控制站與主機和 SLB MUX 裝置的通訊會使用憑證進行驗證。 與主機的通訊會透過 OVSDB 通訊協定，而與 SLB MUX 裝置的通訊則是透過 WCF 通訊協定來進行。

### <a name="hyper-v-host-communication-with-network-controller"></a>Hyper-v 主機與網路控制站的通訊

若要透過 OVSDB 與 Hyper-v 主機通訊，網路控制站必須向主機電腦出示憑證。 根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來 southbound 與主機的通訊。

這就是 SSL 憑證必須設定用戶端驗證 EKU 的原因。 此憑證設定于「伺服器」 REST 資源 (Hyper-v 主機會在網路控制站中表示為伺服器資源) ，而且可以藉由執行 Windows PowerShell 命令 **NetworkControllerServer** 來查看。

以下是伺服器 REST 資源的部分範例。

```
   "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],
```

若要進行相互驗證，Hyper-v 主機也必須有憑證才能與網路控制站通訊。

您可以從憑證授權單位單位 CA 註冊 \( 憑證 \) 。 如果在主機電腦上找不到 CA 型憑證，SCVMM 會建立自我簽署的憑證，並在主機電腦上進行布建。

網路控制站和 Hyper-v 主機憑證必須彼此信任。 Hyper-v 主機憑證的根憑證必須存在於本機電腦的 [網路控制站受信任的根憑證授權單位] 存放區中，反之亦然。

當您使用自我 \- 簽署憑證時，SCVMM 會確保本機電腦的「信任的根憑證授權單位」存放區中有必要的憑證。

如果您使用的是 Hyper-v 主機的 CA 型憑證，您必須確定 CA 根憑證存在於本機電腦的網路控制站受信任的根憑證授權單位存放區。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>軟體 Load Balancer MUX 與網路控制站的通訊

軟體 Load Balancer 多重通訊 \( \) 協定 MUX 和網路控制站會透過 WCF 通訊協定進行通訊，並使用憑證進行驗證。

根據預設，SCVMM 會挑選網路控制站上設定的 SSL 憑證，並使用它來與 Mux 裝置進行 southbound 通訊。 此憑證是在 "NetworkControllerLoadBalancerMux" REST 資源上設定，並可透過執行 Powershell Cmdlet **NetworkControllerLoadBalancerMux** 來查看。

MUX REST 資源部分的 \( 範例 \) ：

```
      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],
```

若要進行相互驗證，您也必須在 SLB MUX 裝置上擁有憑證。 當您部署使用 SCVMM 的軟體負載平衡器時，SCVMM 會自動設定此憑證。

>[!IMPORTANT]
>在主機和 SLB 節點上，「受信任的根憑證授權單位」憑證存放區不包含任何憑證，其中「發行至」與「發行者」不同。 如果發生這種情況，網路控制站與 southbound 裝置之間的通訊將會失敗。

網路控制站和 SLB MUX 憑證必須彼此信任 \( 。在網路控制器電腦的「受信任的根憑證授權單位」存放區中必須有該憑證的根憑證，反之亦然 \) 。 當您使用自我 \- 簽署憑證時，SCVMM 會確定本機電腦的 [受信任的根憑證授權單位] 存放區中有必要的憑證。
