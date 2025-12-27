# 強化学習で使用できるタスク名

Isaac Labのタスクは基本的に強化学習で使用可能です。以下は主要なタスクです。

---

## 1. クラシックなRLタスク

### Cartpole（カートポール）

- `Isaac-Cartpole-v0` - 標準Cartpole（RSL-RL、RL Games、SKRL、SB3対応）
- `Isaac-Cartpole-RGB-v0` - RGBカメラ観測（RL Games対応）
- `Isaac-Cartpole-Depth-v0` - 深度カメラ観測（RL Games対応）
- `Isaac-Cartpole-RGB-ResNet18-v0` - ResNet18特徴抽出（RL Games対応）
- `Isaac-Cartpole-RGB-TheiaTiny-v0` - TheiaTiny特徴抽出（RL Games対応）

### Ant（アリ）

- `Isaac-Ant-v0` - Antロコモーション（RSL-RL、RL Games、SKRL、SB3対応）

### Humanoid（ヒューマノイド）

- `Isaac-Humanoid-v0` - ヒューマノイドロコモーション（RSL-RL、RL Games、SKRL、SB3対応）

---

## 2. ロコモーションタスク（速度追従）

### Unitree A1

- `Isaac-Velocity-Flat-Unitree-A1-v0` - 平坦地形（RSL-RL、SKRL、SB3対応）
- `Isaac-Velocity-Rough-Unitree-A1-v0` - 粗い地形（RSL-RL、SKRL、SB3対応）

### Unitree Go1

- `Isaac-Velocity-Flat-Unitree-Go1-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-Unitree-Go1-v0` - 粗い地形（RSL-RL、SKRL対応）

### Unitree Go2

- `Isaac-Velocity-Flat-Unitree-Go2-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-Unitree-Go2-v0` - 粗い地形（RSL-RL、SKRL対応）

### Unitree G1

- `Isaac-Velocity-Flat-G1-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-G1-v0` - 粗い地形（RSL-RL、SKRL対応）

### Unitree H1

- `Isaac-Velocity-Flat-H1-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-H1-v0` - 粗い地形（RSL-RL、SKRL対応）

### ANYbotics ANYmal-B

- `Isaac-Velocity-Flat-Anymal-B-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-Anymal-B-v0` - 粗い地形（RSL-RL、SKRL対応）

### ANYbotics ANYmal-C

- `Isaac-Velocity-Flat-Anymal-C-v0` - 平坦地形（RSL-RL、RL Games、SKRL対応、対称性対応あり）
- `Isaac-Velocity-Rough-Anymal-C-v0` - 粗い地形（RSL-RL、RL Games、SKRL対応、対称性対応あり）

### ANYbotics ANYmal-D

- `Isaac-Velocity-Flat-Anymal-D-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-Anymal-D-v0` - 粗い地形（RSL-RL、SKRL対応）

### Agility Robotics Cassie

- `Isaac-Velocity-Flat-Cassie-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Rough-Cassie-v0` - 粗い地形（RSL-RL、SKRL対応）

### Agility Robotics Digit

- `Isaac-Velocity-Flat-Digit-v0` - 平坦地形（RSL-RL対応）
- `Isaac-Velocity-Rough-Digit-v0` - 粗い地形（RSL-RL対応）

### Boston Dynamics Spot

- `Isaac-Velocity-Flat-Spot-v0` - 平坦地形（RSL-RL、SKRL対応）
- `Isaac-Velocity-Flat-Spot-Play-v0` - プレイモード

---

## 3. マニピュレーションタスク

### リフトタスク（Franka Panda）

- `Isaac-Lift-Cube-Franka-v0` - 関節位置制御（RSL-RL、RL Games、SKRL、SB3対応）
- `Isaac-Lift-Cube-Franka-Play-v0` - プレイモード
- `Isaac-Lift-Cube-Franka-IK-Abs-v0` - 絶対姿勢制御（IK）
- `Isaac-Lift-Teddy-Bear-Franka-IK-Abs-v0` - テディベアリフト（IK）

