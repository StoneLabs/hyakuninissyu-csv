<div align="center">

# 百人一首 CSV

[🇬🇧 English](README.md) · 🇯🇵 **日本語**

小倉百人一首の100首をひとつのCSVファイルにまとめました。<br>
本文、読み、決まり字、競技かるたでの読み方、取り札に書かれている文字が入っています。

[![Validate CSV](https://github.com/StoneLabs/hyakuninissyu-csv/actions/workflows/validate.yml/badge.svg)](https://github.com/StoneLabs/hyakuninissyu-csv/actions/workflows/validate.yml)
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](LICENSE)
[![frictionless csv: supported](https://img.shields.io/badge/frictionless%20csv-supported-green.svg)](datapackage.json)

</div>

## 目次

- [概要](#概要)
- [はじめかた](#はじめかた)
- [列](#列)
- [使用例](#使用例)
- [補足](#補足)
- [出典](#出典)
- [チェック](#チェック)
- [貢献について](#貢献について)
- [ライセンス](#ライセンス)

## 概要

百人一首と競技かるたの情報をすべて収めた101行のCSVです。本文、作者の情報、メタデータ、決まり字、競技での読みなどが含まれますが、これらに限りません。序歌も含まれています。

## はじめかた

[`data.csv`](data.csv) をダウンロードします（またはこのリポジトリをクローンします）。Pythonでの簡単な例です。

```python
import csv

with open("data.csv", encoding="utf-8", newline="") as f:
    poems = {row["number"]: row for row in csv.DictReader(f)}

poem = poems["60"]
print(poem["author_kanji"])
print(" ".join(poem[f"verse_{i}_kanji"] for i in range(1, 6)))
print(" ".join(poem[f"verse_{i}_hiragana_yomi"] for i in range(1, 6)))
```

```text
小式部内侍
大江山 いく野の道の 遠ければ まだふみも見ず 天の橋立
おおえやま いくののみちの とおければ まだふみもみず あまのはしだて
```

GitHubから直接読み込むこともできます。

```python
import csv
import io
import urllib.request

URL = "https://raw.githubusercontent.com/StoneLabs/hyakuninissyu-csv/master/data.csv"

with urllib.request.urlopen(URL) as response:
    poems = list(csv.DictReader(io.TextIOWrapper(response, encoding="utf-8")))

print(len(poems), "rows")
```

## 列

例は第60首（大江山）です。型などを含めた同じ一覧が [`datapackage.json`](datapackage.json) にあります。

### 歌と作者

|列|説明|例|
|-|-|-|
|`number`|百人一首の歌番号（1〜100）。追加の1行 `序歌` は、競技かるたの試合の始めに読まれる歌です。100首には含まれず札もないため、色・決まり字・取り札の列は空です。|`60`|
|`color`|五色百人一首での色。桃・青・黄・緑・橙のいずれかです。|`黄`|
|`color_num`|その色の中での番号（1〜20）。|`12`|
|`author`|読みつきの作者名。`[名前\|読み]` の形です。|`[小式部内侍\|こしきぶのないし]`|
|`author_kanji`|ふつうの表記の作者名。|`小式部内侍`|
|`author_hiragana`|作者名の読み（ひらがな）。|`こしきぶのないし`|

### 決まり字

|列|説明|例|
|-|-|-|
|`kimariji_kami`|決まり字。上の句（第1〜3句）の始まりのうち、ほかのどの歌とも区別できる最も短い部分です。歴史的仮名遣い。|`おほえ`|
|`kimariji_kami_yomi`|同じ決まり字を、今の発音どおりに書いたもの。|`おおえ`|
|`kimariji_shimo`|下の句（第4〜5句）の始まりのうち、ほかのどの歌とも区別できる最も短い部分。濁点（が の ゛）あり。|`まだ`|
|`kimariji_shimo_no_tenten`|`kimariji_shimo` から濁点を除いたもの。取り札の書き方と同じです。|`また`|

### 5つの句

|列|説明|例|
|-|-|-|
|`verse_1`|第1句。漢字に `[漢字\|読み]` の形で読みをつけたもの。読みは歴史的仮名遣いです。|`[大江山\|おほえやま]`|
|`verse_2`|第2句。`verse_1` と同じ形です。|`いく[野\|の]の[道\|みち]の`|
|`verse_3`|第3句。`verse_1` と同じ形です。|`[遠\|とほ]ければ`|
|`verse_4`|第4句。`verse_1` と同じ形です。|`まだふみも[見\|み]ず`|
|`verse_5`|第5句。`verse_1` と同じ形です。|`[天\|あま]の[橋立\|はしだて]`|
|`verse_1_kanji`|第1句をふつうの表記（漢字かな交じり）で書いたもの。|`大江山`|
|`verse_2_kanji`|第2句。`verse_1_kanji` と同じ形です。|`いく野の道の`|
|`verse_3_kanji`|第3句。`verse_1_kanji` と同じ形です。|`遠ければ`|
|`verse_4_kanji`|第4句。`verse_1_kanji` と同じ形です。|`まだふみも見ず`|
|`verse_5_kanji`|第5句。`verse_1_kanji` と同じ形です。|`天の橋立`|
|`verse_1_hiragana`|第1句をすべてひらがなで、歴史的仮名遣いのまま書いたもの（発音どおりではなく、書かれたとおり）。|`おほえやま`|
|`verse_2_hiragana`|第2句。`verse_1_hiragana` と同じ形です。|`いくののみちの`|
|`verse_3_hiragana`|第3句。`verse_1_hiragana` と同じ形です。|`とほければ`|
|`verse_4_hiragana`|第4句。`verse_1_hiragana` と同じ形です。|`まだふみもみず`|
|`verse_5_hiragana`|第5句。`verse_1_hiragana` と同じ形です。|`あまのはしだて`|
|`verse_1_hiragana_yomi`|第1句をすべてひらがなで、実際の発音どおりに書いたもの（例：らむ → らん、助詞の は → わ）。|`おおえやま`|
|`verse_2_hiragana_yomi`|第2句。`verse_1_hiragana_yomi` と同じ形です。|`いくののみちの`|
|`verse_3_hiragana_yomi`|第3句。`verse_1_hiragana_yomi` と同じ形です。|`とおければ`|
|`verse_4_hiragana_yomi`|第4句。`verse_1_hiragana_yomi` と同じ形です。|`まだふみもみず`|
|`verse_5_hiragana_yomi`|第5句。`verse_1_hiragana_yomi` と同じ形です。|`あまのはしだて`|

### 競技での読み

|列|説明|例|
|-|-|-|
|`kyougi_yomi_kami`|競技かるたで読み上げるときの上の句。`[漢字\|読み]` の形で読みつき。`ー` は長く伸ばす、`ｰ` は短く伸ばす印です。全日本かるた協会の読手テキストに基づきます。|`[大江山\|おおえやま]ーいく[野\|の]の[道\|みち]のｰ[遠\|とお]けれーばー`|
|`kyougi_yomi_shimo`|`kyougi_yomi_kami` と同じ形の下の句。|`まだｰふみも[見\|み]ずー[天\|あま]の[橋\|はし]ー[立\|だて]`|
|`kyougi_yomi_kami_kanji`|競技かるたで読み上げるときの上の句。漢字かな交じりで、`ー`／`ｰ` の伸ばしの印つき。|`大江山ーいく野の道のｰ遠けれーばー`|
|`kyougi_yomi_shimo_kanji`|`kyougi_yomi_kami_kanji` と同じ形の下の句。|`まだｰふみも見ずー天の橋ー立`|
|`kyougi_yomi_kami_hiragana`|競技かるたで読み上げるときの上の句。すべてひらがなで発音どおり、`ー`／`ｰ` の伸ばしの印つき。|`おおえやまーいくののみちのｰとおけれーばー`|
|`kyougi_yomi_shimo_hiragana`|`kyougi_yomi_kami_hiragana` と同じ形の下の句。|`まだｰふみもみずーあまのはしーだて`|

### 取り札

|列|説明|例|
|-|-|-|
|`torifuda_1`|取り札の1行目。取り札には下の句が歴史的仮名遣い・濁点なしで書かれています。5文字、5文字、残りの順です。|`またふみも`|
|`torifuda_2`|取り札の2行目。|`みすあまの`|
|`torifuda_3`|取り札の3行目。残りの文字です。|`はしたて`|

## 使用例

### 一字決まりの札を探す

最初の一音を聞くだけで取れる7枚の札です（有名な「むすめふさほせ」）。

```python
import csv

with open("data.csv", encoding="utf-8", newline="") as f:
    poems = list(csv.DictReader(f))

for poem in poems:
    if len(poem["kimariji_kami_yomi"]) == 1:
        print(poem["number"], poem["kimariji_kami_yomi"], poem["verse_1_kanji"])
```

```text
18 す 住の江の
22 ふ 吹くからに
57 め 巡り逢ひて
70 さ 寂しさに
77 せ 瀬を早み
81 ほ ほととぎす
87 む 村雨の
```

### ウェブページで漢字の上に読みを表示する

ルビの列は `[漢字|読み]` の形です。1行でHTMLの `<ruby>` タグに変換でき、どのブラウザでも表示できます。

```python
import csv
import re

with open("data.csv", encoding="utf-8", newline="") as f:
    poems = {row["number"]: row for row in csv.DictReader(f)}

def to_html(text):
    return re.sub(r"\[([^|\]]+)\|([^\]]+)\]", r"<ruby>\1<rt>\2</rt></ruby>", text)

print(to_html(poems["60"]["verse_1"]))
print(to_html(poems["60"]["verse_3"]))
```

```text
<ruby>大江山<rt>おほえやま</rt></ruby>
<ruby>遠<rt>とほ</rt></ruby>ければ
```

### 取り札を表示する

```python
import csv

with open("data.csv", encoding="utf-8", newline="") as f:
    poems = {row["number"]: row for row in csv.DictReader(f)}

def print_card(poem):
    # 取り札は上から下へ、右から左へ読みます。
    lines = [poem["torifuda_1"], poem["torifuda_2"], poem["torifuda_3"]]
    for i in range(max(len(line) for line in lines)):
        print(" ".join(line[i] if i < len(line) else "　" for line in reversed(lines)))

print_card(poems["60"])
```

```text
は み ま
し す た
た あ ふ
て ま み
　 の も
```

### pandasを使う

決まり字が1音、2音、3音…の歌がそれぞれ何首あるか：

```python
import pandas as pd

df = pd.read_csv("data.csv", dtype=str, keep_default_na=False)
df = df[df["number"] != "序歌"]

print(df["kimariji_kami_yomi"].str.len().value_counts().sort_index())
```

```text
kimariji_kami_yomi
1     7
2    42
3    37
4     6
5     2
6     6
Name: count, dtype: int64
```

### Frictionlessを使う

[`datapackage.json`](datapackage.json) には各列の型が書かれているので、[Frictionless](https://frictionlessdata.io/) で読み込むと数値は数値として、空のセルは `None` として読み込まれます（`pip install frictionless`）。

```python
from frictionless import Package

rows = Package("datapackage.json").get_resource("data").read_rows()
poems = {row["number"]: row for row in rows}

poem = poems["60"]
print(poem["color"], poem["color_num"], type(poem["color_num"]).__name__)  # color_num はすでに数値です
print(poems["序歌"]["color"])  # 空のセルは None になります
```

```text
黄 12 int
None
```

## 補足

- **形式：** 語尾のない列（`author`、`verse_1` 〜 `verse_5`、`kyougi_yomi_kami`、`kyougi_yomi_shimo`）は `[漢字|読み]` の形です。たとえば `[大江山|おほえやま]`。漢字だけを残すと `_kanji` の列に、読みだけを残すと `_hiragana` の列になります。
- **競技での読みは、歌と少し違うことがあります：** 第74首は第3句の最後に「よ」がありますが、競技では読みません。序歌の第4句は歌では「今は春べと」ですが、「今を春べと」と読みます。
- `kyougi_` の列の伸ばしの印は、長さが違います。詳しくは[競技かるた読手テキスト](https://www.karuta.or.jp/karuta/reading/)を見てください。
- **空のセル**は序歌の行にだけあります。序歌には札がないので、色・決まり字・取り札の列が空です。
- **ファイル形式：** BOMなしのUTF-8、セルはカンマ区切り、1行に1首です。カンマを含むセルはないので、引用符はありません。Excelでは「データ → テキストまたはCSVから」で開いてUTF-8を選んでください。そうしないと日本語が文字化けすることがあります。

## 出典

| 出典 | 使った部分 |
|-|-|
| [全日本かるた協会『競技かるた読手テキスト』改訂版（2025年4月1日）](https://www.karuta.or.jp/karuta/reading/) | 競技での読み、伸ばしの印、`_yomi` の読み |
| [近江神宮「小倉百人一首一覧」](https://oumijingu.org/pages/130/) | 本文・決まり字・作者の確認 |
| [小倉山荘「ちょっと差がつく『百人一首講座』」](https://ogurasansou.jp.net/columns_category/hyakunin/) | 本文・作者の確認 |
| [かるたらいふ「取り札一覧（決まり字版）」](https://karutalife.sakura.ne.jp/education/003/) | 取り札の文字の確認 |
| [Wikipedia「難波津 (和歌)」](https://ja.wikipedia.org/wiki/%E9%9B%A3%E6%B3%A2%E6%B4%A5_(%E5%92%8C%E6%AD%8C)) | 序歌の本文と作者 |

出典の著作権はそれぞれの持ち主にあります。出典どうしで違うところは、次のようにしています。

- **第89首・第92首：** 取り札一覧では「よはり」「かはく」ですが、データでは一般的な歴史的仮名遣いの「よわり」「かわく」にしています。

## チェック

プッシュやプルリクエストのたびに、次のチェックが動きます（[`.github/validators`](.github/validators)）。

| チェック | 確認すること |
|-|-|
| `rows.py` | すべての行にすべての列があり、空のセルがないこと（序歌の札の列を除く）。 |
| `numbers.py` | 歌番号1〜100と序歌がそれぞれ1回ずつあり、色の番号1〜20がそれぞれ5回ずつあること。 |
| `ruby.py` | ルビの列から、対応する `_kanji` と `_hiragana` の列が得られること。 |
| `datapackage.py` | `datapackage.json` と両方のREADMEが、`data.csv` と同じ列を同じ例で載せていること。 |
| Frictionless | すべてのセルが `datapackage.json` の型とルールに合っていること。 |

[act](https://github.com/nektos/act) を使うと、自分で実行できます（Dockerが必要です）。

```bash
act -j validate
```

## 貢献について

間違いを見つけたり、アイデアがあったりしたら、ぜひ教えてください。

1. **まず [issue](https://github.com/StoneLabs/hyakuninissyu-csv/issues) を立ててください。** たいていすぐに返事をします。
2. **変更に意味があれば、その issue に [`PR Welcome`](https://github.com/StoneLabs/hyakuninissyu-csv/labels/PR%20Welcome) ラベルをつけます。**
3. **それから、その issue に対するプルリクエストを送ってください。** `PR Welcome` ラベルのついた issue がないプルリクエストは受け付けていません。

データを直すときは、どの出典を使ったかを書き、プッシュする前に[チェック](#チェック)を実行してください。コミットメッセージは `data:`、`doc:`、`validator:` のどれかで始めます。

## ライセンス

[The Unlicense](LICENSE)
