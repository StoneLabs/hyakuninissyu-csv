<div align="center">

# Hyakunin Isshu CSV

🇬🇧 **English** · [🇯🇵 日本語](README.ja.md)

All 100 poems of the Ogura Hyakunin Isshu (小倉百人一首) in one CSV file:<br>
the text, the readings, the kimariji, how each poem is read aloud in competitive karuta, and what is printed on the cards.

[![Validate CSV](https://github.com/StoneLabs/hyakuninissyu-csv/actions/workflows/validate.yml/badge.svg)](https://github.com/StoneLabs/hyakuninissyu-csv/actions/workflows/validate.yml)
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](LICENSE)

</div>

## Contents

- [Overview](#overview)
- [Quick start](#quick-start)
- [Columns](#columns)
- [Examples](#examples)
- [Notes](#notes)
- [Data sources](#data-sources)
- [Checks](#checks)
- [Contributing](#contributing)
- [License](#license)

## Overview

101 line CSV with all the information on 百人一首 and competitive karuta. Including but not limited to: poem texts, author information, metadata, kimariji, competitive readings, etc. Joka (序歌) is included as well.

## Quick start

Download [`data.csv`](data.csv) (or clone this repository). Here is a quick example in Python:

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

You can also read the file straight from GitHub:

```python
import csv
import io
import urllib.request

URL = "https://raw.githubusercontent.com/StoneLabs/hyakuninissyu-csv/master/data.csv"

with urllib.request.urlopen(URL) as response:
    poems = list(csv.DictReader(io.TextIOWrapper(response, encoding="utf-8")))

print(len(poems), "rows")
```

## Columns

The examples are poem 60 (大江山). The same list, with types, is in [`datapackage.json`](datapackage.json).

### Poem and poet

|Column|Description|Example|
|-|-|-|
|`number`|The poem's number in the Hyakunin Isshu, from 1 to 100. One extra row, `序歌`, is the opening poem read before every competitive karuta match. It is not one of the 100 and has no card, so its color, kimariji and torifuda columns are empty.|`60`|
|`color`|Which of the five color groups the poem belongs to in five-color karuta (五色百人一首): 桃 pink, 青 blue, 黄 yellow, 緑 green or 橙 orange.|`黄`|
|`color_num`|The poem's position within its color group, from 1 to 20.|`12`|
|`author`|The poet's name with its reading attached, written as `[name\|reading]`.|`[小式部内侍\|こしきぶのないし]`|
|`author_kanji`|The poet's name as it is normally written.|`小式部内侍`|
|`author_hiragana`|How the poet's name is read, in hiragana.|`こしきぶのないし`|

### Kimariji

|Column|Description|Example|
|-|-|-|
|`kimariji_kami`|The "deciding sounds": the shortest start of the upper half (lines 1–3) that tells this poem apart from all the others. Written in old-style kana.|`おほえ`|
|`kimariji_kami_yomi`|The same deciding sounds, written the way they are pronounced today.|`おおえ`|
|`kimariji_shimo`|The shortest start of the lower half (lines 4–5) that tells this poem apart from all the others, with dakuten (the ゛ marks, as in が).|`まだ`|
|`kimariji_shimo_no_tenten`|The same as `kimariji_shimo`, but without dakuten, the way it looks on the cards.|`また`|

### The five lines

|Column|Description|Example|
|-|-|-|
|`verse_1`|The first line of the poem, with readings attached to the kanji as `[kanji\|reading]`. Readings use old-style kana.|`[大江山\|おほえやま]`|
|`verse_2`|The second line, in the same format as `verse_1`.|`いく[野\|の]の[道\|みち]の`|
|`verse_3`|The third line, in the same format as `verse_1`.|`[遠\|とほ]ければ`|
|`verse_4`|The fourth line, in the same format as `verse_1`.|`まだふみも[見\|み]ず`|
|`verse_5`|The fifth line, in the same format as `verse_1`.|`[天\|あま]の[橋立\|はしだて]`|
|`verse_1_kanji`|The first line of the poem as it is normally written, in kanji and kana.|`大江山`|
|`verse_2_kanji`|The second line, in the same format as `verse_1_kanji`.|`いく野の道の`|
|`verse_3_kanji`|The third line, in the same format as `verse_1_kanji`.|`遠ければ`|
|`verse_4_kanji`|The fourth line, in the same format as `verse_1_kanji`.|`まだふみも見ず`|
|`verse_5_kanji`|The fifth line, in the same format as `verse_1_kanji`.|`天の橋立`|
|`verse_1_hiragana`|The first line all in hiragana, with old-style spelling (how it is written, not how it sounds).|`おほえやま`|
|`verse_2_hiragana`|The second line, in the same format as `verse_1_hiragana`.|`いくののみちの`|
|`verse_3_hiragana`|The third line, in the same format as `verse_1_hiragana`.|`とほければ`|
|`verse_4_hiragana`|The fourth line, in the same format as `verse_1_hiragana`.|`まだふみもみず`|
|`verse_5_hiragana`|The fifth line, in the same format as `verse_1_hiragana`.|`あまのはしだて`|
|`verse_1_hiragana_yomi`|The first line all in hiragana, spelled the way it is actually pronounced (for example らむ becomes らん, and the particle は becomes わ).|`おおえやま`|
|`verse_2_hiragana_yomi`|The second line, in the same format as `verse_1_hiragana_yomi`.|`いくののみちの`|
|`verse_3_hiragana_yomi`|The third line, in the same format as `verse_1_hiragana_yomi`.|`とおければ`|
|`verse_4_hiragana_yomi`|The fourth line, in the same format as `verse_1_hiragana_yomi`.|`まだふみもみず`|
|`verse_5_hiragana_yomi`|The fifth line, in the same format as `verse_1_hiragana_yomi`.|`あまのはしだて`|

### Competition reading

|Column|Description|Example|
|-|-|-|
|`kyougi_yomi_kami`|The upper half as it is read aloud in competitive karuta, with readings attached as `[kanji\|reading]`. `ー` means hold the sound long, `ｰ` means hold it briefly. Based on the reader's guide of the All-Japan Karuta Association.|`[大江山\|おおえやま]ーいく[野\|の]の[道\|みち]のｰ[遠\|とお]けれーばー`|
|`kyougi_yomi_shimo`|The same as `kyougi_yomi_kami`, for the lower half.|`まだｰふみも[見\|み]ずー[天\|あま]の[橋\|はし]ー[立\|だて]`|
|`kyougi_yomi_kami_kanji`|The upper half as read aloud in competitive karuta, in kanji and kana, with the `ー`/`ｰ` hold marks.|`大江山ーいく野の道のｰ遠けれーばー`|
|`kyougi_yomi_shimo_kanji`|The same as `kyougi_yomi_kami_kanji`, for the lower half.|`まだｰふみも見ずー天の橋ー立`|
|`kyougi_yomi_kami_hiragana`|The upper half as read aloud in competitive karuta, all in hiragana as pronounced, with the `ー`/`ｰ` hold marks.|`おおえやまーいくののみちのｰとおけれーばー`|
|`kyougi_yomi_shimo_hiragana`|The same as `kyougi_yomi_kami_hiragana`, for the lower half.|`まだｰふみもみずーあまのはしーだて`|

### Card

|Column|Description|Example|
|-|-|-|
|`torifuda_1`|The first line printed on the card players grab (torifuda). The card shows the lower half in old-style kana without dakuten: 5 characters, then 5, then the rest.|`またふみも`|
|`torifuda_2`|The second line printed on the card.|`みすあまの`|
|`torifuda_3`|The third line printed on the card: whatever characters are left.|`はしたて`|

## Examples

### Find the one-sound cards

These are the seven cards you can take after hearing only one sound (the famous むすめふさほせ):

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

### Show the readings above the kanji in a web page

The ruby columns use `[kanji|reading]`. One line turns them into HTML `<ruby>` tags, which every browser can show:

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

### Print a card

```python
import csv

with open("data.csv", encoding="utf-8", newline="") as f:
    poems = {row["number"]: row for row in csv.DictReader(f)}

def print_card(poem):
    # Cards are read top to bottom, right to left.
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

### Use pandas

How many poems have a kimariji of 1, 2, 3 … sounds:

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

## Notes

- **Format:** Columns without an ending (`author`, `verse_1` … `verse_5`, `kyougi_yomi_kami`, `kyougi_yomi_shimo`) use `[kanji|reading]`, for example `[大江山|おほえやま]`. Keep only the kanji and you get the `_kanji` column. Keep only the readings and you get the `_hiragana` column.
- **The competition reading can differ a little from the poem.** In poem 74 the poem has よ at the end of line 3, but it is not read in competition. In the 序歌, line 4 is 今は春べと in the poem but is read as 今を春べと.
- Nobashi marks in the kyougi section differ in length. Refer to the [Competitive yomite textbook](https://www.karuta.or.jp/karuta/reading/) for details.
- **Empty cells** only appear in the 序歌 row. It has no card, so its color, kimariji and torifuda columns are empty.
- **File format:** UTF-8 without BOM, commas between cells, one row per line. No cell contains a comma, so there are no quotes. In Excel, open the file with *Data → From Text/CSV* and choose UTF-8, or the Japanese text may look broken.

## Data sources

| Source | Used for |
|-|-|
| [全日本かるた協会『競技かるた読手テキスト』改訂版 (2025-04-01)](https://www.karuta.or.jp/karuta/reading/) | The competition reading, the nobashi marks and the `_yomi` readings |
| [近江神宮「小倉百人一首一覧」](https://oumijingu.org/pages/130/) | Checking the poem text, the kimariji and the poets |
| [小倉山荘「ちょっと差がつく『百人一首講座』」](https://ogurasansou.jp.net/columns_category/hyakunin/) | Checking the poem text and the poets |
| [かるたらいふ「取り札一覧（決まり字版）」](https://karutalife.sakura.ne.jp/education/003/) | Checking the card text |
| [Wikipedia「難波津 (和歌)」](https://ja.wikipedia.org/wiki/%E9%9B%A3%E6%B3%A2%E6%B4%A5_(%E5%92%8C%E6%AD%8C)) | The text and poet of the 序歌 |

These sources belong to their owners. Where they disagree, this is what the data uses:

- **Poems 89 and 92:** the card sheet writes よはり and かはく. The data uses the usual old-style spelling よわり and かわく.

## Checks

Every push and pull request runs these checks (in [`.github/validators`](.github/validators)):

| Check | What it makes sure |
|-|-|
| `rows.py` | Every row has every column, and no cell is empty (except the 序歌 card columns). |
| `numbers.py` | The numbers 1 to 100 and 序歌 each appear once, and each color number 1 to 20 appears five times. |
| `ruby.py` | Every ruby column gives back its `_kanji` and `_hiragana` columns. |
| `datapackage.py` | `datapackage.json` and both READMEs list the same columns as `data.csv`, with the same examples. |
| Frictionless | Every cell matches the types and rules in `datapackage.json`. |

Run them yourself:

```bash
for script in .github/validators/*.py; do python3 "$script"; done
pip install frictionless
frictionless validate datapackage.json
```

## Contributing

Found a mistake, or have an idea? Thank you!

1. **Open an [issue](https://github.com/StoneLabs/hyakuninissyu-csv/issues) first.** I usually answer quickly.
2. **If the change makes sense, I add the [`PR Welcome`](https://github.com/StoneLabs/hyakuninissyu-csv/labels/PR%20Welcome) label** to the issue.
3. **Then open a pull request** for that issue. Pull requests without an issue labeled `PR Welcome` are not accepted.

When you fix the data, please say which source you used, and run the [checks](#checks) before you push. Commit messages start with `data:`, `doc:` or `validator:`.

## License

[The Unlicense](LICENSE)
