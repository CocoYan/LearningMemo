Mapping
* Mapping类似数据库中的schema定义，作用如下
  * 定义索引中的字段名称和字段的数据类型
  * 定义字段的倒排索引的相关配置(Analyzed or Not Analyzed, Analyzer)
* Mapping会把JSON文档映射成Lucene所需要的扁平格式
* 一个Mapping属于一个索引的Type
  * 7.0开始，一个Index只有一个Type。不需要在Mapping定义中指定type信息。
  

## Dynamic Mapping
* 在写入文档的时候，如果索引不存在，会自动创建索引。Elasticsearch会自动根据文档信息，推算出文档的类型。(text字段会被自动加上keyword子字段)

### 能否更改Mapping的字段类型
* 新增加字段时：
  * Dynamic设为true时，一旦有新增字段的文档写入，Mapping也同时被更新。
  * Dynamic设为false时，Mapping不会被更新，新增字段无法被索引，但信息会存在这个文档的_source字段中
  * Dynamic设为strict时，文档写入失败
* 对已有字段，一旦已经有数据写入，就不再支持修改字段定义。
* 若希望改变字段类型，必须用Reindex API重建索引

## 显示Mapping定义
* 为减少输入的工作量，减少出错概率，可以依照以下步骤定义Mapping
  1. 创建一个临时index，写入一些样本数据
  2. 通过Mapping API获得该临时文件的动态Mapping定义
  3. 修改后，使用该配置创建你的索引
  4. 删除临时索引
  
* 常用参数
  * index - 控制当前字段是否被索引。默认为true。若设为false，则该字段不可被搜索。
  * index_options - 可以控制倒排索引记录的内容。记录的内容越多，占用存储空间越大。Text类型默认记录positions，其他默认为docs
    1. docs - 记录doc id
    2. freqs - 记录doc id和term frequencies
    3. positions - 记录doc id，term frequencies, term position
    4. offset - 记录doc id，term frequencies, term position, character offsets
  * null_value：需要对null值实现搜索。只有keyword类型的字段支持设置null_value
  
* 数组类型：Elasticsearch不提供专门的数组类型。但是任何字段都可以包含多个相同类型的数值。
