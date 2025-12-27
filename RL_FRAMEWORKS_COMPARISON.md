# RSL-RL、SKRL、RL Games 比較解説

## 概要

Isaac Labでサポートされている3つの主要な強化学習ライブラリ（RSL-RL、SKRL、RL Games）について、それぞれの特徴、違い、使用場面を詳しく解説します。

---

## 1. RSL-RL (Robotic Systems Lab - Reinforcement Learning)

### 基本情報

- **開発元**: ETH ZurichのRobotic Systems Lab
- **リポジトリ**: https://github.com/leggedrobotics/rsl_rl
- **主な用途**: ロボティクス、特に脚式ロボットの制御

### 特徴

#### アルゴリズム
- **PPO (Proximal Policy Optimization)**: メインアルゴリズム
- **Distillation**: 知識蒸留による学習効率化
- シンプルで実装が明確

#### 技術的特徴
- **MLフレームワーク**: PyTorch専用
- **ベクトル化訓練**: 対応（大規模並列環境）
- **分散訓練**: 対応（マルチGPU/マルチノード）
- **マルチエージェント**: PPOベースの対応

#### パフォーマンス
- **訓練速度**: 最速（Isaac-Humanoid-v0で65.5Mステップを199秒）
- **メモリ効率**: 高い
- **GPU利用率**: 最適化されている

#### 設定方法
- **設定形式**: Pythonクラスベース（`@configclass`デコレータ）
- **設定ファイル例**:
```python
@configclass
class PPORunnerCfg(RslRlOnPolicyRunnerCfg):
    num_steps_per_env = 16
    max_iterations = 150
    policy = RslRlPpoActorCriticCfg(
        actor_hidden_dims=[32, 32],
        critic_hidden_dims=[32, 32],
    )
    algorithm = RslRlPpoAlgorithmCfg(
        learning_rate=1.0e-3,
        clip_param=0.2,
        entropy_coef=0.005,
    )
```

#### 長所
1. **最高のパフォーマンス**: 3つのライブラリの中で最も高速
2. **シンプルな実装**: コードが読みやすく、理解しやすい
3. **ロボティクス特化**: 脚式ロボットなどに最適化されている
4. **安定性**: 実績のある実装

#### 短所
1. **アルゴリズムの選択肢が少ない**: PPOとDistillationのみ
2. **ドキュメントが少ない**: コミュニティが小規模
3. **柔軟性が低い**: カスタマイズの余地が限定的

#### 使用推奨場面
- ロボティクスタスク（特に脚式ロボット）
- 最高のパフォーマンスが必要な場合
- PPOで十分な場合
- シンプルで理解しやすい実装を求める場合

---

## 2. SKRL (Scikit-Reinforcement Learning)

### 基本情報

- **開発元**: オープンソースコミュニティ
- **ドキュメント**: https://skrl.readthedocs.io
- **主な用途**: 汎用的な強化学習研究・開発

### 特徴

#### アルゴリズム
- **豊富なアルゴリズム選択肢**:
  - **On-Policy**: PPO, A2C
  - **Off-Policy**: SAC, TD3, DDPG
  - **Multi-Agent**: MAPPO, IPPO
  - **その他**: AMP (Adversarial Motion Priors)
- 最も幅広いアルゴリズムをサポート

#### 技術的特徴
- **MLフレームワーク**: PyTorch、JAX（両方対応）
- **ベクトル化訓練**: 対応
- **分散訓練**: 対応
- **マルチエージェント**: 専用アルゴリズム（MAPPO、IPPO）を含む包括的サポート

#### パフォーマンス
- **訓練速度**: 中程度（Isaac-Humanoid-v0で65.5Mステップを208秒）
- **メモリ効率**: 良好
- **GPU利用率**: 良好

#### 設定方法
- **設定形式**: YAMLファイルベース
- **設定ファイル例**:
```yaml
models:
  separate: False
  policy:
    class: GaussianMixin
    network:
      - name: net
        layers: [400, 200, 100]
        activations: elu
  value:
    class: DeterministicMixin
    network:
      - name: net
        layers: [400, 200, 100]
        activations: elu

agent:
  class: PPO
  rollouts: 32
  learning_epochs: 5
  mini_batches: 4
  learning_rate: 5.0e-04
  discount_factor: 0.99
  lambda: 0.95
```

