---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: "設定電腦，以取得疑難排解"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>設定電腦，以取得疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>在您使用之前進階疑難排解技術來找出並修正 Active Directory，設定您的電腦進行疑難排解。 您也應該會有的基本知識<token>nextref_longhorincludes > 疑難排解概念、程序與工具。 </para>
    <para>Windows Server 2008 監視工具的相關資訊，會看到 Step-by-Step 指南效能和可靠性監視在 Windows Server 2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>設定工作，以取得疑難排解</title>
    <content>
      <para>來設定您的電腦，以取得疑難排解 Active Directory Domain Services (AD DS)，執行下列工作：</para>
      <para>
        <link xlink:href="#BKMK_2">安裝遠端伺服器管理工具 AD ds</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">設定穩定性和效能監視器</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">設定等級登入</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>安裝適用於 AD DS 遠端伺服器管理工具</title>
        <content>
          <para>當您安裝建立網域控制站 AD DS 時，您用來管理 AD DS 系統管理工具會自動安裝。 如果您想要從電腦不是網域控制站的遠端管理網域控制站，您可以執行的成員伺服器上安裝系統管理工具] <token>nextref_longhorincludes > 或的電腦執行 Windows Vista 含 Service Pack 1 (SP1)。 會員伺服器上執行的<token>nextref_longhorincludes >、您使用遠端伺服器管理工具 (RSAT) 的功能在伺服器管理員安裝 Active Directory Domain Services 工具。 RSAT 來取代 Windows 在 Windows Server 2003 的支援工具。 您也可以下載工具，該電腦執行 Windows Vista 含 Service Pack 1 (SP1) 的電腦上安裝 Active Directory Domain 服務工具。</para>
          <para>如安裝 RSAT 相關資訊，<link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">安裝遠端伺服器管理工具 AD ds</link>。</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title>穩定性與效能監視器設定</title>
        <content>
          <para>Windows Server 2008 包含 Windows 的可靠性與效能監視器，也就是 Microsoft Management Console (MMC) 嵌入式管理單元，結合了先前的獨立工具，包括效能登和通知，伺服器效能建議，以及系統監視器的功能。這個嵌入式管理單元資料收集設定和事件追蹤工作階段的自訂提供圖形使用者介面 (GUI)。</para>
          <para>穩定性和效能監視器也包含可靠性監視器，MMC 嵌入式管理單元系統曲目變更，並將它變更比較系統穩定性提供圖形檢視其關係。</para>
        </content>
      </section>
      <section address="BKMK_4">
        <title>設定等級登入</title>
        <content>
          <para>如果您收到事件檢視器中 Directory 服務木頭中的資訊以取得疑難排解不足，引發的登入層級使用中的適當登錄項目<embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>。</para>
          <para>根據預設，登入的所有項目層級設定為<embeddedLabel>0</embeddedLabel>，資訊的最少提供。登入最高的是<embeddedLabel>5</embeddedLabel>。增加層級的項目，導致其他事件登入 Directory 服務事件登入。</para>
          <para>您可以使用下列程序，若要變更診斷的項目的登入層級。</para>
          <alert class="caution">
            <para>我們建議您不要不直接編輯登錄除非另有其他的替代方案。變更登錄無法驗證再套用，如此一來，不正確的值可以儲存或 windows 作業系統。這可能導致處於無法復原錯誤，系統中。可能的話，請使用群組原則」或其他 Windows 工具，例如 MMC 嵌入式管理單元，以完成任務，而非編輯登錄直接。如果您必須編輯登錄，小心謹慎。</para>
          </alert>
          <para>
            <embeddedLabel>需求</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>成員資格<embeddedLabel>網域系統管理員</embeddedLabel>，或相當於，才能完成此程序最小值。 <token>review_detailincludes</para>
            </listItem>
            <listItem>
              <para>工具：Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>要變更診斷的項目登入層級</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>按一下<ui>開始</ui>，按一下 [<ui>執行</ui>，類型<userInput>regedit</userInput>，，然後按一下 [ <ui>[確定]</ui>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>瀏覽至您想要設定 [登入的項目<embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>按兩下的項目，並在<embeddedLabel>基底</embeddedLabel>，按一下 [<embeddedLabel>小數點</embeddedLabel>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>中<embeddedLabel>值</embeddedLabel>，輸入整數<embeddedLabel>0</embeddedLabel>透過<embeddedLabel>5</embeddedLabel>，然後按一下 [ <ui>[確定]</ui>。</para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


