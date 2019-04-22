> [!Note] 
> 執行「受防護網狀架構」診斷工具 (Get-HgsTrace -RunDiagnostics) 時，可能會傳回不正確的狀態，宣告 HTTPS 設定中斷，事實上並不是中斷或者不使用。 無論 HGS 的證明模式為何，可能會傳回這個錯誤。 可能的根本原因如下所示：
>
> - HTTPS 確實未正確設定/中斷<br>
> - 您使用的是系統管理信任證明並且信任關係已損壞<br>
> &nbsp;&nbsp;&nbsp;&nbsp;-這完全是無論 HTTPS 設定是否正確、 不正確，或在不使用。<br>
>
> 請注意，診斷將只有在 Hyper-V 主機為目標時才傳回此不正確的狀態。 如果診斷的目標是「主機守護者服務」，傳回的狀態將會是正確的。

<!-- Appears in guarded-fabric-setting-up-the-host-guardian-service-hgs.md and guarded-fabric-troubleshoot-diagnostics.md
-->
