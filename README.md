# jismesh for js <a href="https://www.buymeacoffee.com/leduong" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="32" /></a>

[![NPM version](https://img.shields.io/npm/v/jismesh.svg?style=flat)](https://www.npmjs.com/package/jismesh)
[![NPM monthly downloads](https://img.shields.io/npm/dm/jismesh.svg?style=flat)](https://npmjs.org/package/jismesh)
[![NPM total downloads](https://img.shields.io/npm/dt/jismesh.svg?style=flat)](https://npmjs.org/package/jismesh)
[![Build Status](https://travis-ci.com/leduong/jismesh.svg?branch=master)](https://travis-ci.com/leduong/jismesh)

Utilities for the Japanese regional grid system defined in Japanese Industrial Standards (JIS X 0410 地域メッシュ).

python の [jismesh パッケージ](https://pypi.org/project/jismesh/) を JavaScript に移植したものです。

## 対応地域メッシュコード

- 1 次(標準地域メッシュ 80km 四方): 1
- 40 倍(拡張地域メッシュ 40km 四方): 40000
- 20 倍(拡張地域メッシュ 20km 四方): 20000
- 16 倍(拡張地域メッシュ 16km 四方): 16000
- 2 次(標準地域メッシュ 10km 四方): 2
- 8 倍(拡張地域メッシュ 8km 四方): 8000
- 5 倍(拡張地域メッシュ 5km 四方): 5000
- 4 倍(拡張地域メッシュ 4km 四方): 4000
- 2.5 倍(拡張地域メッシュ 2.5km 四方): 2500
- 2 倍(拡張地域メッシュ 2km 四方): 2000
- 3 次(標準地域メッシュ 1km 四方): 3
- 4 次(分割地域メッシュ 500m 四方): 4
- 5 次(分割地域メッシュ 250m 四方): 5
- 6 次(分割地域メッシュ 125m 四方): 6

## インストール

```bash
npm install jismesh
```

または、CDN から読み込む場合

```html
<script src="//cdn.jsdelivr.net/npm/jismesh/dist/jismesh.min.js"></script>
```

グローバル変数 `jismesh` が定義されます。

## 使用例

### 緯度経度から地域メッシュコードを求める

メッシュコードに変換する世界測地系緯度経度と変換するメッシュコードの次数を指定します。

```javascript
const jismesh = require("jismesh");

// 緯度経度からメッシュコードを求める。
const meshCode = jismesh.toMeshCode(35.658581, 139.745433, 3);
console.log(meshCode); // => 53393599
```

### 地域メッシュコードから次数を求める

メッシュコードからそのメッシュコードの次数を判定します。

```javascript
const jismesh = require("jismesh");

const meshLevel = jismesh.toMeshLevel("53393599");
console.log(meshLevel); // => 3
```

### 地域メッシュコードから緯度経度を求める

求める緯度経度で表される点は、当該メッシュの基準点(南西端)から、
緯度座標上の点の位置(当該メッシュの単位経度の倍数)、経度座標上の点の位置(当該メッシュの単位緯度の倍数)
を指定します。

```javascript
const jismesh = require("jismesh");

// 南西端の緯度経度を求める。
const [latSW, lonSW] = jismesh.toMeshPoint("53393599", 0, 0);
console.log(latSW, lonSW); // => 35.65833333333333 139.7375

// 北東端の緯度経度を求める。
const [latNE, lonNE] = jismesh.toMeshPoint("53393599", 1, 1);
console.log(latNE, lonNE); // => 35.666666666666664 139.75

// 中心点の緯度経度を求める。
const [latC, lonC] = jismesh.toMeshPoint("53393599", 0.5, 0.5);
console.log(latC, lonC); // => 35.6625 139.74375

// 東隣接メッシュの中心点の緯度経度を求める。
const [latEastNeighborC, lonEastNeighborC] = jismesh.toMeshPoint(
  "53393599",
  0.5,
  1.5
);
console.log(latEastNeighborC, lonEastNeighborC); // => 35.6625 139.75625000000002
```
