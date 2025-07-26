---
title: "Japanese & MeCab"
date: 2017-10-01
github: "japanese-text-analysis"
linkimg: "linkmecab.png"
short:  "Short introduction to tokenization & grammar analysis of Japanese sentences using examples w/Python & MeCab."
tags: ["archive", "project", "japanese"]
draft: false
---
In English, words are separated by spaces, which makes word tokenization very easy -- just split the sentence on whitespace, e.g. string.split() in Python. Japanese sentences rarely involve such a convenient word delimiter, so tokenizing on words requires an extra step. This is a quick introduction to word tokenization of Japanese sentences using Python and [MeCab](https://taku910.github.io/mecab/). These instructions apply to the Linux command line; if you're using MacOS, I suggest looking at [Homebrew Package Manager](https://brew.sh/) (use <code>brew</code> instead of <code>apt-get</code>), and for Windows, the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/about).

[MeCab](https://taku910.github.io/mecab/) is an open source morphological analysis engine that uses [conditional random fields](http://repository.upenn.edu/cgi/viewcontent.cgi?article=1162&context=cis_papers) to build probabilistic models for parameter estimation. If you’re curious about the technical details or advantages over HMMs/MEMMs/stochastic grammars, check out the linked paper.

Install [MeCab](https://taku910.github.io/mecab/) and [Git](https://git-scm.com/downloads):
```bash
$ sudo apt-get install mecab mecab-ipadic libmecab-dev mecab-ipadic-utf8 git
```

Install [mecab-ipadic-neologd](https://github.com/neologd/mecab-ipadic-neologd) (system dictionary w/neologisms pulled from language resources on the web):
```bash
$ git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git
$ cd mecab-ipadic-neologd
$ sudo ./bin/install-mecab-ipadic-neologd -n
```

Install the [Python wrapper for MeCab](https://github.com/SamuraiT/mecab-python3):
```bash
$ pip3 install mecab-python3
```

To check if MeCab is working properly, run <code>mecab</code> from the command line, and then enter a Japanese sentence at the blank prompt which follows (and hit enter).
```bash
$ mecab
今日はいい天気ですね。
今日	名詞,副詞可能,*,*,*,*,今日,キョウ,キョー
は	助詞,係助詞,*,*,*,*,は,ハ,ワ
いい	形容詞,自立,*,*,形容詞・イイ,基本形,いい,イイ,イイ
天気	名詞,一般,*,*,*,*,天気,テンキ,テンキ
です	助動詞,*,*,*,特殊・デス,基本形,です,デス,デス
ね	助詞,終助詞,*,*,*,*,ね,ネ,ネ
。	記号,句点,*,*,*,*,。,。,。
```

Specify the mecab-ipadic-neologd dictionary with <code>-d</code> on the command line (the path may vary -- try looking in <code>/usr/local/lib/mecab/dic</code> or <code>/usr/lib/x86_64-linux-gnu/mecab/dic</code>). The following example illustrates the benefit of using mecab-ipadic-neologd; the former Prime Minister’s name (安倍晋三), along with his wife’s (安倍昭恵), are properly identified and not split up into component kanji when tokenizing the sentence “Shinzo Abe and Akie Abe live in Tokyo.”

```bash
$ mecab
安倍晋三と安倍昭恵は東京に住んでいる。
安倍	名詞,固有名詞,人名,姓,*,*,安倍,アベ,アベ
晋	名詞,固有名詞,人名,名,*,*,晋,ススム,ススム
三	名詞,数,*,*,*,*,三,サン,サン
と	助詞,並立助詞,*,*,*,*,と,ト,ト
安倍	名詞,固有名詞,人名,姓,*,*,安倍,アベ,アベ
昭恵	名詞,固有名詞,人名,名,*,*,昭恵,アキエ,アキエ
は	助詞,係助詞,*,*,*,*,は,ハ,ワ
東京	名詞,固有名詞,地域,一般,*,*,東京,トウキョウ,トーキョー
に	助詞,格助詞,一般,*,*,*,に,ニ,ニ
住ん	動詞,自立,*,*,五段・マ行,連用タ接続,住む,スン,スン
で	助詞,接続助詞,*,*,*,*,で,デ,デ
いる	動詞,非自立,*,*,一段,基本形,いる,イル,イル
。	記号,句点,*,*,*,*,。,。,。
```
```bash
$ mecab -d /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
安倍晋三と安倍昭恵は東京に住んでいる。
安倍晋三	名詞,固有名詞,一般,*,*,*,安倍晋三,アベシンゾウ,アベシンゾー
と	助詞,並立助詞,*,*,*,*,と,ト,ト
安倍昭恵	名詞,固有名詞,人名,一般,*,*,安倍昭恵,アベアキエ,アベアキエ
は	助詞,係助詞,*,*,*,*,は,ハ,ワ
東京	名詞,固有名詞,地域,一般,*,*,東京,トウキョウ,トーキョー
に	助詞,格助詞,一般,*,*,*,に,ニ,ニ
住ん	動詞,自立,*,*,五段・マ行,連用タ接続,住む,スン,スン
で	助詞,接続助詞,*,*,*,*,で,デ,デ
いる	動詞,非自立,*,*,一段,基本形,いる,イル,イル
。	記号,句点,*,*,*,*,。,。,。
```

Change MeCab’s output with <code>mecab -O [style]</code>:
<ul>
<li><a href="http://chasen-legacy.osdn.jp">ChaSen-compatible</a> format: <code>-O chasen</code></li>
<li>phonetic readings: <code>-O yomi</code></li>
<li>split elements with a space: <code>-O wakati</code></li>
<li>show all information: <code>-O dump</code></li>
</ul>

```bash
$ mecab -O chasen
今日はいい天気ですね。
今日	キョウ	今日	名詞-副詞可能
は	ハ	は	助詞-係助詞
いい	イイ	いい	形容詞-自立	形容詞・イイ	基本形
天気	テンキ	天気	名詞-一般
です	デス	です	助動詞	特殊・デス	基本形
ね	ネ	ね	助詞-終助詞
。	。	。	記号-句点
```
```bash
$ mecab -O yomi
今日はいい天気ですね。
キョウハイイテンキデスネ。
```
```bash
$ mecab -O wakati
今日はいい天気ですね。
今日　は　いい　天気　です　ね　。
```
```bash
$ mecab -O dump
今日はいい天気ですね。
0	BOS	BOS/EOS,*,*,*,*,*,*,* 0 0 0 0 0 0 2 1 0.000 0.000 0.000 0
7	今日	名詞,副詞可能,*,*,*,今日,キョウ,キョー 0 6 1314 1314 67 2 0 1 0.000 0.000 0.000 3947
20	は	助詞,係助詞,*,*,*,は,ハ,ワ 6 9 261 261 16 6 0 1 0.000 0.000 0.000 4822
36	いい	形容詞,自立,*,形容詞・イイ,基本形,いい,イイ,イイ 9 15 37 37 10 6 0 1 0.000 0.000 0.000 7936
49	天気	名詞,一般,*,*,*,天気,テンキ,テンキ 15 21 1285 1285 38 2 0 1 0.000 0.000 0.000 10214
62	です	助動詞,*,*,特殊・デス,基本形,です,デス,デス 21 27 460 460 25 6 0 1 0.000 0.000 0.000 11527
73	ね	助詞,終助詞,*,*,*,ね,ネ,ネ 27 30 279 279 17 6 0 1 0.000 0.000 0.000 13779
78	。	記号,句点,*,*,*,。,。,。 30 33 8 8 7 3 0 1 0.000 0.000 0.000 10279
```

Change the output format to focus on specific information. Let’s look at the word/grammar component (<code>%m</code>), the part of speech numerical ID (<code>%h</code>), the character type (<code>%t</code>), and the CSV representation of features (part of speech, utilization, reading, etc) (<code>%H</code>), each separated with a tab character (<code>\t</code>) for readability.

```bash
$ mecab -F"%m\t%h\t%t\t%H\n" -E"EOS\n"
今日はいい天気ですね。
今日	67	2	名詞,副詞可能,*,*,*,*,今日,キョウ,キョー
は	16	6	助詞,係助詞,*,*,*,*,は,ハ,ワ
いい	10	6	形容詞,自立,*,*,形容詞・イイ,基本形,いい,イイ,イイ
天気	38	2	名詞,一般,*,*,*,*,天気,テンキ,テンキ
です	25	6	助動詞,*,*,*,特殊・デス,基本形,です,デス,デス
ね	17	6	助詞,終助詞,*,*,*,*,ね,ネ,ネ
。	7	3	記号,句点,*,*,*,*,。,。,。
```

The character type (<code>%t</code>) is the third column, and describes whether the element is kanji (2), hiragana (6), or punctuation (3) in the sentence above.

The numerical IDs describing parts of speech (<code>%h</code>) and the first few values in the representation of features (<code>%H</code>) indicate which grammar category below best describes the word/grammar component (common grammar elements are in bold in the table below). The representation of features, starting from the left, includes: original form, part of speech, part of speech sub-categories 1/2/3, conjugated form, inflection, reading, and pronunciation. Reading and pronunciation are given in kana.

<div class="divResponsive">
<table>
<thead>
<tr><th>Part of Speech EN</th><th></th><th></th><th></th><th>Part of Speech JP</th><th></th><th></th><th></th><th>PoS ID #&nbsp;&nbsp;</th><th>Examples</th></tr>
</thead>
<tbody>
<tr><td>Other</td><td></td><td></td><td></td><td>その他／間投</td><td></td><td></td><td></td><td>0</td><td></td></tr>
<tr><td>Filler</td><td></td><td></td><td></td><td>フィラー</td><td></td><td></td><td></td><td>1</td><td>えーと</td></tr>
<tr><td>Exclamation</td><td></td><td></td><td></td><td>感動詞</td><td></td><td></td><td></td><td>2</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Symbol</td><td>Alphabet</td><td></td><td></td><td>記号</td><td>アルファベット</td><td></td><td></td><td>3</td><td>Ａ，Ｂ，Ｃ</td></tr>
<tr><td>Symbol</td><td>General</td><td></td><td></td><td>記号</td><td>一般</td><td></td><td></td><td>4</td><td>？、！、￥</td></tr>
<tr><td>Symbol</td><td>Open Segment</td><td></td><td></td><td>記号</td><td>括弧開</td><td></td><td></td><td>5</td><td>（、【</td></tr>
<tr><td>Symbol</td><td>Close Segment</td><td></td><td></td><td>記号</td><td>括弧閉</td><td></td><td></td><td>6</td><td>）、】</td></tr>
<tr><td>Symbol</td><td>Period</td><td></td><td></td><td>記号</td><td>句点</td><td></td><td></td><td>7</td><td>。</td></tr>
<tr><td>Symbol</td><td>Blank</td><td></td><td></td><td>記号</td><td>空白</td><td></td><td></td><td>8</td><td></td></tr>
<tr><td>Symbol</td><td>Comma</td><td></td><td></td><td>記号</td><td>読点</td><td></td><td></td><td>9</td><td>、</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Adjective</td><td class="bold">Independent</td><td></td><td></td><td class="bold">形容詞</td><td class="bold">自立</td><td></td><td></td><td class="bold">10</td><td class="bold">長い、新しい</td></tr>
<tr><td>Adjective</td><td>Suffix</td><td></td><td></td><td>形容詞</td><td>接尾</td><td></td><td></td><td>11</td><td></td></tr>
<tr><td>Adjective</td><td>Dependent</td><td></td><td></td><td>形容詞</td><td>非自立</td><td></td><td></td><td>12</td><td>づらい、がたい</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Particle</td><td class="bold">Case-Making</td><td class="bold">General</td><td></td><td class="bold">助詞</td><td class="bold">格助詞</td><td class="bold">一般</td><td></td><td class="bold">13</td><td class="bold">の、から、を</td></tr>
<tr><td>Particle</td><td>Case-Making</td><td>Quotation</td><td></td><td>助詞</td><td>格助詞</td><td>引用</td><td></td><td>14</td><td>と</td></tr>
<tr><td>Particle</td><td>Case-Making</td><td>Phrase</td><td></td><td>助詞</td><td>格助詞</td><td>連語</td><td></td><td>15</td><td></td></tr>
<tr><td class="bold">Particle</td><td class="bold">Binding, Related</td><td></td><td></td><td class="bold">助詞</td><td class="bold">係助詞</td><td></td><td></td><td class="bold">16</td><td class="bold">は、も</td></tr>
<tr><td>Particle</td><td>Sentence-Ending</td><td></td><td></td><td>助詞</td><td>終助詞</td><td></td><td></td><td>17</td><td>ね、よ</td></tr>
<tr><td class="bold">Particle</td><td class="bold">Connecting</td><td></td><td></td><td class="bold">助詞</td><td class="bold">接続助詞</td><td></td><td></td><td class="bold">18</td><td class="bold">て、つつ、ので</td></tr>
<tr><td>Particle</td><td>Special</td><td></td><td></td><td>助詞</td><td>特殊</td><td></td><td></td><td>19</td><td></td></tr>
<tr><td>Particle</td><td>Adverb</td><td></td><td></td><td>助詞</td><td>副詞化</td><td></td><td></td><td>20</td><td>と、に</td></tr>
<tr><td>Particle</td><td>Deputy Auxiliary</td><td></td><td></td><td>助詞</td><td>副助詞</td><td></td><td></td><td>21</td><td>ばっかり</td></tr>
<tr><td>Particle</td><td>Ambiguous</td><td></td><td></td><td>助詞</td><td>副／並立／終</td><td></td><td></td><td>22</td><td>とか、だの</td></tr>
<tr><td>Particle</td><td>Parallel</td><td></td><td></td><td>助詞</td><td>並立助詞</td><td></td><td></td><td>23</td><td></td></tr>
<tr><td>Particle</td><td>Consolidating</td><td></td><td></td><td>助詞</td><td>連体化</td><td></td><td></td><td>24</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Auxiliary Verb</td><td></td><td></td><td></td><td class="bold">助動詞</td><td></td><td></td><td></td><td class="bold">25</td><td class="bold">です</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Conjunction</td><td></td><td></td><td></td><td>接続詞</td><td></td><td></td><td></td><td>26</td><td>たとえば</td></tr>
<tr><td>Conjunction</td><td>Adj Conj</td><td></td><td></td><td>接頭詞</td><td>形容詞接続</td><td></td><td></td><td>27</td><td>お、まっ</td></tr>
<tr><td>Conjunction</td><td>Multiple</td><td></td><td></td><td>接頭詞</td><td>数接続</td><td></td><td></td><td>28</td><td>No.、計、毎分</td></tr>
<tr><td>Conjunction</td><td>Verb Conj</td><td></td><td></td><td>接頭詞</td><td>動詞接続</td><td></td><td></td><td>29</td><td>ぶっ、引き</td></tr>
<tr><td>Conjunction</td><td>Noun Conj</td><td></td><td></td><td>接頭詞</td><td>名詞接続</td><td></td><td></td><td>30</td><td>もと</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Verb</td><td class="bold">Independent</td><td></td><td></td><td class="bold">動詞</td><td class="bold">自立</td><td></td><td></td><td class="bold">31</td><td class="bold">食べ、見</td></tr>
<tr><td>Verb</td><td>Suffix</td><td></td><td></td><td>動詞</td><td>接尾</td><td></td><td></td><td>32</td><td>する、させる</td></tr>
<tr><td>Verb</td><td>Dependent</td><td></td><td></td><td>動詞</td><td>非自立</td><td></td><td></td><td>33</td><td>しまう</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Adverb</td><td>General</td><td></td><td></td><td>副詞</td><td>一般</td><td></td><td></td><td>34</td><td>たいそう</td></tr>
<tr><td>Adverb</td><td></td><td></td><td></td><td>副詞</td><td>助詞類接続</td><td></td><td></td><td>35</td><td>あまり、いつも</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Noun</td><td></td><td></td><td></td><td class="bold">名詞</td><td class="bold">サ変接続</td><td></td><td></td><td class="bold">36</td><td></td></tr>
<tr><td>Noun</td><td>Adj Stem</td><td></td><td></td><td>名詞</td><td>ナイ形容詞語幹</td><td></td><td></td><td>37</td><td>とんでも</td></tr>
<tr><td class="bold">Noun</td><td class="bold">General</td><td></td><td></td><td class="bold">名詞</td><td class="bold">一般</td><td></td><td></td><td class="bold">38</td><td class="bold">犬、本、電車</td></tr>
<tr><td>Noun</td><td>Quotation</td><td></td><td></td><td>名詞</td><td>引用文字列</td><td></td><td></td><td>39</td><td>いわく</td></tr>
<tr><td>Noun</td><td>Noun-Adj-Verb Stem</td><td></td><td></td><td>名詞</td><td>形容動詞語幹</td><td></td><td></td><td>40</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Noun</td><td class="bold">Proper Noun</td><td class="bold">General</td><td></td><td class="bold">名詞</td><td class="bold">固有名詞</td><td class="bold">一般</td><td></td><td class="bold">41</td><td></td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Name</td><td>General</td><td>名詞</td><td>固有名詞</td><td>人名</td><td>一般</td><td>42</td><td></td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Name</td><td>Last</td><td>名詞</td><td>固有名詞</td><td>人名</td><td>姓</td><td>43</td><td>井上、山田</td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Name</td><td>First</td><td>名詞</td><td>固有名詞</td><td>人名</td><td>名</td><td>44</td><td>アンバー</td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Organization</td><td></td><td>名詞</td><td>固有名詞</td><td>組織</td><td></td><td>45</td><td></td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Area</td><td>General</td><td>名詞</td><td>固有名詞</td><td>地域</td><td>一般</td><td>46</td><td>東京、北海道</td></tr>
<tr><td>Noun</td><td>Proper Noun</td><td>Area</td><td>Country</td><td>名詞</td><td>固有名詞</td><td>地域</td><td>国</td><td>47</td><td>中国、アメリカ</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Noun</td><td>Number</td><td></td><td></td><td>名詞</td><td>数</td><td></td><td></td><td>48</td><td>ゼロ、一、万</td></tr>
<tr><td>Noun</td><td>Conjunction-Like</td><td></td><td></td><td>名詞</td><td>接続詞的</td><td></td><td></td><td>49</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Connection</td><td></td><td>名詞</td><td>接尾</td><td>サ変接続</td><td></td><td>50</td><td>入り</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>General</td><td></td><td>名詞</td><td>接尾</td><td>一般</td><td></td><td>51</td><td>ぎみ、ゆかり</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Adj Verb Stem</td><td></td><td>名詞</td><td>接尾</td><td>形容動詞語幹</td><td></td><td>52</td><td></td></tr>
<tr><td class="bold">Noun</td><td class="bold">Suffix</td><td class="bold">Counter</td><td></td><td class="bold">名詞</td><td class="bold">接尾</td><td class="bold">助数詞</td><td></td><td class="bold">53</td><td class="bold">人、円、階</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Aux Verb Stem</td><td></td><td>名詞</td><td>接尾</td><td>助動詞語幹</td><td></td><td>54</td><td>そ、そう</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Name</td><td></td><td>名詞</td><td>接尾</td><td>人名</td><td></td><td>55</td><td>君、さん</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Area</td><td></td><td>名詞</td><td>接尾</td><td>地域</td><td></td><td>56</td><td>市内、州</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Special</td><td></td><td>名詞</td><td>接尾</td><td>特殊</td><td></td><td>57</td><td>ぶり、み</td></tr>
<tr><td>Noun</td><td>Suffix</td><td>Adverb Possible</td><td></td><td>名詞</td><td>接尾</td><td>副詞可能</td><td></td><td>58</td><td>いっぱい</td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Noun</td><td>Pronoun</td><td>General</td><td></td><td>名詞</td><td>代名詞</td><td>一般</td><td></td><td>59</td><td>これ、ここ、彼</td></tr>
<tr><td>Noun</td><td>Pronoun</td><td>Reduction</td><td></td><td>名詞</td><td>代名詞</td><td>縮約</td><td></td><td>60</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td>Noun</td><td>Verb Dependent</td><td></td><td></td><td>名詞</td><td>動詞非自立的</td><td></td><td></td><td>61</td><td></td></tr>
<tr><td>Noun</td><td>Special</td><td>Aux Verb Stem</td><td></td><td>名詞</td><td>特殊</td><td>助動詞語幹</td><td></td><td>62</td><td></td></tr>
<tr><td>Noun</td><td>Dependent</td><td>General</td><td></td><td>名詞</td><td>非自立</td><td>一般</td><td></td><td>63</td><td>こと、もの</td></tr>
<tr><td>Noun</td><td>Dependent</td><td>Adj Verb Stem</td><td></td><td>名詞</td><td>非自立</td><td>形容動詞語幹</td><td></td><td>64</td><td>ふう、みたい</td></tr>
<tr><td>Noun</td><td>Dependent</td><td>Aux Verb Stem</td><td></td><td>名詞</td><td>非自立</td><td>助動詞語幹</td><td></td><td>65</td><td>よ、よう</td></tr>
<tr><td>Noun</td><td>Dependent</td><td>Adverb Possible</td><td></td><td>名詞</td><td>非自立</td><td>副詞可能</td><td></td><td>66</td><td>うち、さなか</td></tr>
<tr><td>Noun</td><td>Adverb Possible</td><td></td><td></td><td>名詞</td><td>副詞可能</td><td></td><td></td><td>67</td><td></td></tr>
<tr><td>&nbsp;</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
<tr><td class="bold">Pre-Noun Adjectival</td><td></td><td></td><td></td><td class="bold">連体詞</td><td></td><td></td><td></td><td class="bold">68</td><td class="bold">この、その</td></tr>
</tbody>
</table>
</div>

And now for a quick tutorial on using Python to 1) load a simple text file with some Japanese text, 2) perform word tokenization on individual sentences using MeCab, 3) count the most frequently used nouns in the text, 4) perform a quick-and-dirty conversion of past-tense sentences to present-tense. The files mentioned below are also available in the [GitHub repo](https://github.com/hologramber/japanese-text-analysis).

This uses a simplified version of the story [Ant and the Grasshopper](http://read.gov/aesop/052.html), in the file [grasshopper.txt](https://github.com/hologramber/japanese-text-analysis/blob/master/simple-jp-text-analysis/grasshopper.txt).

```
夏は暑いです。螽斯が木の下にいます。螽斯は歌を歌います。楽しいです。蟻が来ました。
蟻は食べ物を運びます。螽斯が言いました。蟻さん、一緒に歌を歌いましょう。
蟻は言いました。いいえ、歌いません。私達は働きます。螽斯は聞きました。
どうしてですか。冬は食べ物がありませんから。今は夏ですよ。螽斯は笑いました。
それから毎日、螽斯は歌を歌いました。働きませんでした。冬です。雪が降ります。
とても寒いです。螽斯の家には、食べ物がありません。蟻の家には、食べ物がたくさんあります。 
螽斯は、蟻の家へ行きました。螽斯は言いました。蟻さん、お願いです。食べ物を下さい。
蟻は言いました。私達は、夏、働きました。だから、食べ物があります。あなたは夏、何をしましたか。
```

And the following Python script: [grasshopper_analysis.py](https://github.com/hologramber/japanese-text-analysis/blob/master/simple-jp-text-analysis/grasshopper_analysis.py).
```python
import MeCab
from collections import Counter                             # for counting most common elements

# load MeCab and the mecab-ipadic-neologd dictionary
# https://github.com/neologd/mecab-ipadic-neologd
# mct = MeCab.Tagger("-O chasen -d /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd")
mct = MeCab.Tagger("-O chasen -d /usr/local/lib/mecab/dic/mecab-ipadic-neologd/")

# simple recreation of previous MeCab command line tests; parse and tokenize sentence
print(mct.parse('今日はいい天気ですね。'))

gh_text = open('grasshopper.txt', 'r').read()               # open text file and read it into gh_text

# split text into sentences on '。' character, and then replace the ending '。' on all sentences
gh_lines = gh_text.split('。')
gh_lines = [x + '。' for x in gh_lines if len(x) > 0]

nouns = []                                                  # create empty lists for nouns, verbs, adj
verbs = []
adjectives = []

for sentence in gh_lines:                                   # loop through each sentence in gh_lines
    jparse_bug = mct.parse(sentence)
    jparse = mct.parseToNode(sentence)

    while jparse:
        mct_split = jparse.feature.split(',')               # split features up by commas
        if mct_split[0] == '名詞':
            nouns.append(jparse.surface)                    # if word is a noun, add the element to nouns
        elif mct_split[0] == '動詞':
            verbs.append(jparse.surface)                    # if word is a verb, add the element to verbs
        elif mct_split[0] == '形容詞':
            adjectives.append(jparse.surface)               # if word is an adj, add the element to adj
        jparse = jparse.next                                # move on to the next word-token

noun_counts = str(Counter(nouns).most_common(3))            # count 3 most common nouns (convert to str)
verb_counts = str(Counter(verbs).most_common(3))            # count 3 most common verbs (convert to str)
adjective_counts = str(Counter(adjectives).most_common(3))  # count  3 most common adjectives (convert to str)

print(noun_counts)                                          # print the 3 most common nouns
print(adjective_counts)                                     # print the 3 most common adjectives
print(verb_counts)                                          # print the 3 most common verbs
```

Running [grasshopper_analysis.py](https://github.com/hologramber/japanese-text-analysis/blob/master/simple-jp-text-analysis/grasshopper_analysis.py) from the command line will (should?) print a breakdown of the example sentence「今日はいい天気ですね」and a list of the 3 most common nouns, adjectives, and verbs.

```python
$ python3 ./grasshopper_analysis.py
今日	キョウ	今日	名詞-副詞可能
は	ハ	は	助詞-係助詞
いい	イイ	いい	形容詞-自立	形容詞・イイ	基本形
天気	テンキ	天気	名詞-一般
です	デス	です	助動詞		特殊・デス	基本形
ね	ネ	ね	助詞-終助詞
。	。	。	記号-句点

[('螽斯', 9), 	('蟻', 8), 	('食べ物', 6)]
[('暑い', 1), 	('楽しい', 1), 	('寒い', 1)]
[('歌い', 4), 	('あり', 4), 	('言い', 4)]
```

Looking at the bottom three lines of text above:
<ul>
<li>The three most common nouns are: grasshopper (螽斯・キリギリス), ant (蟻・アリ), and food (食べ物・たべもの).</li>
<li>The three most common adjectives are: hot (暑い・あつい), fun (楽しい・たのしい), and cold (寒い・さむい).</li>
<li>The three most common verbs are (in dictionary form): to sing (歌う・うたう), to exist (ある), to say (言う・いう).</li>
</ul>

Next, [grasshopper_past_present.py](https://github.com/hologramber/japanese-text-analysis/blob/master/simple-jp-text-analysis/grasshopper_past_present.py) will print all of the sentences which are in present tense, followed by the sentences in past-tense, followed by the sentences in past-tense converted into present-tense. Changing 'した' to 'す' is a pretty messy strategy for changing past-tense to present, but there is only one edge case in this example with a negative-past sentence (〜ませんでした).

```python
import MeCab, re                                                    # regex library (for finding tense)

# load MeCab and the mecab-ipadic-neologd dictionary
# https://github.com/neologd/mecab-ipadic-neologd
# mct = MeCab.Tagger("-O chasen -d /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd")
mct = MeCab.Tagger("-O chasen -d /usr/local/lib/mecab/dic/mecab-ipadic-neologd/")

gh_text = open('grasshopper.txt', 'r').read()                       # open text file, read into gh_text

# split text into sentences on '。' character, and then replace the ending '。' on all sentences
gh_lines = gh_text.split('。')
gh_lines = [x + '。' for x in gh_lines if len(x) > 0]

past = set()                                                        # create empty set for past-tense lines

for sentence in gh_lines:                                           # loop through sentences in gh_lines
    jparse_bug = mct.parse(sentence)
    jparse = mct.parseToNode(sentence)

    while jparse:                                                   # while there are word-tokens in sentence
        if jparse.posid == 25:                                      # catches verb conjugations
            # in this case, past-tense conj. have 'た'
            if jparse.surface == 'た':
                past.add(sentence)                                  # if 'た' was found, add to past-tense
        jparse = jparse.next                                        # move to the next word-token

present = set(gh_lines) - past                                      # remove lines in past from gh_lines

print("Grasshopper and the Ant sentences in present-tense:")
[print(x) for x in present]                                         # "for every x in present, print x"
print("\nGrasshopper and the Ant sentences in past-tense:")
[print(x) for x in past]                                            # "for every x in past, print x"

new_present = []                                                    # empty list for past-tense-to-present

# change all of the sentences in past-tense, to present-tense
for past_sentence in list(past):
    # change negative-past to negative, and postive-past to present
    now_present = re.sub('ませんでした', 'ません', past_sentence)
    now_present = re.sub('した', 'す', past_sentence)
    new_present.append(now_present)                                 # add to list new_present sentences

print("\nGrasshopper and the Ant sentences, past-to-present:")
[print(x) for x in new_present]                                     # "for every x in new_present, print x"
```
```terminal
$ python3 ./grasshopper_past_present.py
Grasshopper and the Ant sentences in present-tense:
どうしてですか。
楽しいです。
螽斯が木の下にいます。
だから、食べ物があります。
蟻さん、お願いです。
とても寒いです。
蟻の家には、食べ物がたくさんあります。
蟻は食べ物を運びます。
蟻さん、一緒に歌を歌いましょう。
今は夏ですよ。
私達は働きます。
食べ物を下さい。
夏は暑いです。
いいえ、歌いません。
螽斯は歌を歌います。
冬です。
螽斯の家には、食べ物がありません。
冬は食べ物がありませんから。
雪が降ります。

Grasshopper and the Ant sentences in the past-tense:
私達は、夏、働きました。
蟻が来ました。
螽斯が言いました。
螽斯は、蟻の家へ行きました。
あなたは夏、何をしましたか。
それから毎日、螽斯は歌を歌いました。
螽斯は聞きました。
蟻は言いました。
働きませんでした。
螽斯は言いました。
螽斯は笑いました。

Grasshopper and the Ant sentences, once past, now present-tense:
私達は、夏、働きます。
蟻が来ます。
螽斯が言います。
螽斯は、蟻の家へ行きます。
あなたは夏、何をしますか。
それから毎日、螽斯は歌を歌います。
螽斯は聞きます。
蟻は言います。
働きませんです。
螽斯は言います。
螽斯は笑います。
```