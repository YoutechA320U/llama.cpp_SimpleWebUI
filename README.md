# llama.cpp_SimpleWebUI

![Picture](https://github.com/YoutechA320U/llama.cpp_SimpleWebUI/blob/master/image/image01.png "UIのイメージ") 
![Picture](https://github.com/YoutechA320U/llama.cpp_SimpleWebUI/blob/master/image/image02.png "マルチモーダルのイメージ") 

`llama.cpp` のサーバー機能など（OpenAI互換API）を利用して、ブラウザから簡単にLLMと対話できる軽量なWebUIです。
マルチモーダル対応（画像入力）および会話履歴の保存機能を備えており、シングルHTMLファイルで動作します。

##  特徴

- **マルチモーダル対応**: 画像ファイルを添付して、Visionモデル（gemma-4など）と対話可能です。
- **会話管理**:
    - 複数の会話スレッドを作成・保存（LocalStorageを利用）。
    - 会話名の編集、削除が可能。
    - 会話データのJSONエクスポートおよびインポート機能。
- **高度なUI/UX**:
    - **ストリーミング表示**: 回答がリアルタイムにタイピング形式で表示されます。
    - **Markdownレンダリング**: テーブルや数式、コードブロックを綺麗に表示。
    - **コードハイライト**: `highlight.js` によるシンタックスハイライトと、ワンクリックコピーボタンを搭載。
    - **レスポンシブデザイン**: PCだけでなく、スマートフォン等のモバイル端末でも快適に利用可能。
    - **長い回答の折りたたみ**: 長文の回答は自動的に折りたたまれ、「Show more」で展開できます。
- **編集・再生成**: 過去のユーザーメッセージを編集し、その時点から回答を再生成させることができます。
- **生成停止機能**: ストリーミング中に生成を強制停止させることが可能です。

##  使い方

### 1. llama.cpp サーバーの起動
このWebUIを利用するには、`llama.cpp` のサーバーを起動しておく必要があります。

```bash
# 例: サーバーを起動 (ポート8080)
./llama-server -m models/your-model.gguf --port 8080
```
※Visionモデルを使用する場合は、`--mmproj` オプションでマルチモーダルプロジェクターを指定してください。

### 2. WebUIの起動
1. 本リポジトリの `index.html` をダウンロードします。
2. ブラウザ（Chrome, Edge, Firefoxなど）で `index.html` を開きます。
3. 準備完了です！

##  設定のカスタマイズ

基本的にUIから設定項目が見えていませんが、`index.html` 内の `<script>` セクションにある定数を変更することで、挙動を調整できます。

- **APIエンドポイント**:
  - `API_URL`: `llama.cpp` サーバーのチャット補完エンドポイント（デフォルト: `http://localhost:8080/v1/chat/completions`）
  - `STOP_API_URL`: 生成停止用エンドポイント（デフォルト: `http://localhost:8080/v1/stop`）
- **生成パラメータ**:
  - `TEMPERATURE`: 生成の多様性（0.8）
  - `TOP_P`: Nucleus sampling（0.95）
- **システムプロンプト**:
  - `SYSTEM_ROLE_TEMPLATE.js`: AIの振る舞いを定義するテンプレート。

##　技術スタック

- **Frontend**: HTML5, CSS3, JavaScript (Vanilla JS)
- **Markdown Parser**: [marked.js](https://marked.js.org/)
- **Code Highlighting**: [highlight.js](https://highlightjs.org/)
- **Fonts**: Google Fonts (Noto Emoji)

##  ライセンス

このプロジェクトはMITライセンスの下で公開されています。

---

### その他Tips
- **画像リサイズ機能**: サーバーへの負荷と転送量を抑えるため、クライアント側で画像を最大400pxにリサイズして送信する処理を組み込んでいます。
- **UIブロッカー**: 生成中は誤操作を防ぐため、サイドバーや入力エリアに透明なレイヤー（UI Blocker）を被せてスクロール以外の操作を制限しています。