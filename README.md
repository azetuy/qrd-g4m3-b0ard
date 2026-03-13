# Quoridor（クオリドール）

2人用ボードゲーム「Quoridor」のWeb実装です。

## ゲーム概要

| 項目 | 内容 |
|------|------|
| ゲーム名 | Quoridor（クオリドール） |
| 種類 | 2人用ボードゲーム（パス争奪戦） |
| 技術 | 単一HTMLファイル（HTML/CSS/JS） |
| 公開URL | https://azetuy.github.io/qrd-g4m3-b0ard/ |

## ゲームルール

- プレイヤー1（青）：下方（行8）から上方（行0）へ移動が目標
- プレイヤー2（赤）：上方（行0）から下方（行8）へ移動が目標
- 各プレイヤー10枚の壁を使用可能
- 壁は相手プレイヤーの経路を塞げない配置のみ可能（パスチェック有）
- 壁配置後は自動的に「移動」モードに戻る仕様

## ファイル構成

| ファイル | 説明 |
|----------|------|
| `index.html` | ゲームの本体（HTML/CSS/JSすべて含む） |
| `.gitignore` | test_quoridor.js をignore対象 |
| `test_quoridor.js` | ロジックUnitTest（`node test_quoridor.js`で実行） |

##  주요コード要素

### stateオブジェクト

```javascript
state = {
  players: [
    { row: 8, col: 4 },  // プレイヤー1（青）の初期位置
    { row: 0, col: 4 },  // プレイヤー2（赤）の初期位置
  ],
  walls: [],              // 配置済み壁の配列 [{ir, ic, orient}, ...]
  wallCounts: [10, 10],   // 各プレイヤーの残り壁数
  turn: 0,                // 現在のターン（0=プレイヤー1、1=プレイヤー2）
};
```

### 主要ロジック関数

| 関数名 | 説明 |
|--------|------|
| `blocked(r1, c1, r2, c2, walls)` | 壁が(r1,c1)から(r2,c2)への移動をブロックするか判定 |
| `canPlace(ir, ic, orient)` | 交点(ir, ic)に方向orientの壁を置けるか判定（重複・交差・パスチェック） |
| `hasPath(pi, walls)` | プレイヤーpiが目標行まで到達可能な経路があるかBFSで判定 |
| `getValidMoves(pi)` | プレイヤーpiの現在の合法手リストを配列で返す |

### ボード座標系

- セル座標：(r, c) で r=0..8, c=0..8
- 交点座標：(ir, ic) で ir=0..7, ic=0..7（壁配置位置）
- DOMグリッド：17x17（セルとギャップが交互に配置）

### UI関数

| 関数名 | 説明 |
|--------|------|
| `render()` | ボード・駒・壁・ハイライトを再描画 |
| `setMode(m)` | モード切替（m='move'または'wall'） |
| `setOrientation(o)` | 壁の向き切替（o='h'または'v'） |
| `placeWall(ir, ic, orient)` | 壁を配置してターン進行 |
| `nextTurn()` | ターンを交代 |
| `undoLast()` | 1手戻す |

## 開発・テスト

### ユニットテスト実行

```bash
node test_quoridor.js
```

### デプロイ

- GitHub Pagesで自動公開
- mainブランチにpushすると自動的にデプロイされる
- 特別なビルドプロセスは不要（静的HTMLファイル）