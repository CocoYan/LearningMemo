## 分词器的组成
1. Character Filters：针对原始文本处理，例如去除html
2. Tokenizer：按照规则切分为单词
3. Token Filter：将切分的单词进行加工，如转换为小写、删除stopwords、增加同义词等
