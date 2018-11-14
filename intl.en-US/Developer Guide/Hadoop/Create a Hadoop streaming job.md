# Create a Hadoop streaming job {#concept_s4c_v4n_hfb .concept}

This topic describes how to use PythonH to create a Hadoop streaming job.

## Use Python to create a Hadoop streaming job {#section_oty_x4n_hfb .section}

The mapper code is as follows:

```
#! /usr/bin/env python
import sys
for line in sys.stdin:
    line = line.strip()
    words = line.split()
    for word in words:
        print '%s\t%s' % (word, 1)
```

The reducer code is as follows:

```
#! /usr/bin/env python
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

Suppose that the mapper code is saved as /home/hadoop/mapper.py and the reducer code is saved as /home/hadoop/reducer.py, and that the paths for input and output are /tmp/input and /tmp/output in the HDFS file system respectively. Submit the following Hadoop command in the E-MapReduce cluster.

```
hadoop jar /usr/lib/hadoop-current/share/hadoop/tools/lib/hadoop-streaming-*.jar -file /home/hadoop/mapper.py -mapper mapper.py -file /home/hadoop/reducer.py -reducer reducer.py -input /tmp/hosts -output /tmp/output
```

