# 八卦おみくじ E2Eテスト仕様

対象URL: `file:///home/hinoe/四柱推命×易簡易アプリ/index.html`

## ページ概要

| 画面ID | 画面名 | 表示条件 |
|--------|--------|---------|
| screen-input | 生年月日入力画面 | 初回起動・クリア後 |
| screen-cast | 筮竹アニメーション画面 | 生年月日確定後 |
| screen-result | 結果表示画面 | 念じてクリック後（3.2秒） |

## テストケース

### E2E-OMI-001: 初回起動 - 生年月日入力画面の表示

**前提条件**: localStorageに `hinoe_omikuji_birthday` が存在しない

**操作**: index.htmlを開く

**期待結果**:
- `#screen-input` が表示されている（`hidden`属性なし）
- `#screen-cast` が非表示
- `#screen-result` が非表示
- `#sel-year` `#sel-month` `#sel-day` セレクトボックスが存在する
- `#btn-decide` ボタンが存在する

---

### E2E-OMI-002: 生年月日入力 → 筮竹画面への遷移

**前提条件**: screen-inputが表示されている

**操作**:
1. `#sel-year` で「1990」を選択
2. `#sel-month` で「5」を選択
3. `#sel-day` で「15」を選択
4. `#btn-decide` をクリック

**期待結果**:
- `#screen-cast` が表示される
- `#screen-input` が非表示になる
- `#cast-kan-label` に十干名（甲〜癸のいずれか）と読み・タイプが表示される（例: 「あなたは 甲（きのえ）剛直・成長型」）

---

### E2E-OMI-003: 「念じてクリック」→ 結果画面への遷移

**前提条件**: screen-castが表示されている

**操作**: `#btn-cast` をクリック → 3.2秒待機

**期待結果**:
- 3.2秒後に `#screen-result` が表示される
- `#res-kan-icon` に十干アイコン（🌱🌿🔥🕯️🏔️🌾⚔️💎🌊💧のいずれか）が表示される
- `#res-hak-icon` に八卦アイコン（⚡🌏⛈️🌀💧🔥🗻😊のいずれか）が表示される
- `#res-message` にメッセージテキストが表示される（空でない）

---

### E2E-OMI-004: 「もう一度引く」→ 筮竹画面に戻る

**前提条件**: screen-resultが表示されている

**操作**: `#btn-again` をクリック

**期待結果**:
- `#screen-cast` が表示される
- `#screen-result` が非表示になる

---

### E2E-OMI-005: 「生年月日をクリア」→ 入力画面に戻りlocalStorage削除

**前提条件**: screen-resultが表示されている（生年月日がlocalStorageに保存済み）

**操作**: `#btn-clear` をクリック

**期待結果**:
- `#screen-input` が表示される
- `#screen-result` が非表示になる
- localStorage に `hinoe_omikuji_birthday` が存在しない

---

### E2E-OMI-006: localStorage保存 → リロード後に筮竹画面が表示される

**前提条件**: localStorage に有効な生年月日（1990年5月15日）が保存されている

**操作**: ページをリロード（再ナビゲーション）

**期待結果**:
- `#screen-cast` が表示される（入力画面をスキップ）
- `#screen-input` が非表示になる
- `#cast-kan-label` に十干タイプが表示される

---

### E2E-OMI-007: 不正な日付入力 → アラートが出て画面遷移しない

**前提条件**: screen-inputが表示されている

**操作**:
1. `#sel-year` で「2024」を選択
2. `#sel-month` で「2」を選択
3. `#sel-day` で「30」を選択（2月30日 = 不正日付）
4. `#btn-decide` をクリック

**期待結果**:
- アラートダイアログが表示される（「正しい日付を入力してください」）
- `#screen-input` のままである（画面遷移しない）
