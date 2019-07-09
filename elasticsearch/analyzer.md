## 分词器的组成
1. Character Filters：针对原始文本处理，例如去除html
2. Tokenizer：按照规则切分为单词
3. Token Filter：将切分的单词进行加工，如转换为小写、删除stopwords、增加同义词等

## Elasticsearch分词器
### Standard Analyzer
1. ES默认分词器；
2. 按词切分；
3. 小写转换

### Simple Analyzer
1. 按非字母切分，去掉非字母的字符；
2. 小写转换

### Stop Analyzer
1. 相比Simple Analyzer，会将stop words如the, a去除掉

### Keyword Analyzer
1. 不做分词处理，直接将输入作为一个term输出

### 中文分词器
* ICU Analyzer
* IK：支持自定义词库，支持热更新分词字典
* THULAC 清华大学的

-------------------------
*记于2019-07-09*
