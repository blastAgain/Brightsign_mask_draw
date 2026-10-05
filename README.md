# BS Mask Draw for PC (BrightSign Multi-Gradient Mask Controller)

BrightSignの実機に対して、マルチレイヤー構造の透過マスクや多段階（マルチ）グラデーションデータをリアルタイムに同期送信・自動記憶・ファイルエクスポートできる、プロジェクションマッピング・空間サイネージ演出用の高精度マスクエディターシステムです。

このプログラム、README.mdはChromeのAIモードで作られました。
ファイルをすべて同じフォルダに入れてください。
English version is at the bottom.
---

## 📦 1. 必須ファイルとプロジェクト構成

本システムを運用するにあたり、以下の**必須ファイル**と、現場の運用に合わせた**構成例**を同一フォルダ内にパッキングして使用してください。アプリ起動時にファイルの場所を尋ねられた際は、このフォルダを指定します。

### 🔹 必須ファイル（コアシステム）
* **`brightsign_mask_draw_for_PC.html`** ：PCのブラウザで開き、マスクを直感的に描画・送信・保存するコントローラー。
* **`brightsign_mask_draw_for_BS.html`** ：BrightSign実機に読み込ませ、受信データや書き出しデータを全画面でレンダリングするプレイヤー。

### 🎬 構成パターン例

#### 【例1】 HTMLリアルタイム同期 ＆ HTMLスタンドアローン運用
通信環境が整っている環境、またはHTML5のみで軽量に透過マスクを常時展開したい場合の構成。
* **BrightAuthor:connected用ファイル**：`brightsign_mask_draw_for_PCmasktest01.bpfx`
* **背景用ムービー**：`loopp.mp4`
* **HTML5プレイヤー**：`brightsign_mask_draw_for_BS.html` （実機受信用・オフライン再生用）

#### 【例2】 UDPコマンド切り替え ＆ 透過PNGマスク上乗せ運用
UDPコマンド（`makemaskUDP` / `normalmaskUDP`）を用いて「マスク作成モード」と「本番（デフォルト）通常マスク」を瞬時に切り替えるプロ仕様の構成。
* **BrightAuthor:connected用ファイル**：`brightsign2_masktest01.bpfx`
* **背景用ムービー**：`loopp.mp4`
* **HTML5プレイヤー**：`brightsign_mask_draw_for_brightsign.html`
* **PNGマスク用プレースホルダー**：`trans_pic.png` （最初に読み込む透明な画像。作成した透過PNGマスクと差し替えて本番運用します）

---

## ⚙️ 2. コントローラーの起動方法（3パターンの開き方）

本システムはブラウザのメモリ領域（`localStorage`）を利用し、**同じブラウザ内でもIPアドレスごとに設定データを完全に分離して自動記憶**します。

### ① 基本の開き方（手動運用）
1. `brightsign_mask_draw_for_PC.html` をブラウザで開きます。
2. 画面右上の「BrightSign IP」欄に、制御したい実機のIPアドレス（例: `192.168.1.101`）を入力します。
3. **【重要】** 欄外をクリックするかEnterで入力を確定した瞬間、そのIP専用のデータが自動的にロードされます。

### ② マルチタブ一発起動リンク（URL引数） ★現場推奨
URLの末尾に `?ip=〇〇` の引数を付けてブラウザのブックマークに登録しておくことで、開いた瞬間に専用データがロードされた状態でスタートできます。マルチサイネージの現場で非常に有効です。
* **1台目用タブ**: `http://<サーバーのIP>/brightsign_mask_draw_for_PC.html?ip=192.168.1.101`
* **2台目用タブ**: `http://<サーバーのIP>/brightsign_mask_draw_for_PC.html?ip=192.168.1.102`

### ③ 保存したJSONファイルでの一発復元起動 ★現場推奨
ページのどこにでも、事前にバックアップ保存したJSONファイルを**ドラッグ＆ドロップするだけ**で、その設定を100%復元して一発起動します。別アドレスのJSONを別タブにドロップして複数台を同時に管理・復元することも可能です。

