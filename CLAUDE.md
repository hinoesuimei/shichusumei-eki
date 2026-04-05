# 八卦おみくじアプリ

## 基本原則
> 「シンプルさは究極の洗練である」

- **最小性**: 不要なコードは一文字も残さない。必要最小限を超えない
- **単一性**: 真実の源は常に一つ（要件: requirements.md、進捗: SCOPE_PROGRESS.md）
- **実証性**: 推測しない。ログ・APIレスポンスで事実を確認する
- **潔癖性**: エラーは隠さない。フォールバックで問題を隠蔽しない

## プロジェクト設定

```yaml
実装方式: HTMLシングルファイル（index.html）
外部ライブラリ: なし
フレームワーク: なし
外部API: なし（完全オフライン動作）
デプロイ: GitHub Pages
```

## 既存ソフトからの流用

計算ロジックは `/home/hinoe/四柱推命命式/am.html` から流用する：
- `calcJDN(y, m, d)` — ユリウス日数計算
- `calcNicchuu(year, month, day)` — 日柱計算（日干取得）

流用時は該当関数をそのままコピーし、不要な依存を除去すること。

## データ

- 十干: 10種（甲乙丙丁戊己庚辛壬癸）
- 八卦: 8種（乾坤震巽坎離艮兌）
- 統合メッセージ: 80通り（十干×八卦）
- 保存: localStorageキー `hinoe_omikuji_birthday`

## コード品質

- 関数: 100行以下 / ファイル: 700行以下 / 行長: 120文字

## デザイン

既存の命式ソフトと統一感を持たせる：
```css
--cream: #FFF8F0;
--orange: #E8732A;
--gold: #C8972A;
--dark-orange: #B85A18;
--light-gold: #F0D080;
--text: #3A2A10;
--muted: #8A6A3A;
--border: #D4A85A;
```
フォント: 'M PLUS Rounded 1c'（Google Fonts）

## エラー対応

- 同じエラー3回 → Web検索で最新情報を収集
- デプロイはユーザーの明示的な承認を得てから実行する

## Playwright

スクリーンショット保存先: /tmp/bluelamp-screenshots/

## ドキュメント管理

許可されたドキュメントのみ作成可能:
- docs/SCOPE_PROGRESS.md（実装計画・進捗）
- docs/requirements.md（要件定義）
- docs/DEPLOYMENT.md（デプロイ情報）
- docs/e2e-specs/（E2Eテスト仕様書）
上記以外のドキュメント作成はユーザー許諾が必要。

## CI/CD設定

### GitHub Actions（プッシュ・PR時に自動実行）
| チェック | 対象 | 内容 |
|---------|------|------|
| HTML存在確認 | index.html | ファイル存在チェック |

### ブランチ戦略
- `main`: 本番環境
- `develop`: 開発統合ブランチ
- `feature/*`: 機能開発ブランチ

### リポジトリ
- URL: https://github.com/hinoesuimei/shichusumei-eki
- 公開設定: Private
