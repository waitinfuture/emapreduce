# Hadoop Streaming {#concept_s4c_v4n_hfb .concept}

本章节介绍如何使用 PythonH 写 Hadoop Streaming作业。

## PythonH 写 Hadoop Streaming {#section_oty_x4n_hfb .section}

mapper 代码如下：

```
#!/usr/bin/env python
import sys
for line in sys.stdin:
    line = line.strip()
    words = line.split()
    for word in words:
        print '%s\t%s' % (word, 1)
```

reducer 代码如下：

```
#!/usr/bin/env python
from operator import itemgetter
import sys
current_word = None
current_count = 0
word = None
for line in sys.stdin:
    line = line.strip()
    word, count = line.split('\t', 1)
    try:
        count = int(count)
    except ValueError:
        continue
    if current_word == word:
        current_count += count
    else:
        if current_word:
            print '%s\t%s' % (current_word, current_count)
        current_count = count
        current_word = word
if current_word == word:
    print '%s\t%s' % (current_word, current_count)
```

假设 mapper 代码保存在/home/hadoop/mapper.py， reducer 代码保存在/home/hadoop/reducer.py ， 输入路径为 hdfs 文件系统的/tmp/input，输出路径为 hdfs 文件系统的 /tmp/output。则在 E-MapReduce 集群上提交下面的 hadoop 命令。

```
hadoop jar /usr/lib/hadoop-current/share/hadoop/tools/lib/hadoop-streaming-*.jar -file /home/hadoop/mapper.py -mapper mapper.py -file /home/hadoop/reducer.py -reducer reducer.py -input /tmp/hosts -output /tmp/output
```

