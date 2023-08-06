# SteppterDriverUnit

<img src="https://github.com/akita11/StepperDriverUnit/blob/main/StepperDriverU.jpg" width="240px">

ステッピングモータをGrove端子からの方向・パルスのみで制御できるUNITです。ドライバICはよく使われるDRV8825の互換IC(HR8255)を使用しており、マイクロステップ駆動も可能です。またイネーブル・リセットは別の端子から与えることもできます。

# 使用法（まずは使ってみる）

- 6pコネクタ（ネジ端子：基板裏面にシルク表記あり）に、ステッピングモータの電源(+12V等）とモータのA相・B相(各2本：計4本）のケーブルを接続します。
- Grove端子にマイコンなど(M5Stack等)を接続します（ここからの+5V給電が本体の動作に必要です）
- Grove端子の信号線(DIRとSTP)でモータの回転の向きとステップを制御します。DIR=0/1で回転方向を決定し（実際の回転方向がどちらが時計回りかはA相・B相の配線によって異なる）、STPの立ち上がり（0→1の変化)で1ステップ進みます。

Grove端子のピン配置（基板裏面にシルク表記あり）： STP/DIR/+5V/GND

# 使用法（詳細な設定など）

半固定抵抗（RV1）でモータの駆動電流を設定てきます。また動作異常時にはD1(Fault)が点灯します。


## 補助制御端子

<img src="https://github.com/akita11/StepperDriverUnit/blob/main/StepperDriverU_front.png" width="240px">

- J2の2本をショートすると、ドライバICをdisable状態にしてモータの駆動をOFFにします。
- J3の2本をショートすると、ドライバICの内部状態をリセットします。
- CN2にもこの2本の信号線が引き出してあり、Grove端子をとりつけてマイコン等から制御することができます。このGrove端子のピン配置（基板裏面にシルク表記あり）： nEN/nRST/+5V/GND （nEN=0でドライバICがON、=1でOFF。nRST=0でドライバICをリセット、=1で通常動作）

<img src="https://github.com/akita11/StepperDriverUnit/blob/main/StepperDriverU_appendix.jpg" width="240px">

## 駆動方法の設定

基板裏面の4つのソルダージャンパで、モータの駆動方法を設定できます。詳細はHR8825（またはDRV8825）のデータシートを参照してください。

<img src="https://github.com/akita11/StepperDriverUnit/blob/main/StepperDriverU_back.png" width="240px">

- M0/M1/M2: マイクロステップの設定。（o=ショート、x=開放）

| M0 | M1 | M2 | マイクロステップ |
| -- | -- | -- | -- |
| x  | x  | x  | フルステップ |
| o  | x  | x  | 1/2ステップ |
| x  | o  | x  | 1/4ステップ |
| o  | o  | x  | 8 microステップ |
| x  | x  | o  | 16 microステップ |
| o  | x  | o  | 32 microステップ |
| x  | o  | o  | 32 microステップ |
| o  | o  | o  | 32 microステップ |

- Dcy: Decayモードの設定（G-中央間をショート=slow / V-中央間をショート=fast / ショートなし=mixed）

# Author

Junichi Akita (akita@ifdl.jp / @akita11)