### リーチングタスク（Franka Panda）

- `Isaac-Reach-Franka-v0` - 関節位置制御（RSL-RL、RL Games、SKRL対応）
- `Isaac-Reach-Franka-Play-v0` - プレイモード
- `Isaac-Reach-Franka-IK-Abs-v0` - 絶対姿勢制御（IK、RSL-RL対応）
- `Isaac-Reach-Franka-IK-Rel-v0` - 相対姿勢制御（IK、RSL-RL対応）
- `Isaac-Reach-Franka-OSC-v0` - 操作空間制御（OSC）
- `Isaac-Reach-Franka-OSC-Play-v0` - OSCプレイモード

### リーチングタスク（UR10）

- `Isaac-Reach-UR10-v0` - 関節位置制御（RSL-RL、RL Games、SKRL対応）
- `Isaac-Reach-UR10-Play-v0` - プレイモード
- `Isaac-Reach-UR10-IK-Abs-v0` - 絶対姿勢制御（IK）

### キャビネット操作（Franka Panda）

- `Isaac-Open-Drawer-Franka-v0` - 関節位置制御（RSL-RL、RL Games、SKRL対応）
- `Isaac-Open-Drawer-Franka-Play-v0` - プレイモード
- `Isaac-Open-Drawer-Franka-IK-Abs-v0` - 絶対姿勢制御（IK）
- `Isaac-Open-Drawer-Franka-IK-Rel-v0` - 相対姿勢制御（IK）

### スタックタスク（Franka Panda）

- `Isaac-Stack-Cube-Franka-v0` - 関節位置制御
- `Isaac-Stack-Cube-Instance-Randomize-Franka-v0` - インスタンスランダマイゼーション
- `Isaac-Stack-Cube-Franka-IK-Rel-v0` - 相対姿勢制御（IK）
- `Isaac-Stack-Cube-Franka-IK-Rel-Visuomotor-v0` - ビジョン観測
- `Isaac-Stack-Cube-Franka-IK-Rel-Visuomotor-Cosmos-v0` - Cosmosビジョン
- `Isaac-Stack-Cube-Franka-IK-Abs-v0` - 絶対姿勢制御（IK）
- `Isaac-Stack-Cube-Instance-Randomize-Franka-IK-Rel-v0` - インスタンスランダマイゼーション（IK）
- `Isaac-Stack-Cube-Franka-IK-Rel-Blueprint-v0` - Blueprint
- `Isaac-Stack-Cube-Franka-IK-Rel-Skillgen-v0` - SkillGen
- `Isaac-Stack-Cube-Bin-Franka-IK-Rel-Mimic-v0` - ビン内スタッキング

### スタックタスク（UR10 + グリッパー）

- `Isaac-Stack-Cube-UR10-Long-Suction-IK-Rel-v0` - 長い吸着グリッパー
- `Isaac-Stack-Cube-UR10-Short-Suction-IK-Rel-v0` - 短い吸着グリッパー

### スタックタスク（Galbot）

- `Isaac-Stack-Cube-Galbot-Left-Arm-Gripper-RmpFlow-v0` - 左アームグリッパー（RMP Flow）
- `Isaac-Stack-Cube-Galbot-Right-Arm-Suction-RmpFlow-v0` - 右アーム吸着（RMP Flow）
- `Isaac-Stack-Cube-Galbot-Left-Arm-Gripper-Visuomotor-v0` - 左アームグリッパー（ビジョン）
- `Isaac-Stack-Cube-Galbot-Left-Arm-Gripper-Visuomotor-Joint-Position-Play-v0` - プレイモード
- `Isaac-Stack-Cube-Galbot-Left-Arm-Gripper-Visuomotor-RmpFlow-Play-v0` - RMP Flowプレイモード

### インハンドマニピュレーション（Allegro Hand）

