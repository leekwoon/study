튜토리얼
- http://cython.readthedocs.io/en/latest/src/userguide/wrapping_CPlusPlus.html
- https://cython.readthedocs.io/en/latest/
- https://github.com/cython/cython/wiki


시작할떄 참고
- http://bywbilly.com/2017/10/09/Light-Approaches-for-High-Efficiency-Python/
- http://nealhughes.net/cython1/
- https://homes.cs.washington.edu/~jmschr/lectures/Parallel_Processing_in_Python.html
  - python code(tensorflow)를 이용하면서 multithreading 을 지원하려면... joblib 이용해야할듯.
    - (윈도우 상에서 openmp가 지원되지 않아서...)

parallel 참고
- https://software.intel.com/en-us/articles/thread-parallelism-in-cython



nogil
- gil 쓰지않고...

except +
- c++ exceptions 이 cython 에 의해 handling
- http://cython.readthedocs.io/en/latest/src/userguide/wrapping_CPlusPlus.html

struct 관련 참고
- http://cython.readthedocs.io/en/latest/src/userguide/external_C_code.html
