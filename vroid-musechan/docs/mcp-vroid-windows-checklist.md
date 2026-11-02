# mcp-vroid Windowsフォーク セットアップチェックリスト

対象: [yub4-arema/mcp-vroid-forWindows](https://github.com/yub4-arema/mcp-vroid-forWindows)
（本家 nhodges/mcp-vroid は Linux/Wayland 専用。このフォークが Windows 10/11 対応を足したもの）

## 先輩のPCで確認したいこと

- [ ] Windows 10 / 11 のどちらか
- [x] VRoid Studio が入っているか → **公式サイト版 2.16.0**（2026-11-02 先輩から確認）
  - Steam 版でなくても `MCP_VROID_EXE` にインストール先の `VRoidStudio.exe` のフルパスを指定すれば起動できる（フォークの CHANGELOG で非 Steam 版向けの仕組みとして明記。BOOTH 版が例示されているが、公式サイト版も同じ単体 exe 形式）
  - バージョン差について: フォークの校正基準は 2.14.0。2.14.0 → 2.16.0 の公式変更は、ヘアー形状の保存（髪型編集内）、設定への「パフォーマンス表示」追加、消しゴムの不具合修正が中心で、タブやパラメータパネルのレイアウト変更はなし。アンカーがそのまま使える見込みは高いが、最終確認はスモークテスト＋使い捨てモデルでの試走が必要
  - 「パフォーマンス表示」はオフのままにしておく（オンだと編集画面左下に FPS 等が重なって写り、認識の邪魔になり得る）
- [ ] ディスプレイ解像度と表示倍率（校正基準は 2560×1440。作者の動作報告は 1920×1080 / 100%。倍率は 100% が無難）

## 導入手順

1. Python 3.11+ と [uv](https://docs.astral.sh/uv/) をインストール
2. Tesseract OCR（英語 `eng` データ入り）をインストール
   - 例: `C:\Program Files\Tesseract-OCR\tesseract.exe`
3. リポジトリを取得
   ```
   git clone https://github.com/yub4-arema/mcp-vroid-forWindows
   cd mcp-vroid-forWindows
   uv sync --python 3.11
   ```
4. スモークテスト
   ```
   uv run python scripts/smoke_test.py
   ```
5. **VRoid Studio の UI 言語を英語に切り替える（必須）**
   - 日本語 Windows では初期状態が日本語 UI で、OCR のアンカー（`Export`、`Parameters`、タブ名など）が一致しない
   - 手順: `⋮` → Settings → Language → English（一度だけ）
6. MCP クライアントに登録（mcpServers 形式の例）
   ```json
   {
     "mcpServers": {
       "vroid": {
         "command": "uv",
         "args": ["run", "--directory", "C:\\path\\to\\mcp-vroid-forWindows", "mcp-vroid"],
         "env": {
           "MCP_VROID_TESSERACT": "C:\\Program Files\\Tesseract-OCR\\tesseract.exe",
           "MCP_VROID_EXE": "C:\\path\\to\\VRoidStudio.exe"
         }
       }
     }
   }
   ```
7. `vroid_status` で健全性を確認 → `vroid_launch` で起動

## 動作実績と未検証の範囲（2026-11-02 時点）

- 作者が上流へ報告した Windows での確認済みフロー:
  Body タブのパラメータ設定（`set_param`）→ 読み戻し → VRM 書き出し（VRM 1.0 メタデータ確認）。
  1920×1080 では名前を付けて保存（`save_project_as`）と `restart=true` も確認済み
- ただしフォーク自身の README の「Verified environments」表では Windows 行はまだ `not yet verified` 表記。
  全項目の正式な受け入れ確認は `scripts/windows_acceptance.py` を回して行う想定
- 未検証: 髪型エディタ、`drag_slider`、100% 以外の表示倍率、日本語 UI
- アンカー座標は解像度依存。書き出しフロー以外は 2560×1440 基準の校正のままなので、
  環境によっては再計測が要る可能性がある

## 運用上の注意

- 実行中は **実デスクトップを直接操作する**（仮想デスクトップやサンドボックスなし）。
  フロー実行中は PC で他の作業をしないこと。付き添い前提
- 入力は VRoid が最前面にある間だけ送られる。他のアプリがフォーカスを奪うと入力は拒否される
- Windows のフォアグラウンド規則で自動で前面化できないことがある。その場合は手動で VRoid を前面に出して再試行
- スクリーンショットはディスクに平文で保存される（保存先フォルダは機微情報として扱う）
- `vroid_launch(restart=true)` は未保存の作業を失う

## 分担の前提

- MCP サーバーと MCP クライアントは **先輩の PC 上で動く** 構成になる
  （Museちゃんの VM から先輩の PC 上のサーバーを直接叩くことはできない）
- Museちゃん側の担当: パラメータ表・テクスチャ素材（ほくろ入りの顔テクスチャ等）の用意、
  手順の案内、結果のスクリーンショット確認
