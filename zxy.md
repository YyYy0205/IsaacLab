# 记录


## 日常记录
### 1.27 

  1. `LocomotionVelocityroughEnvCfg `

  
  2. 训练： `scripts/reinforcement_learning/rsl_rl/train.py --task Isaac-Velocity-Rough-G1_v0 --num_envs 2000 --headless --max_iterations 1000`

     [Isaac中文教程](https://www.bilibili.com/video/BV1SK4CzHEnb?spm_id_from=333.788.videopod.sections&vd_source=527df480cd41c3e25ef0e62a90dca33a) 1.3.1/1.3.2

```
    * scripts/reinforcement_learning/rsl_rl/train.py
    这是训练脚本的路径，位于你的项目目录中。
    表明你正在使用 rsl_rl 这个强化学习库（Robotic Systems Lab RL，常用于Isaac Sim的研究项目）。
    该脚本负责：环境初始化 → 智能体（策略网络）构建 → 训练循环 → 模型保存。

    * --task Isaac-Velocity-Rough-G1_v0 ⭐ 核心参数
    这是要训练的任务环境标识符，拆解如下：
    Isaac	平台	表示这是 Isaac Sim/Gym 环境
    Velocity	任务类型	速度跟踪任务。机器人需要按照指令速度移动
    Rough	地形类型	粗糙地形（可能有小障碍、不平整地面）
    G1	机器人型号	通常指 ANYmal 四足机器人的某个版本（G = Generation）
    _v0	版本号	环境配置的第0个版本

    * --num_envs 2000 ⭐ 关键性能参数
    含义：同时并行运行 2000 个独立的环境实例。
    典型值：在 Isaac Sim 中，通常设置 512–4096 个环境。2000 是一个常见的折中值。

    * --headless ⭐ 运行模式参数
    含义：不启动图形界面（无 GUI 渲染）
    ```
    * --checkpoint_interval	 
    --checkpoint_interval 500	每多少步保存一次模型
```
  3.  验证：`scripts/reinforcement_learning/rsl_rl/play.py --task Isaac-Velocity-Rough-G1_v0 --load_run 2026-01-27_16-46-23 `
<img width="1447" height="937" alt="截图 2026-01-27 17-59-50" src="https://github.com/user-attachments/assets/fdea2bba-54e9-41a4-abf1-ae33ca771945" />


### 1.28 
1. 机器人模型格式

   [Isaac中文教程](https://www.bilibili.com/video/BV1SK4CzHEnb?spm_id_from=333.788.videopod.sections&vd_source=527df480cd41c3e25ef0e62a90dca33a) 1.4.1

* 导入unitree G1机器人urdf
   > .[宇树机器人模型](https://github.com/unitreerobotics/unitree_ros)
    <img width="1340" height="785" alt="unitree g1urdf" src="https://github.com/user-attachments/assets/57300d88-391a-4d9f-b3f5-4c67d68369fd" />
* urdf转换为usd
   * 命令`python scripts/tools/convert_urdf.py /home/yons/isaac_study/unitree_ros-master/robots/g1_description/g1_23dof.urdf /home/yons/isaac_study/unitree_ros-master/robots/g1_description/g1_23dof.usd`
   * 或者使用Isaac sim 5.1.0 自带的转换   file-import 
   > <img width="1431" height="604" alt="截图 2026-01-28 16-06-51" src="https://github.com/user-attachments/assets/eebfcd4f-3fa8-4205-b6f5-50ab91b7db53" />
----------------------------------------------
2. 机器人配置

   [Isaac中文教程](https://www.bilibili.com/video/BV1SK4CzHEnb?spm_id_from=333.788.videopod.sections&vd_source=527df480cd41c3e25ef0e62a90dca33a) 1.4.2
   > 路径：/home/yons/isaac_study/IsaacLab/source/isaaclab_assets/isaaclab_assets/robots/unitree.py

   ```
    G1_CFG = ArticulationCfg(
    spawn=sim_utils.UsdFileCfg( # 导入格式usd
        usd_path=f"{ISAACLAB_NUCLEUS_DIR}/Robots/Unitree/G1/g1.usd", #可换成自己本地的模型
        activate_contact_sensors=True, # 激活接触传感器 
        rigid_props=sim_utils.RigidBodyPropertiesCfg(
            disable_gravity=False,
            retain_accelerations=False,
            linear_damping=0.0,
            angular_damping=0.0,
            max_linear_velocity=1000.0,
            max_angular_velocity=1000.0,
            max_depenetration_velocity=1.0,
        ),
        articulation_props=sim_utils.ArticulationRootPropertiesCfg(
            enabled_self_collisions=False, solver_position_iteration_count=8, solver_velocity_iteration_count=4 #自碰撞
        ),
    ),
   ```
----------------------------------
3. 刚体配置

   [Isaac中文教程](https://www.bilibili.com/video/BV1SK4CzHEnb?spm_id_from=333.788.videopod.sections&vd_source=527df480cd41c3e25ef0e62a90dca33a)1.4.3
   >  路径：IsaacLab/scripts/demos/multi_asset.py
    ```
    >> `class MultiObjectSceneCfg(InteractiveSceneCfg`
    >> `object: RigidObjectCfg = RigidObjectCfg`
    >> `object_collection: RigidObjectCollectionCfg = RigidObjectCollectionCfg`: 把多个东西打包ojectA，objectB....
    ```
    运行demo,终端：`python scripts/demos/multi_asset.py`
    <img width="962" height="604" alt="1 4 3" src="https://github.com/user-attachments/assets/992a2bfe-7983-4662-bc05-277bc024529f" />

---------------------------------
4. 事件配置 EventCfg

     [Isaac中文教程](https://www.bilibili.com/video/BV1SK4CzHEnb?spm_id_from=333.788.videopod.sections&vd_source=527df480cd41c3e25ef0e62a90dca33a) 1.5.1
     >路径：/home/yons/isaac_study/IsaacLab/source/isaaclab_tasks/isaaclab_tasks/manager_based/locomotion/velocity/velocity_env_cfg.py
     
     + startup 环境初始化时被使用
     >包括：mdp.randomize_rigid_body_material， mdp.randomize_rigid_body_mass，randomize_rigid_body_com（重心）。随即训练后，可增加鲁棒性
     + reset 恢复机器人初始状态
     >包括：apply_external_force_torque，reset_root_state_uniform恢复站立的位置/朝向，reset_joints_by_scale
     + interval 更具时间发生
     >包括：push_by_setting_velocity interval_range_s=(10.0, 15.0),每隔10-15秒以一定速度推动机器人
     

    
