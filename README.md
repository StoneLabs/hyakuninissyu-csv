# 百人一首、csvとして ／ Hyakunin-Issyu CSV

> ⚠️ ご注意: 最近、誤字と間違いが見つかった。他のもあるかもしれない。
> 
> I recently found typos and mistakes. There may be more.

PRを歓迎します。PR's welcome.

---

列は以下の通り（例は第60首「大江山」）：

Columns (the examples are poem 60, 大江山):

|Column|Description|Example|
|-|-|-|
|`number`|The poem's number in the Hyakunin Isshu, from 1 to 100.|`60`|
|`color`|Which of the five color groups the poem belongs to in five-color karuta (五色百人一首): 桃 pink, 青 blue, 黄 yellow, 緑 green or 橙 orange.|`黄`|
|`color_num`|The poem's position within its color group, from 1 to 20.|`12`|
|`author`|The poet's name with its reading attached, written as `[name\|reading]`.|`[小式部内侍\|こしきぶのないし]`|
|`author_kanji`|The poet's name as it is normally written.|`小式部内侍`|
|`author_hiragana`|How the poet's name is read, in hiragana.|`こしきぶのないし`|
|`kimariji_kami`|The "deciding sounds": the shortest start of the upper half (lines 1–3) that tells this poem apart from all the others. Written in old-style kana.|`おほえ`|
|`kimariji_kami_yomi`|The same deciding sounds, written the way they are pronounced today.|`おおえ`|
|`kimariji_shimo`|The shortest start of the lower half (lines 4–5) that tells this poem apart from all the others, with dakuten (the ゛ marks, as in が).|`まだ`|
|`kimariji_shimo_no_tenten`|The same as `kimariji_shimo`, but without dakuten, the way it looks on the cards.|`また`|
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
|`kyougi_yomi_kami`|The upper half as it is read aloud in competitive karuta, with readings attached as `[kanji\|reading]`. `ー` means hold the sound long, `ｰ` means hold it briefly. Based on the reader's guide of the All-Japan Karuta Association.|`[大江山\|おおえやま]ーいく[野\|の]の[道\|みち]のｰ[遠\|とお]けれーばー`|
|`kyougi_yomi_shimo`|The same as `kyougi_yomi_kami`, for the lower half.|`まだｰふみも[見\|み]ずー[天\|あま]の[橋\|はし]ー[立\|だて]`|
|`kyougi_yomi_kami_kanji`|The upper half as read aloud in competitive karuta, in kanji and kana, with the `ー`/`ｰ` hold marks.|`大江山ーいく野の道のｰ遠けれーばー`|
|`kyougi_yomi_shimo_kanji`|The same as `kyougi_yomi_kami_kanji`, for the lower half.|`まだｰふみも見ずー天の橋ー立`|
|`kyougi_yomi_kami_hiragana`|The upper half as read aloud in competitive karuta, all in hiragana as pronounced, with the `ー`/`ｰ` hold marks.|`おおえやまーいくののみちのｰとおけれーばー`|
|`kyougi_yomi_shimo_hiragana`|The same as `kyougi_yomi_kami_hiragana`, for the lower half.|`まだｰふみもみずーあまのはしーだて`|
|`torifuda_1`|The first line printed on the card players grab (torifuda). The card shows the lower half in old-style kana without dakuten: 5 characters, then 5, then the rest.|`またふみも`|
|`torifuda_2`|The second line printed on the card.|`みすあまの`|
|`torifuda_3`|The third line printed on the card: whatever characters are left.|`はしたて`|
