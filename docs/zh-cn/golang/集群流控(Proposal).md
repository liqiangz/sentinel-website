1. 借助于redis做全局的流控，一个server周期放token，client全部都去redis拿token。这种做起来相对简单，也容易做，会依赖于redis。

2. 和java里面一个套路，token server做控制，客户端很轻。

3. Token server周期下发token给各个client。token server负责流控策略。实际流控还是在客户端。

4. Client端做流控，周期上报给token server。token server聚合然后分发给client。实际流控策略还是客户端。token server做全局统计的聚合。这种会有时间差问题，只能说是非严格集群流控。

嗯，集群流控这一块 没有万灵药，总会有性能与准确性的权衡。前期我们可以分为两块来搞：（1）抽集群流控接口（Go 层面）以及标准通信接口（可以基于 gRPC），后者方便搞标准化（2）基础的实现 可以先搞最简单的基于 Redis 的（结合前面的集群流控接口），这个可以放到 1.0 版本之后