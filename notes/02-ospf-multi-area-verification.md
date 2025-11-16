OSPF Multi-Area 学習記録



日付

2025-11-16-



トラブルシューティング記録



問題: R1のLo0がOSPFに広告されていない



症状:

・Area 0とArea 2のType 3 LSAに1.1.1.1が存在しない

・R2のOSPF Databaseを確認しても、1.1.1.1のSummary LSAが無い



原因:

・interface Loopback0にip ospf 1 area 1コマンドが設定されていなかった

・Router IDとして使用されているだけで、OSPFには広告されていない状態



解決方法:

R1(config)interface Loopback0

R1(config-if)ip ospf 1 area 1

R1(config-if)end

R1write memory





結果:

・Area 0のType 3 LSAに1.1.1.1が追加された（ADV Router: R2）

・Area 2のType 3 LSAに1.1.1.1が追加された（ADV Router: R3）

・R4から1.1.1.1へのpingが成功



学んだこと:

・Router IDとOSPF広告は別の設定

・router ospf 1 の router-id 1.1.1.1 だけではネットワークは広告されない

・OSPFに広告するには ip ospf <process> area <area-id> または network コマンドが必要



---



OSPFでループバックアドレスを使う理由



安定性

・物理インターフェースがダウンしてもRouter IDは維持される

・OSPFプロセスの再起動を防ぐ

・Neighborリセットを防止



管理性

・ルーターの「代表アドレス」として機能

・SSH/Telnetでの管理アクセスに最適

・物理IF障害時も他経路からアクセス可能

・トポロジー変更でもIPアドレス不変



テスト・検証

・エンドツーエンドの到達性確認がシンプル

・ルーター全体の健全性を1つのIPで確認

・物理IFのIPだと特定インターフェースのみのテストになる

・Loopbackならルーター全体の動作確認が可能



識別性

・Router IDが固定され、変わらない

・LSAの追跡が容易

・監視システムでの一貫した管理対象



実務での使用

・BGPなど他プロトコルでもupdate-sourceとして使用

・ベストプラクティスとして広く採用