---

## 🎨 3. 画面の操作マニュアル

右側のコントロールパネルを上から順番に操作してマスクを構築します。

* **【STEP 1】解像度の設定（WUXGAディスプレイ対応）**
  * 「対象解像度」セレクトボックスから、現場の機器に合わせて解像度を選択します。
  * `1920x1080 (横)` / `1920x1200 (WUXGA) 🌟` / `3840x2160 (4K)` / `1080x1920 (縦)`
  * 選択すると、左側のプレビューキャンバスが指定のアスペクト比（16:9や16:10など）に一瞬で自動変形します。
* **【STEP 2】オブジェクト（図形）の配置・直感ドラッグ**
  * **編集オブジェクト**: 最大12個（Obj 1〜12）を切り替えて個別編集。
  * **種類**: `なし` / `四角ポリゴン` / `線` から選択。選ぶとプレビューにクッキリ出現します。
  * **操作**: マウスで直接掴んで**手の動きにぴったり吸い付くように滑らかにドラッグ移動**。
  * **レイヤー順序**: 右側のリストをドラッグ＆ドロップして**重ね順（zIndex）を直感的に並び替え可能**。
* **【STEP 3】多段階マルチグラデーションの調整**
  * 種類で「四角ポリゴン」を選択している場合、グレーの「グラデーションバー」の**任意の場所をクリックするだけでカラーマーカー（ポイント）を無制限に追加**でき、赤ボタンで削除可能。色や不透明度（0〜100%）を個別に変更できます。

---

## 💾 4. データ出力・保存・復元コマンド一覧

調整が完了したら、右最下部にあるボタン群でデータを出力・バックアップします。

| ボタン名 / 領域 | 処理内容 | 活用シーン |
| :--- | :--- | :--- |
| **BrightSignへ同期送信** | 入力されたIPのアドレスのローカルネットワークへ、変数の引数 `mask_config_string` としてレイヤー・マルチグラデデータを瞬時にPOST送信します。 | 現場で実機をプロジェクターやモニターに繋ぎ、**リアルタイムでマスクの形を合わせ込むとき。** |
| **設定JSONファイル保存<br>（バックアップ）🌟** | 現在作っている12個のオブジェクトデータと解像度設定をすべて含んだJSONファイルをPCへダウンロード保存します。 | **PCの買い替え、ブラウザ履歴の削除、他PCへのデータ移行**、または過去の別パターン配置にいつでも戻せるよう「安全な型」としてセーブしておきたいとき。 |
| **オフライン用 HTML保存** | 構築したマルチレイヤー構造とCSSグラデーションの数値をそのまま保持した、超軽量な1枚のHTMLファイルを生成します。 | SDカード内に `index.html` として直接放り込み、**【例1】のようにBrightSign単体で透過WEBマスクプレイヤーとしてスタンドアローン動作させるとき。** |
| **透明PNG画像保存** | 作成したマスク配置を指定解像度の原寸大（FHDや4K）の「背景が透明な透過PNG画像」として内部キャンバスで超高精度にレンダリング出力します。 | **【例2】のプレースホルダー画像（`trans_pic.png`）と差し替えたり**、Premiere等に持ち込んで動画自体にマスクを焼き付けるとき。 |
| **💡 JSONファイルドロップ領域** | ページのどこにでも保存したJSONファイルをドラッグ＆ドロップするだけで、過去の設定データを一瞬で画面に100%復元します。 | 保存しておいた過去の現場データや、別のIPで使っていたバックアップデータを**瞬時にロードして再編集したいとき。** |

---

# 🤖 BrightSign実機側の準備・設定マニュアル

BrightSignがコントローラーからのリアルタイム送信（POST通信）を確実に受け付け、かつマスク図形を正確にレンダリングできるように、BrightAuthor / BrightAuthor:connectedで以下のシステム設定を組んでおきます。

