# Isaac Lab プロジェクト構造の詳細説明

このドキュメントでは、Isaac Labプロジェクトの全体構造と各ディレクトリの役割について詳しく説明します。

## 目次

1. [プロジェクト概要](#プロジェクト概要)
2. [ルートディレクトリ](#ルートディレクトリ)
3. [sourceディレクトリ（拡張機能）](#sourceディレクトリ拡張機能)
4. [scriptsディレクトリ（スタンドアロンアプリケーション）](#scriptsディレクトリスタンドアロンアプリケーション)
5. [toolsディレクトリ](#toolsディレクトリ)
6. [docsディレクトリ](#docsディレクトリ)
7. [dockerディレクトリ](#dockerディレクトリ)
8. [appsディレクトリ](#appsディレクトリ)

---

## プロジェクト概要

Isaac Labは、NVIDIA Isaac Sim上に構築されたGPU加速型のロボティクス研究フレームワークです。プロジェクトは**拡張機能（Extensions）**と**スタンドアロンアプリケーション（Standalone Applications）**の2つの主要な方法で構成されています。

```
IsaacLab/
├── source/          # 拡張機能（Isaac Sim Extensions）
├── scripts/         # スタンドアロンアプリケーション
├── tools/           # 開発・テストツール
├── docs/            # ドキュメント
├── docker/          # Docker設定
└── apps/            # Isaac Simアプリケーション設定
```

---

## ルートディレクトリ

### 主要ファイル

- **`isaaclab.sh`** / **`isaaclab.bat`**: Isaac Sim環境を起動するためのラッパースクリプト（Linux/Windows）
- **`pyproject.toml`**: プロジェクト全体のPython設定と依存関係
- **`environment.yml`**: Conda環境設定ファイル
- **`pytest.ini`**: pytest設定ファイル
- **`VERSION`**: プロジェクトのバージョン情報
- **`README.md`**: プロジェクトの概要とクイックスタートガイド
- **`CONTRIBUTING.md`**: コントリビューションガイドライン
- **`LICENSE`**: BSD-3ライセンス
- **`LICENSE-mimic`**: Apache 2.0ライセンス（isaaclab_mimic用）

---

## sourceディレクトリ（拡張機能）

`source/`ディレクトリには、Isaac Sim拡張機能として実装された主要なPythonパッケージが含まれています。各拡張機能は独立したPythonパッケージであり、`setup.py`と`config/extension.toml`を持ちます。

### 拡張機能の構造

各拡張機能は以下の構造を持ちます：

```
<extension-name>/
├── config/
│   └── extension.toml      # Isaac Sim拡張機能のメタデータ
├── docs/
│   ├── CHANGELOG.rst        # 変更履歴
│   └── README.md            # 拡張機能の説明
├── <extension-name>/        # メインのPythonパッケージ
│   ├── __init__.py
│   └── ...                  # 実装コード
├── test/                     # テストコード
├── pyproject.toml           # Pythonパッケージ設定
└── setup.py                  # セットアップスクリプト
```

### 1. isaaclab（コア拡張機能）

Isaac Labのコアインターフェース拡張機能。ロボット、センサー、環境の基本機能を提供します。

**主要モジュール:**

#### `actuators/` - アクチュエータ

- **`actuator_base.py`** / **`actuator_base_cfg.py`**: アクチュエータの基底クラスと設定
- **`actuator_cfg.py`**: アクチュエータ設定の共通クラス
- **`actuator_pd.py`** / **`actuator_pd_cfg.py`**: PD（比例微分）制御アクチュエータ
- **`actuator_net.py`** / **`actuator_net_cfg.py`**: ニューラルネットワークベースのアクチュエータ（学習可能なアクチュエータモデル）

#### `assets/` - アセット

- **`asset_base.py`** / **`asset_base_cfg.py`**: アセットの基底クラスと設定

**`articulation/`** - 関節ロボット:
- **`articulation.py`**: 関節ロボットの実装（関節位置、速度、トルクの管理）
- **`articulation_cfg.py`**: 関節ロボットの設定
- **`articulation_data.py`**: 関節ロボットのデータ構造（状態、速度、トルクなど）

**`rigid_object/`** - 剛体オブジェクト:
- **`rigid_object.py`**: 剛体オブジェクトの実装
- **`rigid_object_cfg.py`**: 剛体オブジェクトの設定
- **`rigid_object_data.py`**: 剛体オブジェクトのデータ構造（位置、姿勢、速度など）

**`rigid_object_collection/`** - 剛体オブジェクトコレクション:
- **`rigid_object_collection.py`**: 複数の剛体オブジェクトを管理するコレクション
- **`rigid_object_collection_cfg.py`**: コレクションの設定
- **`rigid_object_collection_data.py`**: コレクションのデータ構造

**`deformable_object/`** - 変形可能オブジェクト:
- **`deformable_object.py`**: 変形可能オブジェクトの実装（布、ロープなど）
- **`deformable_object_cfg.py`**: 変形可能オブジェクトの設定
- **`deformable_object_data.py`**: 変形可能オブジェクトのデータ構造

**`surface_gripper/`** - サーフェスグリッパー:
- **`surface_gripper.py`**: サーフェスグリッパーの実装（吸着式グリッパー）
- **`surface_gripper_cfg.py`**: サーフェスグリッパーの設定

#### `controllers/` - 制御器

- **`differential_ik.py`** / **`differential_ik_cfg.py`**: 差分逆運動学（Differential IK）制御器
- **`operational_space.py`** / **`operational_space_cfg.py`**: 操作空間制御（Operational Space Control）
- **`joint_impedance.py`**: 関節インピーダンス制御
- **`rmp_flow.py`**: RMP Flow（Riemannian Motion Policy）制御器
- **`utils.py`**: 制御器のユーティリティ関数

**`pink_ik/`** - PINK IK制御器:
- **`pink_ik.py`**: PINK IK（Position-based Inverse Kinematics）の実装
- **`pink_ik_cfg.py`**: PINK IKの設定
- **`pink_kinematics_configuration.py`**: 運動学設定
- **`local_frame_task.py`**: ローカルフレームタスク
- **`null_space_posture_task.py`**: ヌル空間姿勢タスク

**`config/`** - 制御器設定:
- **`rmp_flow.py`**: RMP Flowの設定ファイル
- **`data/`**: 制御器のデータファイル（URDFなど）

#### `devices/` - 入力デバイス

- **`device_base.py`**: デバイスの基底クラス
- **`retargeter_base.py`**: リターゲッター（デバイス入力をロボット動作に変換）の基底クラス
- **`teleop_device_factory.py`**: テレオペレーションデバイスのファクトリー

**`spacemouse/`** - SpaceMouseデバイス:
- **`se2_spacemouse.py`**: SE(2)空間でのSpaceMouse（2D移動）
- **`se3_spacemouse.py`**: SE(3)空間でのSpaceMouse（3D移動・回転）
- **`utils.py`**: SpaceMouseのユーティリティ

**`gamepad/`** - ゲームパッド:
- **`se2_gamepad.py`**: SE(2)空間でのゲームパッド
- **`se3_gamepad.py`**: SE(3)空間でのゲームパッド

**`keyboard/`** - キーボード:
- **`se2_keyboard.py`**: SE(2)空間でのキーボード入力
- **`se3_keyboard.py`**: SE(3)空間でのキーボード入力

**`haply/`** - Haplyデバイス:
- **`se3_haply.py`**: SE(3)空間でのHaplyデバイス

**`openxr/`** - OpenXR（VR/AR）:
- **`openxr_device.py`**: OpenXRデバイスの実装
- **`xr_cfg.py`**: OpenXR設定
- **`xr_anchor_utils.py`**: XRアンカーのユーティリティ
- **`manus_vive.py`**: Manus VRグローブの統合
- **`manus_vive_utils.py`**: Manus Viveのユーティリティ
- **`common.py`: 共通ユーティリティ
- **`retargeters/`**: 様々なロボット用のリターゲッター（Franka、Shadow Handなど）

#### `envs/` - 環境クラス

- **`common.py`**: 環境の共通機能
- **`manager_based_env.py`** / **`manager_based_env_cfg.py`**: マネージャーベース環境（推奨、高レベルAPI）
- **`manager_based_rl_env.py`** / **`manager_based_rl_env_cfg.py`**: マネージャーベースRL環境
- **`manager_based_rl_mimic_env.py`**: マネージャーベース模倣学習環境
- **`direct_rl_env.py`** / **`direct_rl_env_cfg.py`**: 直接RL環境（低レベルAPI）
- **`direct_marl_env.py`** / **`direct_marl_env_cfg.py`**: 直接マルチエージェントRL環境
- **`mimic_env_cfg.py`**: 模倣学習環境設定

**`mdp/`** - MDP（Markov Decision Process）関数:
- **`__init__.py`**: MDP関数のエクスポート
- **`observations.py`**: 観測関数（位置、速度、センサー読み取りなど）
- **`rewards.py`**: 報酬関数（距離、速度、接触など）
- **`terminations.py`**: 終了条件関数（タイムアウト、失敗条件など）
- **`events.py`**: イベント関数（リセット時の処理など）
- **`curriculums.py`**: カリキュラム学習関数

**`actions/`** - アクション項:
- **`actions_cfg.py`**: アクション設定クラス
- **`joint_actions.py`**: 関節アクション（位置、速度、トルク）
- **`binary_joint_actions.py`**: バイナリ関節アクション
- **`joint_actions_to_limits.py`**: 関節限界へのアクション
- **`task_space_actions.py`**: タスク空間アクション（エンドエフェクタ制御）
- **`non_holonomic_actions.py`**: 非ホロノミックアクション（車輪ロボットなど）
- **`surface_gripper_actions.py`**: サーフェスグリッパーアクション
- **`pink_actions_cfg.py`**: PINK IKアクション設定
- **`pink_task_space_actions.py`**: PINK IKタスク空間アクション
- **`rmpflow_actions_cfg.py`**: RMP Flowアクション設定
- **`rmpflow_task_space_actions.py`**: RMP Flowタスク空間アクション

**`commands/`** - コマンド生成:
- **`commands_cfg.py`**: コマンド設定クラス
- **`null_command.py`**: ヌルコマンド（コマンドなし）
- **`velocity_command.py`**: 速度コマンド（一様分布、正規分布）
- **`pose_command.py`**: 姿勢コマンド（一様分布）
- **`pose_2d_command.py`**: 2D姿勢コマンド（地形ベース、一様分布）

**`recorders/`** - 記録機能:
- **`recorders.py`**: 記録器の実装
- **`recorders_cfg.py`**: 記録器の設定

**`utils/`** - 環境ユーティリティ:
- **`io_descriptors.py`**: I/O記述子（観測・アクション空間の定義）
- **`spaces.py`**: Gym空間の定義
- **`marl.py`**: マルチエージェントRLユーティリティ

**`ui/`** - UI関連:
- **`ui_widget_wrapper.py`**: UIウィジェットラッパー
- **`manager_live_visualizer.py`**: マネージャーのライブ可視化
- **`line_plot.py`**: ラインプロット
- **`image_plot.py`**: 画像プロット

#### `managers/` - マネージャー

- **`manager_base.py`**: マネージャーの基底クラス
- **`manager_term_cfg.py`**: マネージャー項の設定クラス
- **`scene_entity_cfg.py`**: シーンエンティティの設定（ロボット、オブジェクトの参照）

**観測マネージャー:**
- **`observation_manager.py`**: 観測の計算と管理（観測項の連結、履歴管理など）

**報酬マネージャー:**
- **`reward_manager.py`**: 報酬の計算と管理（報酬項の合計、重み付けなど）

**終了条件マネージャー:**
- **`termination_manager.py`**: 終了条件の判定と管理

**アクションマネージャー:**
- **`action_manager.py`**: アクションの処理と管理（アクション項の処理、スケーリングなど）

**コマンドマネージャー:**
- **`command_manager.py`**: コマンドの生成と管理（速度、姿勢コマンドなど）

**イベントマネージャー:**
- **`event_manager.py`**: イベントの処理と管理（リセット時の処理など）

**カリキュラムマネージャー:**
- **`curriculum_manager.py`**: カリキュラム学習の管理（難易度の段階的増加）

**記録マネージャー:**
- **`recorder_manager.py`**: デモンストレーションの記録管理

#### `sensors/` - センサー

- **`sensor_base.py`** / **`sensor_base_cfg.py`**: センサーの基底クラスと設定

**`camera/`** - カメラ:
- **`camera.py`**: カメラセンサーの実装（RGB、深度、セグメンテーション）
- **`camera_cfg.py`**: カメラ設定
- **`camera_data.py`**: カメラデータ構造
- **`tiled_camera.py`** / **`tiled_camera_cfg.py`**: タイルカメラ（複数カメラの統合）
- **`utils.py`**: カメラユーティリティ

**`ray_caster/`** - レイキャスター:
- **`ray_caster.py`**: レイキャスターの実装（距離測定、衝突検出）
- **`ray_caster_cfg.py`**: レイキャスター設定
- **`ray_caster_data.py`**: レイキャスターデータ構造
- **`ray_caster_camera.py`** / **`ray_caster_camera_cfg.py`**: レイキャスターカメラ（LiDAR風）
- **`patterns/`**: レイキャスターのパターン（グリッド、円形など）

**`imu/`** - IMU:
- **`imu.py`**: IMU（慣性測定ユニット）の実装
- **`imu_cfg.py`**: IMU設定
- **`imu_data.py`**: IMUデータ構造（加速度、角速度、磁場）

**`contact_sensor/`** - 接触センサー:
- **`contact_sensor.py`**: 接触センサーの実装
- **`contact_sensor_cfg.py`**: 接触センサー設定
- **`contact_sensor_data.py`**: 接触センサーデータ構造（力、トルク）

**`frame_transformer/`** - フレーム変換:
- **`frame_transformer.py`**: フレーム変換センサー（座標系変換）
- **`frame_transformer_cfg.py`**: フレーム変換設定
- **`frame_transformer_data.py`**: フレーム変換データ構造

#### `sim/` - シミュレーション

- **`simulation_context.py`**: シミュレーションコンテキスト（物理エンジン、ステージ管理）
- **`simulation_cfg.py`**: シミュレーション設定（物理エンジン、タイムステップなど）

**`spawners/`** - スポーナー（オブジェクト生成）:
- **`spawner_cfg.py`**: スポーナー設定
- **`shapes/`**: 形状のスポーン（ボックス、スフィア、カプセルなど）
- **`meshes/`**: メッシュのスポーン
- **`materials/`**: マテリアルのスポーン（視覚的、物理的マテリアル）
- **`lights/`**: ライトのスポーン
- **`sensors/`**: センサーのスポーン
- **`from_files/`**: ファイルからのスポーン（USD、URDF、MJCFなど）
- **`wrappers/`**: スポーナーラッパー（複数オブジェクトの生成など）

**`converters/`** - コンバーター:
- **`asset_converter_base.py`** / **`asset_converter_base_cfg.py`**: アセットコンバーターの基底クラス
- **`urdf_converter.py`** / **`urdf_converter_cfg.py`**: URDFからUSDへの変換
- **`mjcf_converter.py`** / **`mjcf_converter_cfg.py`**: MJCFからUSDへの変換
- **`mesh_converter.py`** / **`mesh_converter_cfg.py`**: メッシュ形式の変換

**`schemas/`** - スキーマ:
- **`schemas.py`**: USDスキーマの定義
- **`schemas_cfg.py`**: スキーマ設定

**`utils/`** - シミュレーションユーティリティ:
- **`utils.py`**: シミュレーションのユーティリティ関数
- **`stage.py`**: ステージ（シーン）管理
- **`nucleus.py`**: Omniverse Nucleusサーバーとの通信

#### `terrains/` - 地形生成

- **`terrain_generator.py`** / **`terrain_generator_cfg.py`**: 地形生成器の基底クラス
- **`sub_terrain_cfg.py`**: サブ地形の設定
- **`terrain_importer.py`** / **`terrain_importer_cfg.py`**: 地形インポーター（既存地形の読み込み）
- **`utils.py`**: 地形のユーティリティ関数

**`height_field/`** - 高さフィールド地形:
- **`hf_terrains.py`**: 高さフィールド地形の実装
- **`hf_terrains_cfg.py`**: 高さフィールド地形の設定
- **`utils.py`**: 高さフィールド地形のユーティリティ

**`trimesh/`** - メッシュ地形:
- **`mesh_terrains.py`**: メッシュ地形の実装
- **`mesh_terrains_cfg.py`**: メッシュ地形の設定
- **`utils.py`**: メッシュ地形のユーティリティ

**`config/`** - 地形設定:
- **`rough.py`**: 粗い地形の設定例

#### `utils/` - ユーティリティ

- **`__init__.py`**: ユーティリティのエクスポート
- **`configclass.py`**: 設定クラスのデコレータ（`@configclass`）
- **`math.py`**: 数学ユーティリティ（回転、変換など）
- **`array.py`**: 配列操作ユーティリティ
- **`dict.py`**: 辞書操作ユーティリティ
- **`string.py`**: 文字列操作ユーティリティ
- **`types.py`**: 型定義
- **`seed.py`**: 乱数シード管理
- **`version.py`**: バージョン管理
- **`timer.py`**: タイマー（パフォーマンス測定）
- **`assets.py`**: アセット関連ユーティリティ
- **`sensors.py`**: センサー関連ユーティリティ
- **`pretrained_checkpoint.py`**: 事前訓練済みチェックポイントの管理

**`buffers/`** - バッファ:
- **`circular_buffer.py`**: 循環バッファ（履歴管理）
- **`delay_buffer.py`**: 遅延バッファ
- **`timestamped_buffer.py`**: タイムスタンプ付きバッファ

**`datasets/`** - データセット:
- **`dataset_file_handler_base.py`**: データセットファイルハンドラーの基底クラス
- **`hdf5_dataset_file_handler.py`**: HDF5データセットファイルハンドラー
- **`episode_data.py`**: エピソードデータ構造

**`io/`** - I/O:
- **`yaml.py`**: YAMLファイルの読み書き
- **`torchscript.py`**: TorchScriptの読み書き

**`interpolation/`** - 補間:
- **`linear_interpolation.py`**: 線形補間

**`modifiers/`** - モディファイア:
- **`modifier_base.py`**: モディファイアの基底クラス
- **`modifier.py`**: モディファイアの実装（スケーリング、クリッピングなど）
- **`modifier_cfg.py`**: モディファイア設定

**`noise/`** - ノイズ:
- **`noise_model.py`**: ノイズモデルの実装
- **`noise_cfg.py`**: ノイズ設定

**`warp/`** - Warp（GPU計算）:
- **`ops.py`**: Warp演算
- **`kernels.py`**: Warpカーネル

#### `app/` - アプリケーション

- **`app_launcher.py`**: Isaac Simアプリケーションの起動と設定

#### `scene/` - シーン

- **`interactive_scene.py`** / **`interactive_scene_cfg.py`**: インタラクティブシーンの管理（ロボット、オブジェクト、センサーの統合）

#### `markers/` - マーカー

- **`visualization_markers.py`**: 可視化マーカー（デバッグ用の視覚的表示）
- **`config/`**: マーカー設定

### 2. isaaclab_assets（アセット拡張機能）

事前設定されたロボットとオブジェクトのアセット設定を含む拡張機能。

**主要ファイル:**

- **`__init__.py`**: パッケージの初期化とアセットパスの定義

**主要モジュール:**

#### `robots/` - ロボット設定

各ロボットの設定ファイル（USDパス、関節名、アクチュエータ設定など）を含む。

**主要ロボット:**

- **`franka.py`**: Franka Panda（7-DOFアーム）
- **`anymal.py`**: ANYbotics ANYmal（四足歩行ロボット）
- **`spot.py`**: Boston Dynamics Spot（四足歩行ロボット）
- **`unitree_g1.py`**: Unitree G1（ヒューマノイド）
- **`unitree_h1.py`**: Unitree H1（ヒューマノイド）
- **`unitree_go1.py`**: Unitree Go1（四足歩行ロボット）
- **`unitree_go2.py`**: Unitree Go2（四足歩行ロボット）
- **`unitree_a1.py`**: Unitree A1（四足歩行ロボット）
- **`cassie.py`**: Agility Robotics Cassie（二足歩行ロボット）
- **`digit.py`**: Agility Robotics Digit（二足歩行ロボット）
- **`shadow_hand.py`**: Shadow Hand（多指ハンド）
- **`allegro_hand.py`**: Allegro Hand（多指ハンド）
- **その他多数**

各ロボットファイルには以下が含まれます：
- USDファイルパス（Omniverse Nucleusまたはローカル）
- 関節名のリスト
- アクチュエータ設定
- 初期姿勢
- 物理パラメータ

#### `sensors/` - センサー設定

ロボットに取り付けられるセンサーの設定を含む。

**データ構造:**

アセットは通常、Omniverse Nucleusサーバーに保存されますが、ローカル開発用に`data/`ディレクトリも使用できます：

```
data/
├── Robots/<Company-Name>/<Robot-Name>/
│   └── <robot-name>.usd
├── Props/<Prop-Type>/<Prop-Name>/
│   └── <prop-name>.usd
├── ActuatorNets/<Company-Name>/
│   └── <actuator-name>.usd
└── Policies/<Task-Name>/
    └── policy.pt
```

### 3. isaaclab_tasks（タスク拡張機能）

事前設定された環境（タスク）を含む拡張機能。30以上の訓練可能な環境を提供します。

**主要ファイル:**

- **`__init__.py`**: パッケージの初期化とGym環境の登録
- **`utils/`**: タスク共通のユーティリティ
  - **`hydra.py`**: Hydra設定のユーティリティ
  - **`importer.py`**: 動的インポートのユーティリティ
  - **`parse_cfg.py`**: 設定パースのユーティリティ

**主要ディレクトリ:**

#### `direct/` - 直接RL環境（低レベルAPI）

単一ファイルで実装された直接RL環境。低レベルAPIを使用。

**主要タスク:**

- **`ant/`**: Antロコモーション
  - `ant_env.py`: Ant環境の実装
  - `ant_env_cfg.py`: Ant環境の設定
  - `agents/`: エージェント設定（RSL-RL、RL Games、SKRLなど）

- **`cartpole/`**: カートポール
  - `cartpole_env.py`: カートポール環境の実装
  - `cartpole_camera_env.py`: カメラ観測付きカートポール環境
  - `agents/`: エージェント設定

- **`humanoid/`**: ヒューマノイド
  - `humanoid_env.py`: ヒューマノイド環境の実装
  - `agents/`: エージェント設定

- **`allegro_hand/`**: Allegro Hand（多指ハンド）
  - `allegro_hand_env_cfg.py`: Allegro Hand環境の設定
  - `agents/`: エージェント設定

- **`shadow_hand/`**: Shadow Hand（多指ハンド）
  - `shadow_hand_env_cfg.py`: Shadow Hand環境の設定
  - `shadow_hand_vision_env.py`: ビジョン観測付きShadow Hand環境
  - `feature_extractor.py`: 特徴抽出器
  - `agents/`: エージェント設定

- **`franka_cabinet/`**: Franka Pandaによるキャビネット操作
  - `franka_cabinet_env.py`: キャビネット操作環境の実装
  - `agents/`: エージェント設定

- **`quadcopter/`**: クアッドコプター
  - `quadcopter_env.py`: クアッドコプター環境の実装
  - `agents/`: エージェント設定

- **`automate/`**: 自動化タスク（組み立て・分解）
  - `assembly_env.py` / `assembly_env_cfg.py`: 組み立て環境
  - `disassembly_env.py` / `disassembly_env_cfg.py`: 分解環境
  - `assembly_tasks_cfg.py` / `disassembly_tasks_cfg.py`: タスク設定
  - `automate_algo_utils.py`: アルゴリズムユーティリティ
  - `automate_log_utils.py`: ログユーティリティ
  - `factory_control.py`: ファクトリー制御
  - `industreal_algo_utils.py`: IndustRealアルゴリズムユーティリティ
  - `soft_dtw_cuda.py`: Soft DTW（CUDA実装）
  - `run_w_id.py` / `run_disassembly_w_id.py`: ID付き実行スクリプト

- **`factory/`**: ファクトリータスク
  - `factory_env.py` / `factory_env_cfg.py`: ファクトリー環境
  - `factory_tasks_cfg.py`: タスク設定
  - `factory_control.py`: ファクトリー制御
  - `factory_utils.py`: ファクトリーユーティリティ

- **`forge/`**: フォージタスク（ねじ締めなど）
  - `forge_env.py` / `forge_env_cfg.py`: フォージ環境
  - `forge_tasks_cfg.py`: タスク設定
  - `forge_events.py`: フォージイベント
  - `forge_utils.py`: フォージユーティリティ

- **`humanoid_amp/`**: ヒューマノイドAMP（Adversarial Motion Priors）
  - `humanoid_amp_env.py` / `humanoid_amp_env_cfg.py`: AMP環境
  - `motions/`: モーションデータ（.npzファイル）
  - `agents/`: エージェント設定

- **`cart_double_pendulum/`**: カート二重振り子
  - `cart_double_pendulum_env.py`: カート二重振り子環境の実装
  - `agents/`: エージェント設定（マルチエージェント対応）

- **`cartpole_showcase/`**: カートポールショーケース（様々なバリエーション）
  - `cartpole/`: 標準カートポール
  - `cartpole_camera/`: カメラ観測付きカートポール

#### `manager_based/` - マネージャーベース環境（推奨、高レベルAPI）

マネージャーシステムを使用した高レベルAPI環境。観測、報酬、終了条件などをマネージャーで管理。

**`classic/`** - クラシックなRLタスク:

- **`ant/`**: Antロコモーション
  - `ant_env_cfg.py`: Ant環境の設定
  - `mdp/`: MDP定義（観測、報酬、終了条件）
  - `agents/`: エージェント設定

- **`cartpole/`**: カートポール
  - `cartpole_env_cfg.py`: カートポール環境の設定
  - `cartpole_camera_env_cfg.py`: カメラ観測付きカートポール環境の設定
  - `mdp/`: MDP定義
    - `observations.py`: 観測関数
    - `rewards.py`: 報酬関数
    - `symmetry.py`: 対称性の処理
  - `agents/`: エージェント設定

- **`humanoid/`**: ヒューマノイド
  - `humanoid_env_cfg.py`: ヒューマノイド環境の設定
  - `mdp/`: MDP定義
  - `agents/`: エージェント設定

**`locomotion/`** - ロコモーションタスク:

- **`velocity/`**: 速度追従ロコモーション
  - `velocity_env_cfg.py`: 速度追従環境の設定
  - **`config/`**: ロボット設定
    - **`a1/`**: Unitree A1
      - `a1_env_cfg.py`: A1環境設定
      - `agents/`: エージェント設定
    - **`anymal_b/`**: ANYbotics ANYmal-B
    - **`anymal_c/`**: ANYbotics ANYmal-C
    - **`anymal_d/`**: ANYbotics ANYmal-D
    - **`cassie/`**: Agility Robotics Cassie
    - **`digit/`**: Agility Robotics Digit
    - **`g1/`**: Unitree G1
    - **`go1/`**: Unitree Go1
    - **`go2/`**: Unitree Go2
    - **`h1/`**: Unitree H1
    - **`spot/`**: Boston Dynamics Spot
  - **`mdp/`**: MDP定義
    - `rewards.py`: 報酬関数（速度追従、姿勢維持など）
    - `curriculums.py`: カリキュラム学習（難易度の段階的増加）
    - `terminations.py`: 終了条件（転倒、タイムアウトなど）
    - `symmetry/`: 対称性の処理（左右対称ロボット用）

**`manipulation/`** - マニピュレーションタスク:

- **`cabinet/`**: キャビネット操作
  - `cabinet_env_cfg.py`: キャビネット環境の設定
  - **`config/franka/`**: Franka Panda設定
    - `franka_cabinet_env_cfg.py`: Frankaキャビネット環境設定
    - `agents/`: エージェント設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数（関節状態、エンドエフェクタ位置、キャビネット状態など）
    - `rewards.py`: 報酬関数（キャビネットの開閉進捗など）

- **`lift/`**: 物体を持ち上げる
  - `lift_env_cfg.py`: リフト環境の設定
  - **`config/franka/`**: Franka Panda設定
    - `franka_lift_env_cfg.py`: Frankaリフト環境設定
    - `agents/`: エージェント設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数
    - `rewards.py`: 報酬関数（物体の高さ、把持状態など）
    - `terminations.py`: 終了条件

- **`reach/`**: リーチング（エンドエフェクタを目標位置に移動）
  - `reach_env_cfg.py`: リーチ環境の設定
  - **`config/franka/`**: Franka Panda設定
  - **`config/ur_10/`**: Universal Robots UR10設定
  - **`mdp/`**: MDP定義
    - `rewards.py`: 報酬関数（目標位置への距離など）

- **`stack/`**: キューブスタッキング
  - `stack_env_cfg.py`: スタック環境の設定
  - `stack_instance_randomize_env_cfg.py`: インスタンスランダマイゼーション付きスタック環境
  - **`config/franka/`**: Franka Panda設定
    - `stack_joint_pos_env_cfg.py`: 関節位置制御スタック環境
    - `agents/`: エージェント設定
  - **`config/galbot/`**: Galbot設定
  - **`config/ur10_gripper/`**: UR10グリッパー設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数（キューブ位置、姿勢、エンドエフェクタ状態など）
    - `rewards.py`: 報酬関数（スタッキング進捗など）
    - `terminations.py`: 終了条件
    - `franka_stack_events.py`: Frankaスタックイベント（リセット時の処理など）

- **`pick_place/`**: ピックアンドプレース
  - `pickplace_gr1t2_env_cfg.py`: Unitree GR1T2ピックアンドプレース環境
  - `pickplace_gr1t2_waist_enabled_env_cfg.py`: 腰関節有効GR1T2環境
  - `pickplace_unitree_g1_inspire_hand_env_cfg.py`: Unitree G1 Inspire Hand環境
  - `exhaustpipe_gr1t2_base_env_cfg.py`: 排気パイプGR1T2環境
  - `exhaustpipe_gr1t2_pink_ik_env_cfg.py`: PINK IK排気パイプ環境
  - `nutpour_gr1t2_base_env_cfg.py`: ナット注ぎGR1T2環境
  - `nutpour_gr1t2_pink_ik_env_cfg.py`: PINK IKナット注ぎ環境
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数
    - `pick_place_events.py`: ピックアンドプレースイベント
    - `terminations.py`: 終了条件
  - **`agents/robomimic/`**: RoboMimicエージェント設定

- **`inhand/`**: インハンドマニピュレーション
  - `inhand_env_cfg.py`: インハンド環境の設定
  - **`config/allegro_hand/`**: Allegro Hand設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数
    - `rewards.py`: 報酬関数
    - `terminations.py`: 終了条件
    - `commands/`: コマンド生成

- **`dexsuite/`**: デクスタス操作タスク（複雑な操作タスク）
  - `dexsuite_env_cfg.py`: デクスタス環境の設定
  - **`config/kuka_allegro/`**: KUKA + Allegro Hand設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数
    - `rewards.py`: 報酬関数
    - `terminations.py`: 終了条件
    - `curriculums.py`: カリキュラム学習
    - `commands/`: コマンド生成
    - `utils.py`: ユーティリティ
  - `adr_curriculum.py`: ADR（Automatic Domain Randomization）カリキュラム

- **`place/`**: プレースタスク
  - **`config/agibot/`**: Agibot設定
  - **`mdp/`**: MDP定義
    - `observations.py`: 観測関数
    - `terminations.py`: 終了条件

- **`deploy/`**: デプロイタスク
  - **`reach/`**: リーチングデプロイ
    - `reach_env_cfg.py`: リーチ環境設定
    - **`config/`**: ロボット設定
  - **`mdp/`**: MDP定義
    - `rewards.py`: 報酬関数

**`locomanipulation/`** - ロコマニピュレーション（移動しながら操作）:

- **`pick_place/`**: 移動しながらピックアンドプレース
  - `locomanipulation_g1_env_cfg.py`: Unitree G1ロコマニピュレーション環境
  - `fixed_base_upper_body_ik_g1_env_cfg.py`: 固定ベース上半身IK G1環境
  - **`configs/`**: 設定ファイル
    - `action_cfg.py`: アクション設定
    - `agile_locomotion_observation_cfg.py`: アジャイルロコモーション観測設定
    - `pink_controller_cfg.py`: PINK IK制御器設定
  - **`mdp/`**: MDP定義
    - `actions.py`: アクション関数
    - `observations.py`: 観測関数

- **`tracking/`**: トラッキングタスク
  - **`config/digit/`**: Digitロボット設定
    - `digit_tracking_env_cfg.py`: Digitトラッキング環境設定

**`navigation/`** - ナビゲーションタスク:

- **`config/anymal_c/`**: ANYmal-C設定
  - `navigation_env_cfg.py`: ナビゲーション環境設定
  - `agents/`: エージェント設定
- **`mdp/`**: MDP定義
  - `rewards.py`: 報酬関数
  - `pre_trained_policy_action.py`: 事前訓練済みポリシーアクション

**各タスクの構造:**

```
<task-name>/
├── config/              # ロボット固有の設定
│   └── <robot-name>/    # ロボット別設定
│       ├── <robot>_<task>_env_cfg.py  # 環境設定
│       └── agents/      # エージェント設定
│           ├── rsl_rl_ppo_cfg.py
│           ├── rl_games_ppo_cfg.yaml
│           └── skrl_ppo_cfg.yaml
├── mdp/                 # MDP定義
│   ├── observations.py  # 観測関数
│   ├── rewards.py       # 報酬関数
│   ├── terminations.py # 終了条件
│   ├── events.py       # イベント（リセット時など）
│   ├── curriculums.py  # カリキュラム学習
│   └── commands/       # コマンド生成
└── <task>_env_cfg.py    # 環境設定クラス（基底）
```

### 4. isaaclab_mimic（模倣学習拡張機能）

模倣学習用のデータ生成APIと環境を含む拡張機能。

**主要ファイル:**

- **`__init__.py`**: パッケージの初期化

**主要モジュール:**

#### `datagen/` - データ生成ユーティリティ

- **`dataset_generator.py`**: データセット生成器の基底クラス
- **`dataset_generator_cfg.py`**: データセット生成器の設定
- **`skill_generator.py`**: スキル生成器（SDGなど）
- **`skill_generator_cfg.py`**: スキル生成器の設定
- **`selection_strategy.py`**: デモンストレーション選択戦略
- **`selection_strategy_cfg.py`**: 選択戦略の設定
- **`utils.py`**: データ生成のユーティリティ関数

#### `envs/` - 模倣学習用環境

模倣学習に特化した環境実装。

- **`mimic_env_cfg.py`**: 模倣学習環境の設定基底クラス
- **`mimic_env.py`**: 模倣学習環境の実装
- **各タスク固有の模倣学習環境**（lift、reach、stackなど）

#### `locomanipulation_sdg/` - ロコマニピュレーションSDG

ロコマニピュレーション（移動しながら操作）用のSkill Diffusion Generator。

- **`sdg_config.py`**: SDG設定
- **`sdg_generator.py`**: SDG生成器の実装
- **`navigation_policy.py`**: ナビゲーションポリシー
- **`manipulation_policy.py`**: マニピュレーションポリシー
- **`skill_composer.py`**: スキルコンポーザー（ナビゲーションとマニピュレーションの統合）

#### `motion_planners/` - モーションプランナー

- **`curobo_planner.py`**: cuRoboプランナーの統合
- **`curobo_planner_cfg.py`**: cuRoboプランナーの設定
- **`planner_base.py`**: プランナーの基底クラス
- **`planner_cfg.py`**: プランナー設定の基底クラス

#### `ui/` - UI関連

- **`datagen_ui.py`**: データ生成UI

### 5. isaaclab_rl（強化学習拡張機能）

様々な強化学習ライブラリとの統合ラッパーを含む拡張機能。

**主要ファイル:**

- **`__init__.py`**: パッケージの初期化

**主要モジュール:**

#### `rsl_rl/` - RSL-RLラッパー

- **`rsl_rl_cfg.py`**: RSL-RL設定クラス
- **`rsl_rl_ppo_cfg.py`**: PPO（Proximal Policy Optimization）設定
- **`rsl_rl_runner.py`**: RSL-RLランナー（訓練ループ）
- **`rsl_rl_data.py`**: RSL-RLデータ構造
- **`rsl_rl_wrapper.py`**: RSL-RL環境ラッパー

#### `rl_games/` - RL Gamesラッパー

- **`rl_games_cfg.py`**: RL Games設定クラス
- **`rl_games_ppo_cfg.py`**: PPO設定
- **`rl_games_runner.py`**: RL Gamesランナー
- **`rl_games_wrapper.py`**: RL Games環境ラッパー

#### `skrl.py` - SKRLラッパー

- SKRLフレームワークとの統合
- 環境ラッパーの実装

#### `sb3.py` - Stable Baselines3ラッパー

- Stable Baselines3フレームワークとの統合
- Gym環境ラッパーの実装

---

## scriptsディレクトリ（スタンドアロンアプリケーション）

`scripts/`ディレクトリには、Isaac Sim環境外で実行可能なPythonスクリプトが含まれています。

### 1. benchmarks/ - ベンチマークスクリプト

フレームワークコンポーネントのパフォーマンス測定スクリプト

- **`benchmark_cameras.py`**: カメラセンサーのパフォーマンス測定（FPS、レイテンシなど）
- **`benchmark_load_robot.py`**: ロボットの読み込み速度測定（USDファイルの読み込み時間など）
- **`benchmark_non_rl.py`**: 非RL環境のパフォーマンス測定（環境ステップ時間など）
- **`benchmark_rlgames.py`**: RL Gamesフレームワークのパフォーマンス測定（訓練速度など）
- **`benchmark_rsl_rl.py`**: RSL-RLフレームワークのパフォーマンス測定（訓練速度など）
- **`utils.py`**: ベンチマークの共通ユーティリティ関数

### 2. demos/ - デモアプリケーション

コアフレームワーク`isaaclab`の機能を紹介するデモアプリケーション

- **`arms.py`**: アームロボット（Franka Pandaなど）のデモ
- **`bipeds.py`**: 二足歩行ロボット（Humanoid、Digitなど）のデモ
- **`quadrupeds.py`**: 四足歩行ロボット（ANYmal、Spot、A1など）のデモ
- **`hands.py`**: ハンドロボット（Shadow Hand、Allegro Handなど）のデモ
- **`quadcopter.py`**: クアッドコプターのデモ
- **`pick_and_place.py`**: ピックアンドプレースタスクのデモ
- **`multi_asset.py`**: 複数アセットの同時操作デモ
- **`procedural_terrain.py`**: 手続き型地形生成のデモ
- **`markers.py`**: 可視化マーカーのデモ
- **`h1_locomotion.py`**: Unitree H1ロコモーションのデモ
- **`haply_teleoperation.py`**: Haplyデバイスを使用したテレオペレーションのデモ
- **`deformables.py`**: 変形可能オブジェクト（布、ロープなど）のデモ

**`sensors/`** - センサーのデモ:
- **`cameras.py`**: カメラセンサー（RGB、深度、セグメンテーション）のデモ
- **`contact_sensor.py`**: 接触センサーのデモ
- **`imu_sensor.py`**: IMU（慣性測定ユニット）センサーのデモ
- **`frame_transformer_sensor.py`**: フレーム変換センサーのデモ
- **`raycaster_sensor.py`**: レイキャスター（LiDAR風）センサーのデモ

### 3. environments/ - 環境実行スクリプト

`isaaclab_tasks`で定義された環境を様々なエージェントで実行するアプリケーション

- **`random_agent.py`**: ランダムポリシーで環境を実行（デバッグ・テスト用）
- **`zero_agent.py`**: ゼロアクション（何もしない）ポリシーで環境を実行
- **`list_envs.py`**: 利用可能な環境の一覧を表示（Gym環境の登録確認）
- **`export_IODescriptors.py`**: 環境のI/O記述子（観測・アクション空間の定義）をエクスポート

**`state_machine/`** - ステートマシンベースのエージェント:
- **`lift_cube_sm.py`**: キューブを持ち上げるステートマシン
- **`lift_teddy_bear.py`**: テディベアを持ち上げるステートマシン
- **`open_cabinet_sm.py`**: キャビネットを開くステートマシン

**`teleoperation/`** - テレオペレーション:
- **`teleop_se3_agent.py`**: SE(3)空間でのテレオペレーションエージェント（SpaceMouse、ゲームパッドなど）

### 4. reinforcement_learning/ - 強化学習スクリプト

様々な強化学習ライブラリを使用した訓練・実行スクリプト

#### `rsl_rl/` - RSL-RL

- **`train.py`**: RSL-RLフレームワークを使用した強化学習の訓練
  - コマンドライン引数: `--task`, `--num_envs`, `--max_iterations`など
  - ビデオ記録、I/O記述子のエクスポート機能
- **`play.py`**: 訓練済みポリシーで環境を実行・評価
- **`cli_args.py`**: RSL-RLのコマンドライン引数定義

#### `rl_games/` - RL Games

- **`train.py`**: RL Gamesフレームワークを使用した強化学習の訓練
- **`play.py`**: 訓練済みポリシーで環境を実行・評価

#### `sb3/` - Stable Baselines3

- **`train.py`**: Stable Baselines3フレームワークを使用した強化学習の訓練
- **`play.py`**: 訓練済みポリシーで環境を実行・評価

#### `skrl/` - SKRL

- **`train.py`**: SKRLフレームワークを使用した強化学習の訓練
- **`play.py`**: 訓練済みポリシーで環境を実行・評価

#### `ray/` - Ray RLlib（分散強化学習）

- **`launch.py`**: Ray RLlibを使用したローカル分散訓練の起動
- **`submit_job.py`**: クラスター（SLURM、PBSなど）への分散訓練ジョブ送信
- **`task_runner.py`**: 分散訓練タスクの実行
- **`tuner.py`**: ハイパーパラメータチューニング（Optunaなど）
- **`util.py`**: Ray関連のユーティリティ関数
- **`wrap_resources.py`**: リソース（CPU、GPU、メモリ）のラッピング
- **`mlflow_to_local_tensorboard.py`**: MLflowログをTensorBoard形式に変換
- **`grok_cluster_with_kubectl.py`**: Kubernetesクラスターでの実行（Grok用）

**`hyperparameter_tuning/`** - ハイパーパラメータチューニング設定:
- **`vision_cfg.py`**: ビジョンベースタスクのハイパーパラメータ設定
- **`vision_cartpole_cfg.py`**: ビジョンCartpoleタスクのハイパーパラメータ設定

**`cluster_configs/`** - クラスター設定:
- **`Dockerfile`**: クラスター実行用のDockerイメージ
- **`google_cloud/kuberay.yaml.jinja`**: Google CloudでのKubeRay設定テンプレート

### 5. imitation_learning/ - 模倣学習スクリプト

模倣学習手法の実装とデータ生成スクリプト

#### `isaaclab_mimic/` - Isaac Lab Mimic拡張機能

- **`generate_dataset.py`**: 模倣学習用データセットの生成
  - cuRoboプランナーやその他のモーションプランナーを使用
  - HDF5形式でデモンストレーションを保存
- **`annotate_demos.py`**: デモンストレーションのアノテーション
  - 成功/失敗のラベル付け
  - タスク固有のメタデータの追加
- **`consolidated_demo.py`**: 複数のデモンストレーションを統合
  - データセットの前処理
  - 品質フィルタリング

#### `robomimic/` - RoboMimicライブラリ統合

- **`train.py`**: RoboMimicライブラリを使用した模倣学習の訓練
  - BC（Behavioral Cloning）、BC-RNN、DAggerなどの手法をサポート
- **`play.py`**: 訓練済み模倣学習ポリシーで環境を実行
- **`robust_eval.py`**: ロバスト性評価
  - 様々な条件（ノイズ、外乱など）での性能評価

#### `locomanipulation_sdg/` - ロコマニピュレーションSDG

- **`generate_data.py`**: ロコマニピュレーション（移動しながら操作）用のデータ生成
  - Skill Diffusion Generator（SDG）を使用
- **`plot_navigation_trajectory.py`**: ナビゲーション軌道の可視化
  - 移動軌道のプロット
  - 操作動作との統合可視化

### 6. tools/ - ツールスクリプト

フレームワークが提供する様々なツールを使用するアプリケーション

- **`record_demos.py`**: デモンストレーションの記録
  - テレオペレーションデバイス（SpaceMouse、キーボードなど）を使用
  - HDF5形式でデモンストレーションを保存
  - コマンドライン引数: `--task`, `--teleop_device`, `--dataset_file`, `--step_hz`, `--num_demos`, `--num_success_steps`など

- **`replay_demos.py`**: 記録されたデモンストレーションの再生
  - HDF5ファイルからデモを読み込み、環境で再生
  - 可視化と検証

- **`convert_urdf.py`**: URDFファイルからUSDファイルへの変換
  - ロボットモデルの変換
  - マテリアルとテクスチャの処理

- **`convert_mjcf.py`**: MJCF（MuJoCo XML）ファイルからUSDファイルへの変換
  - MuJoCoモデルの変換

- **`convert_mesh.py`**: メッシュ形式の変換
  - OBJ、STL、PLYなどの形式間の変換
  - メッシュの最適化

- **`convert_instanceable.py`**: USDファイルをインスタンス化可能な形式に変換
  - パフォーマンス最適化

- **`check_instanceable.py`**: USDファイルがインスタンス化可能かチェック

- **`blender_obj.py`**: Blender OBJファイルの処理

- **`process_meshes_to_obj.py`**: メッシュをOBJ形式に処理

- **`merge_hdf5_datasets.py`**: 複数のHDF5データセットをマージ
  - デモンストレーションデータセットの統合

- **`hdf5_to_mp4.py`**: HDF5デモンストレーションをMP4ビデオに変換
  - 可視化と共有用

- **`mp4_to_hdf5.py`**: MP4ビデオからHDF5デモンストレーションに変換
  - ビデオからのデモ抽出

- **`pretrained_checkpoint.py`**: 事前訓練済みチェックポイントの管理
  - チェックポイントのダウンロード
  - バージョン管理

**`cosmos/`** - COSMOS（Conversational Sim-to-Real Optimization）:
- **`cosmos_prompt_gen.py`**: COSMOSプロンプト生成
  - シミュレーションから実機への転移最適化

**`test/`** - ツールのテスト:
- **`test_cosmos_prompt_gen.py`**: COSMOSプロンプト生成のテスト
- **`test_hdf5_to_mp4.py`**: HDF5からMP4への変換のテスト
- **`test_mp4_to_hdf5.py`**: MP4からHDF5への変換のテスト

### 7. tutorials/ - チュートリアルスクリプト

フレームワークAPIのステップバイステップチュートリアル

#### `00_sim/` - シミュレーションの基本

- **`create_empty.py`**: 空のシミュレーションシーンの作成
- **`launch_app.py`**: Isaac Simアプリケーションの起動方法
- **`spawn_prims.py`**: プリミティブ（ボックス、スフィアなど）の生成
- **`set_rendering_mode.py`**: レンダリングモードの設定（パフォーマンス、品質など）
- **`log_time.py`**: シミュレーション時間の記録と可視化

#### `01_assets/` - アセットの操作

- **`add_new_robot.py`**: 新しいロボットの追加方法
  - USDファイルからの読み込み
  - 設定のカスタマイズ
- **`run_articulation.py`**: 関節ロボットの実行
  - 関節制御
  - 状態の取得
- **`run_rigid_object.py`**: 剛体オブジェクトの実行
  - 物理シミュレーション
  - 衝突検出
- **`run_deformable_object.py`**: 変形可能オブジェクトの実行
  - 布、ロープなどのシミュレーション
- **`run_surface_gripper.py`**: サーフェスグリッパーの実行
  - 吸着式グリッパーの制御

#### `02_scene/` - シーンの作成

- **`create_scene.py`**: インタラクティブシーンの作成
  - ロボット、オブジェクト、センサーの統合
  - シーン管理

#### `03_envs/` - 環境の作成

- **`create_cartpole_base_env.py`**: Cartpole環境の基本作成
  - 環境クラスの実装
  - 観測と報酬の定義
- **`create_cube_base_env.py`**: キューブ操作環境の基本作成
- **`create_quadruped_base_env.py`**: 四足歩行ロボット環境の基本作成
- **`run_cartpole_rl_env.py`**: Cartpole RL環境の実行
  - Gymインターフェース
  - ランダムポリシーでの実行
- **`policy_inference_in_usd.py`**: USD内でのポリシー推論
  - 訓練済みポリシーの読み込み
  - リアルタイム推論

#### `04_sensors/` - センサーの追加

- **`add_sensors_on_robot.py`**: ロボットへのセンサー追加
  - カメラ、IMU、接触センサーの取り付け
- **`run_usd_camera.py`**: USDカメラの実行
  - カメラ設定
  - 画像取得
- **`run_ray_caster_camera.py`**: レイキャスターカメラ（LiDAR風）の実行
- **`run_ray_caster.py`**: レイキャスターの実行
  - 距離測定
  - 衝突検出
- **`run_frame_transformer.py`**: フレーム変換センサーの実行
  - 座標系変換

#### `05_controllers/` - 制御器の使用

- **`run_diff_ik.py`**: 差分逆運動学（Differential IK）の実行
  - エンドエフェクタ制御
- **`run_osc.py`**: 操作空間制御（Operational Space Control）の実行
  - 力制御

### 8. sim2sim_transfer/ - シミュレーション間転移

異なる物理エンジン間でのポリシー転移スクリプト

- **`rsl_rl_transfer.py`**: RSL-RLポリシーの転移
  - Newton → PhysXへの転移
  - 物理パラメータの調整

**`config/`** - 転移設定ファイル:
- **`newton_to_physx_anymal_d.yaml`**: ANYmal-D用のNewton→PhysX転移設定
- **`newton_to_physx_g1.yaml`**: Unitree G1用のNewton→PhysX転移設定
- **`newton_to_physx_go2.yaml`**: Unitree Go2用のNewton→PhysX転移設定
- **`newton_to_physx_h1.yaml`**: Unitree H1用のNewton→PhysX転移設定

---

## toolsディレクトリ

開発・テスト・プロジェクト生成用のツール

- **`conftest.py`**: pytest設定
- **`install_deps.py`**: 依存関係のインストール
- **`run_all_tests.py`**: 全テストの実行
- **`run_train_envs.py`**: 環境の訓練テスト
- **`test_settings.py`**: テスト設定
- **`template/`**: プロジェクトテンプレート生成ツール
  - `generator.py`: テンプレート生成エンジン
  - `templates/`: テンプレートファイル
    - `extension/`: 拡張機能テンプレート
    - `tasks/`: タスクテンプレート
    - `agents/`: エージェント設定テンプレート

---

## docsディレクトリ

Sphinxを使用したドキュメント生成システム

**構造:**

```
docs/
├── conf.py              # Sphinx設定
├── index.rst            # ドキュメントのエントリーポイント
├── Makefile             # ビルドコマンド（Linux）
├── make.bat             # ビルドコマンド（Windows）
├── requirements.txt     # ドキュメント生成に必要な依存関係
├── source/              # ソースドキュメント
│   ├── api/             # APIリファレンス
│   ├── deployment/       # デプロイメントガイド
│   ├── features/        # 機能説明
│   ├── how-to/          # ハウツーガイド
│   ├── overview/        # 概要と概念
│   ├── setup/           # セットアップガイド
│   ├── tutorials/       # チュートリアル
│   └── _static/         # 静的ファイル（画像など）
└── licenses/            # ライセンスファイル
```

---

## dockerディレクトリ

Dockerコンテナとクラスター実行の設定

- **`Dockerfile.base`**: ベースDockerイメージ
- **`Dockerfile.curobo`**: cuRobo用Dockerイメージ
- **`Dockerfile.ros2`**: ROS2用Dockerイメージ
- **`docker-compose.yaml`**: Docker Compose設定
- **`container.py`**: コンテナ管理スクリプト
- **`cluster/`**: クラスター実行スクリプト
  - `submit_job_slurm.sh`: SLURMジョブ送信
  - `submit_job_pbs.sh`: PBSジョブ送信
  - `run_singularity.sh`: Singularity実行

---

## appsディレクトリ

Isaac Simアプリケーション設定（.kitファイル）

- **`isaaclab.python.kit`**: 標準Pythonアプリケーション
- **`isaaclab.python.headless.kit`**: ヘッドレスPythonアプリケーション
- **`isaaclab.python.rendering.kit`**: レンダリング付きPythonアプリケーション
- **`isaaclab.python.headless.rendering.kit`**: ヘッドレスレンダリング付きPythonアプリケーション
- **`isaaclab.python.xr.openxr.kit`**: OpenXR対応アプリケーション
- **`isaaclab.python.xr.openxr.headless.kit`**: ヘッドレスOpenXRアプリケーション
- **`rendering_modes/`**: レンダリングモード設定
  - `performance.kit`: パフォーマンス優先
  - `balanced.kit`: バランス型
  - `quality.kit`: 品質優先
- **`isaacsim_4_5/`**: Isaac Sim 4.5用の設定

---

## その他の重要なディレクトリ

### output/

訓練結果、ログ、チェックポイントなどの出力ディレクトリ（通常は`.gitignore`に含まれる）

### venv/ または daisuke/

仮想環境やユーザー固有の作業ディレクトリ（通常は`.gitignore`に含まれる）

---

## プロジェクト構造の理解

### 拡張機能 vs スタンドアロンアプリケーション

Isaac Labは2つの主要な使用方法をサポートしています：

1. **拡張機能（Extensions）**: Isaac Sim内でロードされるモジュール
   - `source/`ディレクトリに配置
   - `config/extension.toml`が必要
   - 他の拡張機能からインポート可能

2. **スタンドアロンアプリケーション（Standalone Apps）**: 独立して実行されるPythonスクリプト
   - `scripts/`ディレクトリに配置
   - Isaac Sim環境を起動してから実行

### 環境の種類

1. **Manager-Based環境**（推奨）: マネージャーシステムを使用した高レベルAPI
   - 観測、報酬、終了条件などをマネージャーで管理
   - `manager_based_env_cfg.py`で設定

2. **Direct環境**: 低レベルAPIを使用した直接制御
   - より細かい制御が可能
   - `direct_rl_env_cfg.py`で設定

### タスクの構成要素

各タスクは以下の要素で構成されます：

- **環境設定（`*_env_cfg.py`）**: 環境の全体設定
- **MDP定義（`mdp/`）**:
  - `observations.py`: 観測空間の定義
  - `rewards.py`: 報酬関数の定義
  - `terminations.py`: 終了条件の定義
  - `events.py`: イベント（リセット時など）の定義
- **ロボット設定（`config/<robot-name>/`）**: ロボット固有の設定

---

## まとめ

Isaac Labプロジェクトは、以下の主要なコンポーネントで構成されています：

1. **`source/`**: コア拡張機能（isaaclab、isaaclab_assets、isaaclab_tasksなど）
2. **`scripts/`**: スタンドアロンアプリケーション（訓練、実行、ツールなど）
3. **`tools/`**: 開発・テストツール
4. **`docs/`**: ドキュメント生成システム
5. **`docker/`**: コンテナ化とクラスター実行
6. **`apps/`**: Isaac Simアプリケーション設定

この構造により、Isaac Labは柔軟で拡張可能なロボティクス研究フレームワークとして機能します。

