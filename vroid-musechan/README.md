# vroid-musechan — Museちゃん 3Dモデル制作の参考資料

VRoid Studio でキャラクター「Museちゃん」の 3D モデルを作るための参考資料一式です。

- **作業する AI（Claude Desktop）へ**：まず [INSTRUCTIONS.md](INSTRUCTIONS.md) を読んでください。制作の指示、確定デザイン、不変条件、進め方がすべてそこにあります。
- MCP サーバー（VRoid Studio 2.16.0 版フォーク）：https://github.com/muse-chan/mcp-vroid-forWindows-216

## 中身

| パス | 内容 |
|---|---|
| `INSTRUCTIONS.md` | Claude Desktop 向け指示書（最初に読むもの） |
| `images/front-hoodie.webp` | 正面・標準衣装（パーカー）の採用絵。デザインの正 |
| `images/front-camisole.webp` | 正面・白キャミ姿の採用絵 |
| `images/front-china-dress.webp` | チャイナドレスの採用絵（将来の追加衣装用） |
| `images/face-closeup.png` | 確定デザインの顔アップ。ほくろ位置などの照合用 |
| `images/ref-hoodie-side.webp` / `images/ref-hoodie-back.webp` | パーカー姿の側面・背面の参考（生成図。既知のズレは指示書 §4 参照） |
| `images/view-front.webp` / `view-back.webp` / `view-left.webp` / `view-right.webp` / `view-top.webp` / `view-bottom.webp` | パーカー姿の六面図セット（正面・背面・左側面・右側面・真上・真下。2026-11-02 追加） |
| `images/ref-camisole-side.webp` / `images/ref-camisole-back.webp` | 白キャミ姿の側面・背面の参考（生成図） |
| `docs/musechan-3d-spec.md` | 設定資料（パラメーター、カラーパレット、不変条件、衣装仕様） |
| `docs/vroid-plan.md` | 全体計画（素体 1 つ＋衣装は XWear 差分の構造） |
| `docs/mcp-vroid-windows-checklist.md` | セットアップと環境確認のチェックリスト |

このフォルダは検証用の一時置き場リポジトリ（temp）にあります。秘密情報は置いていません（置かないでください）。