#### 長所
1. **豊富なアルゴリズム**: 最も多くのアルゴリズムをサポート
2. **包括的なドキュメント**: 詳細なドキュメントとAPIリファレンス
3. **柔軟性**: 高いカスタマイズ性
4. **マルチフレームワーク**: PyTorchとJAXの両方に対応
5. **マルチエージェント**: 専用アルゴリズムを含む包括的サポート

#### 短所
1. **パフォーマンス**: RSL-RLより若干遅い
2. **設定の複雑さ**: YAML設定がやや複雑
3. **コミュニティ**: 小規模（ただしドキュメントは充実）

#### 使用推奨場面
- 複数のアルゴリズムを試したい場合
- Off-Policyアルゴリズム（SAC、TD3など）が必要な場合
- マルチエージェント強化学習
- JAXフレームワークを使用したい場合
- 研究・実験的な用途

---

## 3. RL Games

### 基本情報

- **開発元**: NVIDIA（元々はDenys88氏が開発）
- **リポジトリ**: https://github.com/Denys88/rl_games
- **主な用途**: ゲームAI、シミュレーション環境

### 特徴

#### アルゴリズム
- **PPO**: 主要アルゴリズム
- **SAC**: Soft Actor-Critic
- **A2C**: Advantage Actor-Critic
- 中程度のアルゴリズム選択肢

#### 技術的特徴
- **MLフレームワーク**: PyTorch専用
- **ベクトル化訓練**: 対応
- **分散訓練**: 対応
- **マルチエージェント**: PPOベースの対応
- **Population-Based Training (PBT)**: ハイパーパラメータ自動最適化機能

#### パフォーマンス
- **訓練速度**: 高速（Isaac-Humanoid-v0で65.5Mステップを207秒）
- **メモリ効率**: 良好
- **GPU利用率**: 最適化されている

#### 設定方法
- **設定形式**: YAMLファイルベース
- **設定ファイル例**:
```yaml
params:
  algo:
    name: a2c_continuous
  model:
    name: continuous_a2c_logstd
  network:
    name: actor_critic
    separate: False
    mlp:
      units: [400, 200, 100]
      activation: elu
  config:
    ppo: True
    mixed_precision: True
    normalize_input: True
    normalize_value: True
    gamma: 0.99
    tau: 0.95
    learning_rate: 5e-4
    lr_schedule: adaptive
    kl_threshold: 0.008
    e_clip: 0.2
    horizon_length: 32
    mini_epochs: 5
```

#### 長所
1. **高速**: RSL-RLに次ぐ高速な訓練速度
2. **PBT対応**: ハイパーパラメータ自動最適化
3. **混合精度訓練**: メモリ効率と速度の向上
4. **NVIDIAサポート**: 継続的な開発とサポート

#### 短所
1. **アルゴリズムの選択肢が限定的**: PPO、SAC、A2Cのみ
2. **ドキュメントが少ない**: コミュニティが小規模
3. **設定の複雑さ**: YAML設定がやや複雑

#### 使用推奨場面
- 高速な訓練が必要な場合
- PBTによるハイパーパラメータ最適化が必要な場合
- 混合精度訓練を活用したい場合
- ゲームAIやシミュレーション環境

---

## 4. 詳細比較表

| 特徴 | RSL-RL | SKRL | RL Games |
|------|--------|------|----------|
| **アルゴリズム** | PPO, Distillation | PPO, SAC, TD3, DDPG, MAPPO, IPPO, AMP, A2C | PPO, SAC, A2C |
| **MLフレームワーク** | PyTorch | PyTorch, JAX | PyTorch |
| **ベクトル化訓練** | ✅ | ✅ | ✅ |
| **分散訓練** | ✅ | ✅ | ✅ |
| **マルチエージェント** | PPOベース | 専用アルゴリズム含む | PPOベース |
| **訓練速度** | 199秒（最速） | 208秒 | 207秒 |
| **ドキュメント** | 低 | 包括的 | 低 |
| **コミュニティ** | 小規模 | 小規模 | 小規模 |
| **設定形式** | Pythonクラス | YAML | YAML |
| **柔軟性** | 中 | 高 | 中 |
| **PBT対応** | ❌ | ❌ | ✅ |
| **混合精度訓練** | ❌ | ❌ | ✅ |
| **Isaac Labでの例** | 大規模 | 大規模 | 大規模 |

### パフォーマンス比較（Isaac-Humanoid-v0）

- **環境**: RTX PRO 6000 GPU、4096環境、ヘッドレスモード
- **訓練ステップ**: 65.5Mステップ

