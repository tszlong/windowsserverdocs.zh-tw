## <a name="standard-configuration-considerations"></a>標準設定考慮

Always On VPN 有許多設定選項。 不過，您可以選擇您的 VPN 設定，但請包含下列資訊：

-   **連線類型。** 連接通訊協定的選擇很重要，而且最終會與您要使用的驗證類型密切配合。 如需可用通道通訊協定的詳細資訊，請參閱[VPN 連線類型](/windows/security/identity-protection/vpn/vpn-connection-type/)。

-   **傳入.** 在此內容中，路由規則會決定使用者是否可以在連線到 VPN 時使用其他網路路由。

    -   _分割通道_可讓您同時存取其他網路，例如網際網路。

    -   _強制通道_要求所有流量都是透過 VPN 獨佔傳送，且不允許同時存取其他網路。

-   **各個.** [_觸發_] 會決定 VPN 連線起始的方式和時間（例如，當應用程式開啟時，由使用者手動啟動裝置）。 如需觸發選項，請參閱[VPN 自動觸發的設定檔選項](/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/)。

-   **裝置或使用者驗證。** Always On VPN 會透過稱為「[裝置](../vpn/vpn-device-tunnel-config.md)通道」的功能，使用裝置憑證和裝置起始的連接。 該連線可自動起始且持續性，類似于 DirectAccess 基礎結構通道連線。

>[!TIP]
>從 DirectAccess 遷移至 Always On VPN 時，請考慮從可與您擁有的設定選項開始，然後從該處展開。

藉由使用使用者憑證，Always On VPN 用戶端會自動連線，但會在使用者層級（在使用者登入之後），而不是在裝置層級（在使用者登入之前）執行。 使用者仍然可以順暢地體驗，但它支援更先進的驗證機制，例如 Windows Hello 企業版。