- `Isaac-Repose-Cube-Allegro-v0` - キューブ再配置（RSL-RL、RL Games、SKRL対応）
- `Isaac-Repose-Cube-Allegro-Play-v0` - プレイモード
- `Isaac-Repose-Cube-Allegro-NoVelObs-v0` - 速度観測なし（RSL-RL、RL Games、SKRL対応）
- `Isaac-Repose-Cube-Allegro-NoVelObs-Play-v0` - 速度観測なしプレイモード

### デクスタス操作（KUKA + Allegro Hand）

- `Isaac-Dexsuite-Kuka-Allegro-Reorient-v0` - 再配置（RSL-RL、RL Games対応）
- `Isaac-Dexsuite-Kuka-Allegro-Reorient-Play-v0` - プレイモード
- `Isaac-Dexsuite-Kuka-Allegro-Lift-v0` - リフト（RSL-RL、RL Games対応）
- `Isaac-Dexsuite-Kuka-Allegro-Lift-Play-v0` - プレイモード

### プレースタスク（Agibot）

- `Isaac-Place-Toy2Box-Agibot-Right-Arm-RmpFlow-v0` - おもちゃを箱に配置
- `Isaac-Place-Mug-Agibot-Left-Arm-RmpFlow-v0` - マグを立てて配置

### デプロイタスク（UR10e）

- `Isaac-Deploy-Reach-UR10e-v0` - リーチング（RSL-RL対応）
- `Isaac-Deploy-Reach-UR10e-Play-v0` - プレイモード
- `Isaac-Deploy-Reach-UR10e-ROS-Inference-v0` - ROS推論

---

## 4. ナビゲーションタスク

### ANYmal-C

- `Isaac-Navigation-Flat-Anymal-C-v0` - 平坦地形ナビゲーション（RSL-RL、SKRL対応）
- `Isaac-Navigation-Flat-Anymal-C-Play-v0` - プレイモード

---

## 5. Direct環境（低レベルAPI）

Direct環境も強化学習で使用可能です。主なタスク：

- `Isaac-Ant-v0` (direct)
- `Isaac-Cartpole-v0` (direct)
- `Isaac-Humanoid-v0` (direct)
- `Isaac-Quadcopter-v0` (direct)
- `Isaac-Shadow-Hand-*` (direct) - Shadow Hand関連タスク
- `Isaac-Allegro-Hand-*` (direct) - Allegro Hand関連タスク
- `Isaac-Anymal-C-*` (direct) - ANYmal-C関連タスク

---

## 対応するRLライブラリ

各タスクは以下のRLライブラリに対応しています（タスクにより異なります）：

- **RSL-RL**: 多くのタスクで対応
- **RL Games**: 多くのタスクで対応
- **SKRL**: 多くのタスクで対応
- **Stable Baselines3 (SB3)**: 一部のタスクで対応（Cartpole、Ant、Humanoid、Lift、一部のロコモーションタスク）
- **Ray RLlib**: すべてのタスクで使用可能（ラッパー経由）

---

## 使用方法

### RSL-RLで訓練する場合

```bash
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py \
    --task <タスク名> \
    --num_envs 4096 \
    --headless
```

### RL Gamesで訓練する場合

```bash
./isaaclab.sh -p scripts/reinforcement_learning/rl_games/train.py \
    --task <タスク名> \
    --num_envs 4096 \
    --headless
```

### SKRLで訓練する場合

```bash
./isaaclab.sh -p scripts/reinforcement_learning/skrl/train.py \
    --task <タスク名> \
    --num_envs 4096 \
    --headless
```

### Stable Baselines3で訓練する場合

```bash
./isaaclab.sh -p scripts/reinforcement_learning/sb3/train.py \
    --task <タスク名> \
    --num_envs 4096 \
    --headless
```

---

## 注意事項

すべてのタスクがすべてのRLライブラリに対応しているわけではありません。タスクの登録情報（`rsl_rl_cfg_entry_point`、`rl_games_cfg_entry_point`など）を確認してください。

利用可能なタスクの一覧は、以下のコマンドで確認できます：

```bash
./isaaclab.sh -p scripts/environments/list_envs.py
```

