* ref: [Sparse high-resolution histograms for Prometheus
](https://docs.google.com/document/d/1cLNv3aufPZb3fNfaJgdaRBZsInZKKIHo9E6HinJVbpM/edit#)

桶分布：
1. zero-bucket-width：考虑趋近于0的数据的统计和存储
2. resolution、power
     bound = power ^ (bucket_index / resolution)：考虑值一般是符合卡方分布的，所以index约小，桶约密
     bucket_index也不是无限大，因为bound的精度受到float、double的精度限制

值分布
[offset, length, [delta]]，认为值分布稀疏
    1. [delta].size() == length
    2. offset = bucket_index - resolution
    3. delta 使用 变长整型压缩协议，所以最好存储小值

桶存储（猜测，具体看源码）
1. 桶分布
2. 正桶
3. 负桶

优势就是压缩率超高
劣势就是所有promql需要重新做兼容
