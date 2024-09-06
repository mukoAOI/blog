### 1.格式

**ID**: 单词的位置编号（从1开始）。

**FORM**: 单词的实际形式（如“我”，“爱”）。

**LEMMA**: 单词的词根形式（基本形式），通常与FORM相同。

**UPOS**: 通用词性标签（如`NOUN`表示名词，`VERB`表示动词）。

**XPOS**: 语言特定的词性标签（可选）。

**FEATS**: 其他语言特征（如名词的单复数，动词的时态等，通常以“键=值”对表示）。

**HEAD**: 依存关系中的头词的ID（表示当前单词依赖于哪个词）。

**DEPREL**: 当前单词与头词之间的关系标签（如`nsubj`表示主语，`obj`表示宾语）。

**DEPS**: 依存结构中的其他相关信息（通常为空）。

**MISC**: 其他附加信息（通常为空）



```
1	上海	上海	NR	NR	_	2	name	_	_
2	浦东	浦东	NR	NR	_	6	nmod:assmod	_	_
3	开发	开发	NN	NN	_	6	conj	_	_
4	与	与	CC	CC	_	6	cc	_	_
5	法制	法制	NN	NN	_	6	compound:nn	_	_
6	建设	建设	NN	NN	_	7	nsubj	_	_
7	同步	同步	VV	VV	_	0	root	_	_
```





```python
def conllu_to_text(conllu_format):
    lines = conllu_format.strip().split("\n")
    words = []

    for line in lines:
        if line.startswith("#") or not line.strip():
            continue
        fields = line.split("\t")
        if len(fields) > 1:  # 确保该行有足够的字段
            words.append(fields[1])  # 提取FORM字段
            if len(fields) == 10 and fields[9] != "SpaceAfter=No":
                words.append("")

    return "".join(words).strip()
```