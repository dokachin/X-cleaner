# 設計

## 修正箇所
_source.js のフィルタ判定ロジック1箇所

## 変更内容
deleteKeywords が設定されている場合、マッチしないポストは continue でスキップする。

### 変更前（誤り）
const isDeleteTarget = config.deleteKeywords.length > 0 && config.deleteKeywords.some(k => textContent.includes(k));
if (!isDeleteTarget) {
    // 保護フィルタチェック（マッチしなければそのまま削除へ） ← 誤り
}

### 変更後（正しい）
if (config.deleteKeywords.length > 0) {
    if (!config.deleteKeywords.some(k => textContent.includes(k))) { continue; } // マッチしない→スキップ
    // マッチした→保護フィルタをスキップして削除へ
} else {
    // 通常モード: 保護フィルタを評価
    if (config.ids.length > 0 && config.ids.includes(tweetId)) { continue; }
    if (config.keywords.some(k => textContent.includes(k))) { continue; }
    if (config.minLikes > 0 && currentLikes >= config.minLikes) { continue; }
}
