# API设计

- [API Design 101: From Basics to Best Practices (youtube.com)](https://www.youtube.com/watch?v=7QfswaV0re4)
  - restfulAPI风格：
    - 对于同一个实体的操作就用相同的资源路径，比如对于商品的增删改查，就分别使用`/api/products`的 POST/GET/PUT/DELETE方法
    - rest的全拼是representational state transfer（表述性状态转移）
    - 特点：
      - 无状态，也就是说一次请求需要包含所有需要的信息，可能会是优点，也可能导致不必要的消耗。
      - 遵循标准HTTP方法
      - endpoint会变多，因为可能对同一个实体可能会需要有两种GET请求。
        - 某location + 某HTTP方法就是一个endpoint，形如`GET /posts/1`。在rest下，一个endpoint应该只对应一种资源。
      - 对于特定的资源可能会需要消耗更多endpoint（没懂？）
      - 使用json进行数据交换
  - 