| ライブラリ | 訓練時間 |
|-----------|---------|
| RSL-RL | 199秒 ⚡ |
| RL Games | 207秒 |
| SKRL | 208秒 |
| Stable-Baselines3 | 322秒（参考） |

---

## 5. アルゴリズム別の選択指針

### PPOを使用する場合

1. **RSL-RL**: 最高のパフォーマンスが必要な場合
2. **RL Games**: PBTや混合精度訓練が必要な場合
3. **SKRL**: 将来的に他のアルゴリズムに切り替える可能性がある場合

### Off-Policyアルゴリズム（SAC、TD3、DDPG）が必要な場合

- **SKRL**: 唯一の選択肢（RSL-RLとRL Gamesは対応していない）

### マルチエージェント強化学習の場合

- **SKRL**: MAPPO、IPPOなどの専用アルゴリズムが利用可能
- **RSL-RL / RL Games**: PPOベースの対応のみ

### JAXフレームワークを使用したい場合

- **SKRL**: 唯一の選択肢

---

## 6. 実装の違い

### 設定ファイルの形式

#### RSL-RL（Pythonクラス）
```python
@configclass
class PPORunnerCfg(RslRlOnPolicyRunnerCfg):
    num_steps_per_env = 16
    max_iterations = 150
    policy = RslRlPpoActorCriticCfg(...)
    algorithm = RslRlPpoAlgorithmCfg(...)
```

#### SKRL（YAML）
```yaml
models:
  policy:
    class: GaussianMixin
    network:
      - layers: [32, 32]
agent:
  class: PPO
  rollouts: 32
  learning_epochs: 8
```

#### RL Games（YAML）
```yaml
params:
  algo:
    name: a2c_continuous
  config:
    ppo: True
    gamma: 0.99
    learning_rate: 5e-4
```

### 訓練スクリプトの実行

#### RSL-RL
```bash
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py \
    --task Isaac-Cartpole-Direct-v0 \
    --num_envs 4096 \
    --headless
```

#### SKRL
```bash
./isaaclab.sh -p scripts/reinforcement_learning/skrl/train.py \
    --task Isaac-Cartpole-Direct-v0 \
    --num_envs 4096 \
    --algorithm PPO \
    --ml_framework torch \
    --headless
```

#### RL Games
```bash
./isaaclab.sh -p scripts/reinforcement_learning/rl_games/train.py \
    --task Isaac-Cartpole-Direct-v0 \
    --num_envs 4096 \
    --headless
```

---

## 7. まとめと推奨事項

### パフォーマンス重視の場合
→ **RSL-RL**: 最高の訓練速度を提供

### アルゴリズムの選択肢を重視する場合
→ **SKRL**: 最も豊富なアルゴリズムと柔軟性

### バランス型（速度と機能の両立）
→ **RL Games**: 高速でPBTなどの高度な機能も利用可能

### 研究・実験用途
→ **SKRL**: 豊富なアルゴリズムと包括的なドキュメント

### ロボティクス特化
→ **RSL-RL**: ロボティクスに最適化された実装

### マルチエージェント強化学習
→ **SKRL**: 専用アルゴリズム（MAPPO、IPPO）が利用可能

### JAXフレームワークを使用
→ **SKRL**: 唯一の選択肢

---

## 8. 参考資料

- **RSL-RL**: https://github.com/leggedrobotics/rsl_rl
- **SKRL**: https://skrl.readthedocs.io
- **RL Games**: https://github.com/Denys88/rl_games
- **Isaac Lab ドキュメント**: https://isaac-sim.github.io/IsaacLab/main/source/overview/reinforcement-learning/rl_frameworks.html

---

## 9. 補足情報

### Isaac Labでの統合

すべてのライブラリはIsaac Labの環境と統合されており、同じ環境設定を使用できます。主な違いは：

1. **エージェント設定ファイルの場所**:
   - RSL-RL: `agents/rsl_rl_ppo_cfg.py`
   - SKRL: `agents/skrl_ppo_cfg.yaml`
   - RL Games: `agents/rl_games_ppo_cfg.yaml`

2. **ラッパー**:
   - 各ライブラリ用の専用ラッパーが`source/isaaclab_rl/`に実装されている

3. **訓練スクリプト**:
   - `scripts/reinforcement_learning/{library_name}/train.py`

### 移行の容易さ

同じタスクで異なるライブラリを試すことは比較的容易です。環境設定は共通で、エージェント設定のみを変更すれば良いためです。

---

*このドキュメントはIsaac Labプロジェクトの情報に基づいて作成されました。*

