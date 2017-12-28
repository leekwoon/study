

- digital Curling
  - wrappers 작성할때 필요한것만...

export PYTHONPATH="/home/leekwoon/github_download/NoisyNet-DQN/baselines:$PYTHONPATH"

python3 train.py --env Breakout --no-double-q --noisy --save-dir NoisyNet-DQN

python3 train.py --env Breakout --no-double-q --save-dir Vanilla-DQN


python3 -m baselines.dqn.experiments.atari.train


# DQN baseline
- 사소하지만 Performance 에 중요한 요소...
  - gamma 1.0 보단 gamma 0.99 가 더 좋은 성능
  - 그냥 rgb를 gray로 바꿀때 scale이 ~ 255 까지의 값을 가지는데.. 이것을 0~1 값을 가지게 normalize 해주는 것이 큰 성능 향상...
    - 최고점 300 에서 약 400 으로...
