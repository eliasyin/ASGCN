# Brief introduction of this project

## 1.feature extract
    dependency_graphbiedges.py  -> generate  .edgevocab  .graph file
        |
        |----> .tree  dependency adj有向图
        |----> .graph  dependency adj无向图

    data_utils.py -> generate .embedding_matrix  .datas.pkl
        |
        |----> .embedding_matrix 用于模型初始化
        |----> .datas.pkl  所有特征整合到一块

## 2. train
    trina.py
        |
        |----> bucket_iterator : dataset & dataloader
                    |
                    |---- : pad data (vectors & matrixs)  每个batch的padding长度都会变化


## 3. ASGCN


需要注意的是，这个模型使用glove进行tokenize&spacy进行依赖解析，但是并没有进行对齐

```python
def dependency_adj_matrix(text):
    # https://spacy.io/docs/usage/processing-text
    tokens = nlp(text)
    words = text.split()
    matrix = np.zeros((len(words), len(words))).astype('float32')
    assert len(words) == len(list(tokens))   # <--------- 这里确保glove分词结果和spacy分词结果长度一致

    for token in tokens:
        matrix[token.i][token.i] = 1
        for child in token.children:
            matrix[token.i][child.i] = 1

    return matrix
```


一些疑问：
ABSADatesetReader