### ① ローカルネットワークサーバー（サーバー機能）の有効化
コントローラーはBrightSignの **ポート`8008`** に向けてデータをPOST送信します。実機側のWebサーバー機能を必ずONにしてください。
* **従来のBrightAuthor**: `Presentation Properties` ＞ `Interactive` ＞ `Networking` タブを開き、**`Enable Local Web Server` にチェックを入れ、ポートを `8008`** に設定します。
* **BrightAuthor:connected 🌟**: **ポート8008の設定は不要です**（デフォルトで自動的に最適化されて有効になっているため、設定を無視してスキップしてください）。

### ② 受信用 変数（User Variable）の作成
コントローラーが送信するデータの受け皿となる変数を実機側に登録します。
* `User Variables` メニューを開き、新しく変数を追加します。
  * **Variable Name（変数名）**: **`mask_config_string`** （※1文字でも違うと通信が通りません）
  * **Access (アクセス権限)**: **`private`** に設定します。
  * **Type（種類）**: **`private`** に設定します。

### ③ HTML描画ゾーン（HTML5 Site）の設定
実機側でWEBマスクを描画・再生するゾーン（Layer）を設定します。
* プレゼンテーション内に「HTML5ゾーン」を作成し、必須ファイルである **`brightsign_mask_draw_for_BS.html`** を読み込ませます。
* **最重要プロパティ（※BA従来版のみ設定、connectedには自動最適化されるため項目がありません）**
  * `Optimize Graphics`（グラフィックスの最適化）：**必ずチェックを入れます（ON）**
  * `Enable Hardware Acceleration`（ハードウェアアクセラレーション）：**必ずチェックを入れます（ON）**
  * `Background transparent`（背景を透明にする）：**必ずチェックを入れます（ON）**（※マスクの下にある背景ムービーを綺麗に透過させるために絶対必須です）

### 📌 【絶対必須】JavaScript Objectの有効化
* HTML5サイト設定画面（HTML5 Site Properties）において、**`Enable BrightSign JavaScript objects` に必ずチェック**を入れてください。
* **有効化しないとどうなるか？**: PCコントローラーから送信したデータ（`mask_config_string`）を実機がネットワーク受信しても、HTMLプレイヤー側が実機内部の変数を読み取ることができず、**「送信は成功しているのに、実機上のマスクの形がピクリとも変わらない」**という現象が発生します。

---

## 🏁 5. 調整が終わったら（最終現場流し込み手順）

PCコントローラー上で完璧にマスクの型・グラデーション調整が終わったら、以下の手順で実機へ本番ファイルを投入し、システムを完全ロックします。

* **【例1（HTMLプレイヤー運用）】の場合**  
  「オフライン用 HTML保存」で書き出したファイルを、実機側のHTML5プレイヤーファイル（`brightsign_mask_draw_for_brightsign.html`）と差し替えてSDカードかネットワーク経由で投入します。
* **【例2（透過PNG画像運用）】の場合**  
  「透明PNG画像保存」で書き出した透過PNGマスクを、最初に読み込ませていたプレースホルダー用の透明画像（**`trans_pic.png`**）と完全に差し替えてSDカードかネットワーク経由で投入します。

---

# BS Mask Draw for PC (BrightSign Multi-Gradient Mask Controller)

This high-precision mask editor system allows you to real-time synchronize, transmit, automatically store, and export multi-layer transparent masks and multi-stage gradient data to BrightSign media players for projection mapping and spatial digital signage installations.

---

## 📦 1. Required Files and Project Configuration

To operate this system, pack the following **required files** and **configuration examples** into the same folder. When prompted for the file location upon launching the app, specify this folder.

### 🔹 Required Files (Core System)
* **`brightsign_mask_draw_for_PC.html`**: The controller opened in a PC browser to intuitively draw, transmit, and save masks.
* **`brightsign_mask_draw_for_BS.html`**: The player loaded into the BrightSign unit to render received data or exported files in full screen.

### 🎬 Configuration Pattern Examples

