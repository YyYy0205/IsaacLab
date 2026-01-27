# 记录


## 日常记录
- 1.27

  1. `LocomotionVelocityroughEnvCfg `

  
  2. 训练： `scripts/reinforcemnet_learning/rsl_rl/train.py --task Isaac-Velocity-Rough-G1_v0 --num_envs 2000 --headless --max_iterations 10000`
    ```
    1. scripts/reinforcement_learning/rsl_rl/train.py
    这是训练脚本的路径，位于你的项目目录中。
    表明你正在使用 rsl_rl 这个强化学习库（Robotic Systems Lab RL，常用于Isaac Sim的研究项目）。
    该脚本负责：环境初始化 → 智能体（策略网络）构建 → 训练循环 → 模型保存。

    2. --task Isaac-Velocity-Rough-G1_v0 ⭐ 核心参数
    这是要训练的任务环境标识符，拆解如下：
    Isaac	平台	表示这是 Isaac Sim/Gym 环境
    Velocity	任务类型	速度跟踪任务。机器人需要按照指令速度移动
    Rough	地形类型	粗糙地形（可能有小障碍、不平整地面）
    G1	机器人型号	通常指 ANYmal 四足机器人的某个版本（G = Generation）
    _v0	版本号	环境配置的第0个版本

    3. --num_envs 2000 ⭐ 关键性能参数
    含义：同时并行运行 2000 个独立的环境实例。
    典型值：在 Isaac Sim 中，通常设置 512–4096 个环境。2000 是一个常见的折中值。

    4. --headless ⭐ 运行模式参数
    含义：不启动图形界面（无 GUI 渲染）
    ```

  3.  验证：`scripts/reinforcemnet_learning/rsl_rl/play.py --task Isaac-Velocity-Rough-G1_v0 --load run 2026-01-27_16-46-23 `

- 1.28