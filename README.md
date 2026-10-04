# QMK Firmware for ThinkBook 14 G5+ ARP (RP2040 Keyboard Converter)
`./keyboards/converter` に `git clone` して使用すること。

Lenovo ThinkBook 14 G5+ ARP の内蔵キーボードを Raspberry Pi Pico (RP2040) を用いて USB キーボード化するコンバーター用 QMK ファームウェアです。  
Remap (https://remap-keys.app/configure) によるブラウザからのリアルタイムキーマッピング変更、マクロ機能、6レイヤーに対応しています。

---

## 1. ビルド方法 (uvx + Scoop 環境 / PowerShell)

`C:\Users\kmakoto\qmk_firmware` ルートディレクトリから PowerShell でビルドします。

```powershell
$env:PATH = "$env:USERPROFILE\scoop\apps\make\current\bin;$env:USERPROFILE\scoop\apps\gcc-arm-none-eabi\current\bin;$env:USERPROFILE\scoop\shims;$env:PATH"
uvx --with-requirements requirements.txt qmk compile -j 1 -kb converter/thinkbook14_g5plus_arp/rpi_pico -km default
```

ビルド完了後、ルート直下に `converter_thinkbook14_g5plus_arp_rpi_pico_default.uf2` が生成されます。  
Raspberry Pi Pico の BOOTSEL ボタンを押しながら USB 接続し、マウントされたドライブへ上記 `.uf2` をコピーしてください。

---

## 2. USB 仕様および pid.codes (Open Source Hardware) 規約準拠

USB-IF 本家の規格およびオープンソースハードウェアコミュニティ向け公式 USB ID 管理機構である pid.codes（`0x1209`）の規約に準拠しています。

* **Vendor ID (VID)**: `0x1209` (InterBiometrics / pid.codes - Open Source Hardware)
* **Product ID (PID)**: `0x0001` (pid.codes Test PID 1)
* **Manufacturer**: `Makoto KUNO <makomako0829bump@gmail.com>`
* **Product Name**: `ThinkBook 14 G5+ Keyboard Converter`
* **Serial Number**: `makomako0829bump@gmail.com:thinkbook14_g5plus_arp`
  * USB ディスクリプタ（iSerialNumber）にメールアドレスプレフィックス形式のシリアル番号を埋め込んでいます。

---

## 3. キーマップ構成および最上段ホットキー仕様

デフォルトでは、最上段ファンクション列（単体押し / レイヤー0）に以下の機能が割り当てられています（Fn 同時押し時は標準の `F1` 〜 `F12`、`Insert` が入力されます）:

| 位置 | デフォルト機能 | 送信キーコード | 動作説明 |
| :---: | :--- | :--- | :--- |
| **F1** | 音声ミュート | `KC_MUTE` | スピーカーの消音 / 解除 |
| **F2** | 音量ダウン | `KC_VOLD` | マスター音量を下げる |
| **F3** | 音量アップ | `KC_VOLU` | マスター音量を上げる |
| **F4** | **マイクミュート** | `KC_F20` | マイクの消音 / 解除（Linux / Windows / Teams / Zoom 連動） |
| **F5** | 輝度ダウン | `KC_BRID` | 画面の明るさを下げる |
| **F6** | 輝度アップ | `KC_BRIU` | 画面の明るさを上げる |
| **F7** | **ディスプレイ切替** | `LGUI(KC_P)` | Win+P（複製 / 拡張 / 外部出力画面設定） |
| **F8** | **クイック設定 (機内モード等)** | `LGUI(KC_A)` | Win+A（機内モード / Wi-Fi / Bluetooth パネル） |
| **F9** | **設定画面** | `LGUI(KC_I)` | Win+I（Windows / Linux 設定画面の起動） |
| **F10** | **PC 画面ロック** | `LGUI(KC_L)` | Win+L（画面ロック） |
| **F11** | **タスクビュー** | `LGUI(KC_TAB)` | Win+Tab（デスクトップ一覧 / アプリ切替） |
| **F12** | 電卓 | `KC_CALC` | 電卓アプリの起動 |
| **PrtSc左** | Insert | `KC_INS` | Insert キー（Layer 0, 1 ともに有効） |

※FnLock（`Esc` キー位置に割り当てられたカスタムキーコード `FN_LOCK`）を押すことで、単体押しで `F1`〜`F12`、Fn 同時押しでホットキー機能となるモードへ切り替え可能です。

---

## 4. Remap によるキーマップ変更・マクロ設定

本ファームウェアは動的キーマップ 6 レイヤーおよびマクロ機能（Macro 0〜15）に対応しており、Remap 上で直感的にカスタマイズできます。

### 設定用定義ファイル
* **`remap.json`**: 本ディレクトリ直下に同梱されています。

### Remap の利用手順:
1. Google Chrome または Microsoft Edge で **[https://remap-keys.app/configure](https://remap-keys.app/configure)** を開きます。
2. 「START REMAP FOR YOUR KEYBOARD」をクリックし、接続されたコンバーターを選択します。
3. 初回接続時またはレイアウトが表示されない場合は、本フォルダ直下の **`remap.json`** を画面上にドラッグ＆ドロップしてインポートします。
4. キーマップ編集画面が開き、GUI 上でドラッグ＆ドロップによりキーの再配置が即座に反映されます。

### マクロ（Macro）の使い方（Fn＋キーの割り当てなど）:
1. レイヤー 1（Fn レイヤー）の任意のキーに、キーコード `M0` 〜 `M15`（Macro 0〜15）を配置します。
2. Remap の「MACRO」タブを開き、対象マクロ（例: `M0`）に送信したいキーストローク（例: `Ctrl + C`、ショートカット、定型テキスト等）を登録して保存します。
3. 実機で `Fn` を押しながらそのキーを押すことで、登録したマクロが実行されます。

---

## 5. 基板ピン配置および GPIO 仕様

### Matrix Rows (8本)
`GP8` (Row 0), `GP5` (Row 1), `GP0` (Row 2), `GP7` (Row 3), `GP4` (Row 4), `GP11` (Row 5), `GP2` (Row 6), `GP1` (Row 7)

### Matrix Cols (16本)
`GP6` (Col 0), `GP12` (Col 1), `GP10` (Col 2), `GP13` (Col 3), `GP9` (Col 4), `GP16` (Col 5), `GP14` (Col 6), `GP15` (Col 7), `GP22` (Col 8), `GP20` (Col 9), `GP21` (Col 10), `GP19` (Col 11), `GP18` (Col 12), `GP17` (Col 13), `GP3` (Col 14), `GP28` (Col 15)

### インジケータ LED
* **CapsLock LED**: `GP27`（QMK 標準キーボード LED と自動連動）
* **FnLock LED**: `GP26`（カスタムキーコード `FN_LOCK` によるトグル連動）

---

## 6. キーマトリクス交点対応表 (Matrix Crosspoint Table)

8 Rows × 16 Cols のマトリクス交点と割り当てられている物理キーの対応表です。

| Row \ Col | Col 0<br>`GP6` | Col 1<br>`GP12` | Col 2<br>`GP10` | Col 3<br>`GP13` | Col 4<br>`GP9` | Col 5<br>`GP16` | Col 6<br>`GP14` | Col 7<br>`GP15` | Col 8<br>`GP22` | Col 9<br>`GP20` | Col 10<br>`GP21` | Col 11<br>`GP19` | Col 12<br>`GP18` | Col 13<br>`GP17` | Col 14<br>`GP3` | Col 15<br>`GP28` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Row 0**<br>`GP8` | `~ | F1 | F2 | 5% | 6^ | =+ | F8 | -_ | F9 | - | - | - | - | Ctrl | - | - |
| **Row 1**<br>`GP5` | 1! | 2@ | 3# | 4$ | 7& | 8* | 9( | 0) | F10 | - | F11 | F12 | Insert | - | - | - |
| **Row 2**<br>`GP0` | Tab | Caps Lock | F3 | T | Y | ]} | F7 | [{ | Bksp | - | - | - | - | - | Shift | - |
| **Row 3**<br>`GP7` | Q | W | E | R | U | I | O | P | - | - | - | - | Delete | - | - | Left OS |
| **Row 4**<br>`GP4` | A | S | D | F | J | K | L | ;: | \| | Fn | - | - | - | - | - | - |
| **Row 5**<br>`GP11` | Esc | - | F4 | G | H | F6 | - | '" | F5 | Up | - | - | Alt | - | - | - |
| **Row 6**<br>`GP2` | Z | X | C | V | M | ,< | .> | - | Enter | PrtSc | - | - | - | Ctrl | Shift | - |
| **Row 7**<br>`GP1` | - | - | - | B | N | - | - | /? | Space | Left | Down | Right | AltGr | - | - | - |

---

## 7. 配布用アセット（GitHub Releases）
GitHub Release を作成する際は、以下のファイルを一緒に配布することを推奨します:
1. `converter_thinkbook14_g5plus_arp_rpi_pico_default.uf2`（ファームウェア本体）
2. `remap.json`（Remap 用定義ファイル）