#### [Example 1] HTML Real-Time Sync & HTML Standalone Operation
Ideal for environments with an established network connection, or when you want to constantly display a lightweight transparent mask using HTML5 only.
* **BrightAuthor:connected File**: `brightsign_mask_draw_for_PCmasktest01.bpfx`
* **Background Video File**: `loopp.mp4`
* **HTML5 Player**: `brightsign_mask_draw_for_brightsign.html` (For real-time receiving and offline playback on the unit)

#### [Example 2] UDP Command Switching & Transparent PNG Mask Overlay Operation
A professional-grade configuration that uses UDP commands (`makemaskUDP` / `normalmaskUDP`) to instantly switch between "Mask Creation Mode" and the "Production (Default) Normal Mask".
* **BrightAuthor:connected File**: `brightsign2_masktest01.bpfx`
* **Background Video File**: `loopp.mp4`
* **HTML5 Player**: `brightsign_mask_draw_for_brightsign.html`
* **PNG Mask Placeholder**: `trans_pic.png` (A blank transparent image loaded initially. Replace this with your generated transparent PNG mask for production)

---

## ⚙️ 2. Controller Launch Methods (3 Patterns)

This system utilizes browser `localStorage` to **completely separate and automatically store configuration data per IP address**, even within the same browser.

### Pattern 1: Basic Launch (Manual Operation)
1. Open `brightsign_mask_draw_for_PC.html` in a web browser.
2. Enter the target BrightSign IP address (e.g., `192.168.1.101`) into the **"BrightSign IP"** field in the upper right.
3. **[Crucial]** The moment you click outside the IP field or press Enter to confirm, the data specific to that IP will load automatically.

### Pattern 2: Quick-Launch via Multi-Tab URL Parameters ★ Highly Recommended for On-Site Use
By adding a `?ip=XX` parameter to the end of the URL and bookmarking it, you can start the page with the specific IP data pre-loaded. This is extremely efficient for multi-signage setups.
* **Tab for Unit 1**: `http://<YOUR_SERVER_IP>/brightsign_mask_draw_for_PC.html?ip=192.168.1.101`
* **Tab for Unit 2**: `http://<YOUR_SERVER_IP>/brightsign_mask_draw_for_PC.html?ip=192.168.1.102`

### Pattern 3: Instant Restoration via JSON File Drag & Drop ★ Highly Recommended for On-Site Use
Simply **drag and drop** a previously exported JSON backup file anywhere onto the page to 100% restore your settings instantly. You can manage and restore multiple units simultaneously by dropping different JSON files into separate browser tabs.

---

## 🎨 3. UI Operation Manual

Operate the right control panel from top to bottom to construct your mask layout.

* **[STEP 1] Target Resolution Settings (WUXGA Display Supported)**
  * Select your display resolution from the drop-down menu to match your site hardware.
  * Options: `1920x1080 (Landscape)` / `1920x1200 (WUXGA) 🌟` / `3840x2160 (4K)` / `1080x1920 (Portrait)`
  * Upon selection, the left preview canvas automatically snaps to the designated aspect ratio (16:9, 16:10, etc.).
* **[STEP 2] Object Placement and Intuitive Dragging**
  * **Edit Object**: Switch and individualize up to 12 objects (`Obj 1–12`).
  * **Type**: Choose from `None` / `Polygon (Rectangle)` / `Line`. Your shape will instantly appear on the canvas.
  * **Operation**: Click and grab shapes directly on the left screen for **smooth, pixel-perfect drag movement following your mouse**.
  * **Layer Order**: Drag and drop items up or down within the right list to **intuitively rearrange the stacking order (zIndex)**.
* **[STEP 3] Multi-Stage Gradient Adjustments**
  * When `Polygon` is selected, **click anywhere on the gray gradient bar to add an unlimited number of color markers (points)**. Select a marker to change its color or opacity (0–100%), or click the red button to delete it.

---

## 💾 4. Output, Save, and Restore Commands

Once adjustments are complete, use the buttons at the bottom right to export or back up your data.

