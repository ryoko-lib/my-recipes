# レシピ＆献立管理アプリ - 開発引き継ぎ

## プロジェクト概要
iPhoneメイン、iPadサブ、Macで編集する、レシピ＆週次献立管理アプリ（HTML1ファイル構成）。

## 現状のファイル
- `index.html`（1ファイル完結のSPA、約750行）
- 配信先：GitHub Pages（これからセットアップ予定）
- 公開URL例：`https://[username].github.io/my-recipes/`

## 技術スタック
- 素のHTML/CSS/JavaScript（フレームワーク・ビルドツール不使用）
- データ保存：ブラウザのWeb Storage API
  - **レシピ** → `localStorage`（キー: `r_v4`）永久保存
  - **献立** → `sessionStorage`（キー: `m_v5`）都度上書き、タブを閉じると消える
  - **最終バックアップ日時** → `localStorage`（キー: `last_backup`）
- ストレージにフォールバック実装あり（localStorage使用不可時はメモリ上で動作）

## 主要機能
1. **レシピタブ**：レシピのCRUD（追加・編集・削除・検索・フィルタ）
2. **献立タブ**：曜日×料理種別（主菜・汁物・副菜1・副菜2・デザート）の週次献立組み
3. **バックアップ機能**
   - 書き出し：`recipes.json` 固定ファイル名でダウンロード（iCloud Drive保存想定）
   - 読み込み：JSONファイル選択で復元
   - 最終バックアップ日時を表示、7日以上で赤字警告

## iPhone Safari対応済みの工夫
- viewport: `width=device-width, initial-scale=1.0, viewport-fit=cover`
- 入力欄のフォントサイズを16pxに統一（自動ズーム防止）
- FAB・トーストの位置を`env(safe-area-inset-bottom)`対応
- body の padding-bottom も safe-area 対応
- `DOMContentLoaded`待ちで初期化、エラー時は画面に表示

## 運用方針
- レシピ追加・編集は**iPhoneメインで実施**
- iPad・家族の端末は**閲覧専用**（同期は手動JSON読み込み）
- データ同期はGitHubではなく、**iCloud Driveの`recipes.json`手動エクスポート/インポート**で行う
- 将来家族共有が必要になったら、iCloud Driveに「家族レシピ」共有フォルダを作成し、「表示のみ」権限で共有する想定

## やりたいこと（今後の課題候補）
- GitHub Pagesへのデプロイ（Macで実施予定、未着手）
- ホーム画面アイコン用のapple-touch-icon追加
- 読み込み時のJSON最終更新日チェック（古いデータ警告）
- 将来的にはCloudflare Pages + D1への移行も視野（同期が必要になった段階で）

## 既知の仕様・制約
- レシピ追加ボタン（FAB）は**レシピタブでのみ表示**（献立タブでは非表示）
- localStorageは端末ごと・ブラウザごとに独立 → デバイス間のデータ同期はJSON手動運用が前提
- GitHub Pagesは公開リポジトリ必須だが、レシピデータ自体は端末のlocalStorageにあるため公開されない

## 引き継ぎ後にやってほしいこと
- やりたいことを順に進める手伝い