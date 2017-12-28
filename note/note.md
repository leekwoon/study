# tensorflow 속도 최적화
1. Freeze Graph
https://github.com/.../tenso.../python/tools/freeze_graph.py

2. Inference Optimization
https://github.com/.../tools/optimize_for_inference.py

3. Quantize
https://github.com/.../tools/graph_transforms/README.md

4. AOT & JIT Compile (training때도 가능)
```
jit_level = tf.OptimizerOptions.ON_1
config.graph_options.optimizer_options.global_jit_level = jit_level
```
5. tensorflow light