| Button Name / Area | Functionality | On-Site Use Cases |
| :--- | :--- | :--- |
| **BrightSignへ同期送信<br>(Sync to BrightSign)** | Instantly transmits layer and multi-gradient data via POST request to BrightSign's local network (Port `8008`) under the User Variable string `mask_config_string`. | **For real-time mask alignment on-site while projecting onto a canvas or monitor.** |
| **設定JSONファイル保存<br>(Save Config JSON Backup) 🌟** | Downloads a backup **`JSON file`** containing all 12 object datasets and resolution settings to your PC. | **For hardware replacements, clearing browser history, migrating data to other PCs**, or saving a secure template to revert to previous layouts at any time. |
| **オフライン用 HTML保存<br>(Save Offline HTML)** | Generates a single, ultra-lightweight HTML file that completely retains the constructed multi-layer structure and CSS gradient values. | Save directly as `index.html` on the SD card to **run the BrightSign standalone as a transparent web mask player (as shown in [Example 1]).** |
| **透明PNG画像保存<br>(Save Transparent PNG)** | Renders and exports your mask layout as a full-scale transparent background PNG image (FHD or 4K) via the internal canvas. | **To replace the placeholder image (`trans_pic.png`) in [Example 2]**, or to import into video editing software (Premiere/After Effects) to bake the mask onto content. |
| **💡 Drag & Drop Area for JSON Files** | Drag and drop a saved JSON file anywhere on the page to immediately restore previous configuration data to 100%. | **To instantly load and re-edit past site data** or backup data used on another IP address. |

---

# 🤖 BrightSign Hardware Preparation & Setup Manual

To ensure BrightSign correctly receives real-time sync transmissions (POST requests) from the controller and precisely renders mask shapes, apply the following system configurations in BrightAuthor or BrightAuthor:connected.

### 1) Enable Local Network Server (Web Server Feature)
The controller sends data to BrightSign via **Port `8008`**. The unit's local web server feature must be enabled.
* **BrightAuthor (Classic)**: Go to `Presentation Properties` > `Interactive` > `Networking` tab, check **`Enable Local Web Server`**, and set the port to **`8008`**.
* **BrightAuthor:connected 🌟**: **No manual configuration for port 8008 is required** (it is optimized and enabled by default; you can skip this step).

### 2) Create User Variable for Receiving Data
Register the variable name (parameter) that will catch the transmitted data string on the device.
* Open the `User Variables` menu and add a new variable:
  * **Variable Name**: **`mask_config_string`** (Must be exact; case-sensitive)
  * **Access / Type**: Set to **`private`**.

### 3) HTML描画ゾーン (HTML5 Site Layer) Settings
Set up the layer zone within your presentation to play the web mask.
* Create an "HTML5 Zone" in your presentation layout and load the required file: **`brightsign_mask_draw_for_BS.html`**.
* **Critical Properties (Note: Apply to Classic BA only; these are auto-optimized in connected)**
  * `Optimize Graphics`: **Check (ON)**
  * `Enable Hardware Acceleration`: **Check (ON)**
  * `Background transparent`: **Check (ON)** (Absolutely required to see the background video layer beneath the mask)

### 📌 [Mandatory Switch] Enable BrightSign JavaScript Objects
* Within the HTML5 Site configuration window (HTML5 Site Properties), **you must check `Enable BrightSign JavaScript objects`**.
* **What happens if left disabled?**: Even if the device successfully receives the data string (`mask_config_string`) over the network, the HTML player logic will be blocked from reading it. As a result, **the transmission status will report success, but the mask shape on the screen will remain unchanged.**

---

## 🏁 5. Production Workflow (Final Deployment)

Once fine-tuning is completed on the PC controller, deploy the final files to the SD card to lock the system configuration.

* **For [Example 1] (HTML Player Mode)**  
  Take the file generated via "Save Offline HTML", rename it to **`brightsign_mask_draw_for_brightsign.html`**, and overwrite the player file on the SD card.
* **For [Example 2] (Transparent PNG Mode)**  
  Take the transparent mask image generated via "Save Transparent PNG", rename it to **`trans_pic.png`**, and completely replace the initial placeholder image on the SD card.

---

