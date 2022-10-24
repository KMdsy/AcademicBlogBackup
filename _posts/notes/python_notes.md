---
title: Python语法查漏补缺
date: 2020-01-01 18:00:00
updated: 2022-05-26 22:51:00
tag:

- note
- python
---

仅记录一些项目中常用但总记不住的语法，希望能早日记住👐🏻

<!-- more -->

**目录**

- [Some reposts](#转载)
- [Datetime](#datetime)
- [Pandas](#pandas) 
- [Json](#json)
- [Networkx](#networkx)
- [Warnings](#warnings)
- [Pandoc (pypandoc)](#pypandoc)



## 转载

+ [python 字符串用法总结](https://mp.weixin.qq.com/s/dTrW68RjQ6K0nO5CPWUjlA)
+ [python 内置异常总结](https://www.cnblogs.com/nmb-musen/p/10856023.html)
+ [Python进度条库——tqdm](https://www.jianshu.com/p/21cf48be6bf6)



## pandas

普通的列赋值，官方新版本推荐使用`.loc`进行索引，如

```python
df.loc[:, 'site_id'] = [site_id] * data_len
# 旧版本，会报warning
df['site_id'] = [site_id] * data_len
```

按条件赋值DataFrame

```python
data_len = len(df[df['ulEutranCellId'] == user_id])
df.loc[df['ulEutranCellId'] == user_id, 'site_id'] = [site_id] * data_len
```

数据替换

```python
# df = df.replace(source, target)
df = df.replace(255, np.nan)
```

## datetime

字符串与timestamp的转换

```python
import datetime
def convert_str_to_timestamp(x):
    d_bj = datetime.datetime.strptime(x, '%Y-%m-%d %H:%M:%S')
    d_delta = datetime.timedelta(days=0, hours=8) # BJ = UTC + 8
    d_utc = d_bj - d_delta # UTC = BJ - 8
    timestamp = (d_utc - datetime.datetime(1970, 1, 1)).total_seconds()
    return timestamp

def convert_timestamp_to_str(x):
    d = datetime.datetime.utcfromtimestamp(x) # UTC时间
    d_delta = datetime.timedelta(days=0, hours=8) # BJ = UTC + 8
    d_cest = d + d_delta
    time_string = d_cest.strftime('%Y-%m-%d %H:%M:%S')
    return time_string
```

🌟pandas中的`pandas.datetime`对象可以通过`.to_pydatetime()`转为`datetime`对象，然后再用上面的方法。

## json

Python data to json

```python
import json
data = {'name':'name','email':'email'}
# sort_keys=True: 重新排列keys
# indent=4: 打印时的缩进
json_str = json.dumps(data, sort_keys=True, indent=4)
json_str = json_str.encode('utf-8').decode('unicode_escape')
print(json_str)
```

json to python data

```python
with open(json_path, 'r', encoding='utf-8') as f:
    context = json.load(f)
    f.close()
```



## networkx

### Build a graph

```python
import networkx as nx
edge_list = [(1, 0), (2, 3)] # [(node_idx1, node_idx2), ...]
edge_list = [('node1', 'node2'), ('node3', 'node4')] # [('node_name1', 'node_name2'), ...]
# use this
g = nx.DiGraph(edge_list)
# or
g = nx.DiGraph()
g.add_edges_from(edge_list)
```

### Find root/leaf node

遍历node并检查出/入度

```python
def find_root_node(g: nx.DiGraph):
    root = []
    for n, d in g.in_degree():
        if d == 0: root.append(n)
    return root
def find_leaf_node(g: nx.DiGraph):
    leaf = []
    for n, d in g.out_degree():
        if d == 0: leaf.append(n)
    return leaf
```

### 深度优先遍历图

```python
# 查找图中的根结点
root_node = find_root_node(g)
all_path = []
# 构造深度生成树
for root in root_node:
    t = nx.dfs_tree(g, source=root)
    paths = list(t.edges())
    all_path.append(paths)
    print(paths)
    # paths, e.g. [(0, 1), (1, 2), (2, 3), (3, 4)]
```



## warnings

### Output a warning

```python
import warnings
warnings.warn('Warning info.')
```

### Warning重定向

```python
# 重定向warnings的输出到stdout
def redirection():
    def customwarn(message, category, filename, lineno, file=None, line=None):
        sys.stdout.write(warnings.formatwarning(message, category, filename, lineno))
    warnings.showwarning = customwarn
# 重定向stdout
__console__ = sys.stdout # 以备恢复stdout到console
std_out = open(os.path.join(output_dir, 'somefile.txt'), 'w')
sys.stdout = std_out
redirection()
# 恢复重定向
sys.stdout = __console__
```



## pypandoc

将markdown文档转换为pdf文件

```python
# convert to pdf
pypandoc.convert_file(
  source_file=md_file,
  outputfile=pdf_file,
  to='pdf',
  format='md', 
  extra_args=['-V', 'geometry:margin=1cm'])
```



