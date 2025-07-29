# Poetry Camera 詩意相機 🎭📷

A Raspberry Pi-based AI poetry camera that combines gesture recognition, image analysis, and automatic poetry generation. When you make a gesture, the camera automatically takes a photo, analyzes the content, and generates unique poetry, which is then printed through a thermal printer.

一個基於 Raspberry Pi 的 AI 詩意相機，結合手勢識別、影像分析和自動新詩生成功能。當您做出手勢時，相機會自動拍照、分析內容，並生成獨特的新詩，最後通過熱敏印表機列印出來。

## ✨ 功能特色 / Features

- 🤖 **AI 驅動新詩生成 / AI-Driven Poetry Generation**: 使用 OpenAI GPT-4o 分析照片，DeepSeek 生成中文新詩 / Uses OpenAI GPT-4o to analyze photos, DeepSeek to generate Chinese poetry
- ✋ **手勢識別觸發 / Gesture Recognition Trigger**: 支援 OK 手勢和 YA 手勢自動觸發拍照 / Supports OK and YA gestures for automatic photo capture
- 🌐 **雙語介面支援 / Bilingual Interface Support**: 完整的中英文雙語介面，包含所有操作提示和錯誤訊息 / Complete bilingual Chinese-English interface with all operation prompts and error messages
- 🎵 **音階提示音系統 / Musical Prompt System**: 豐富的音樂提示音，包含啟動、倒數、成功等不同音效 / Rich musical prompts including startup, countdown, success sounds
- 🖨️ **自動熱敏列印 / Automatic Thermal Printing**: 詩歌自動列印在 58mm 熱敏紙上 / Poetry automatically printed on 58mm thermal paper
- 📱 **觸控螢幕介面 / Touch Screen Interface**: 直觀的 LCD 觸控操作介面 / Intuitive LCD touch operation interface
- 🔄 **多重模式支援 / Multiple Mode Support**: 
  - Teachable Machine 手勢識別 / Teachable Machine gesture recognition
  - MediaPipe 手勢識別 / MediaPipe gesture recognition
  - 手動按鈕模式 / Manual button mode
- 🛡️ **智能容錯 / Intelligent Fault Tolerance**: 相機、網路、API 的多重備用方案 / Multiple backup solutions for camera, network, and API
- 🎨 **實時預覽 / Real-time Preview**: LCD 螢幕即時顯示相機畫面和手勢信心度 / LCD screen displays camera feed and gesture confidence in real-time

## 🚀 快速開始 / Quick Start

### 系統需求 / System Requirements

- **硬體 / Hardware**: Raspberry Pi 4B+ (建議 4GB+ RAM / Recommended 4GB+ RAM)
- **相機 / Camera**: Raspberry Pi Camera Module 或 USB 攝像頭 / Raspberry Pi Camera Module or USB camera
- **顯示器 / Display**: 觸控 LCD 螢幕 (1920x1080) / Touch LCD screen (1920x1080)
- **印表機 / Printer**: 58mm 熱敏印表機 (USB) / 58mm thermal printer (USB)
- **其他 / Others**: LED、蜂鳴器、按鈕等 GPIO 元件 / LED, buzzer, button and other GPIO components

### 一鍵安裝 / One-Click Installation

```bash
# 1. 克隆專案 / Clone project
git clone https://github.com/YOUR_USERNAME/poetry-camera.git
cd poetry-camera

# 2. 執行安裝腳本 / Run installation script
chmod +x install_dependencies.sh
./install_dependencies.sh

# 3. 設置環境變數 / Set environment variables
cp env_example.txt .env
nano .env  # 填入您的 API 金鑰 / Fill in your API keys

# 4. 檢查環境 / Check environment
source venv/bin/activate
python3 environment_check.py

# 5. 運行程式 / Run program
python3 main.py
```

### 手動安裝 / Manual Installation

<details>
<summary>點擊展開詳細安裝步驟 / Click to expand detailed installation steps</summary>

```bash
# 更新系統 / Update system
sudo apt update && sudo apt upgrade -y

# 安裝系統依賴 / Install system dependencies
sudo apt install -y python3-pip python3-venv git cmake gcc g++ pkg-config \
    libcamera-apps libcamera-dev libjpeg-dev libpng-dev libtiff-dev \
    libavcodec-dev libavformat-dev libswscale-dev libgtk-3-dev \
    python3-pygame python3-opencv alsa-utils

# 克隆專案 / Clone project
git clone https://github.com/YOUR_USERNAME/poetry-camera.git
cd poetry-camera

# 建立虛擬環境 / Create virtual environment
python3 -m venv venv
source venv/bin/activate

# 安裝 Python 套件 / Install Python packages
pip install -r requirements_tested.txt

# 設置環境變數 / Set environment variables
cp env_example.txt .env
# 編輯 .env 文件並填入您的 API 金鑰 / Edit .env file and fill in your API keys
```

</details>

## ⚙️ 配置設置 / Configuration Settings

### API 金鑰設置 / API Key Configuration

在 `.env` 文件中設置您的 API 金鑰 / Set your API keys in the `.env` file:

```bash
# OpenAI API (用於圖像分析 / for image analysis)
OPENAI_API_KEY=your_openai_api_key_here

# DeepSeek API (用於詩歌生成 / for poetry generation)  
DEEPSEEK_API_KEY=your_deepseek_api_key_here
```

### 硬體連接 / Hardware Connection

| 元件 / Component | GPIO 針腳 / GPIO Pin | 說明 / Description |
|------|-----------|------|
| 按鈕 / Button | GPIO 16 | 手動觸發按鈕 / Manual trigger button |
| LED | GPIO 13 | 狀態指示燈 / Status indicator |
| 蜂鳴器 / Buzzer | GPIO 5 | 音效提示 / Sound prompt |
| 相機 / Camera | CSI/USB | Raspberry Pi Camera 或 USB 攝像頭 / Raspberry Pi Camera or USB camera |
| 印表機 / Printer | USB | 58mm 熱敏印表機 / 58mm thermal printer |

詳細的硬體設置請參閱 [docs/HARDWARE.md](docs/HARDWARE.md) / For detailed hardware setup, see [docs/HARDWARE.md](docs/HARDWARE.md)

## 🏗️ 架構說明 / Architecture Overview

### 雙模型架構設計 / Dual-Model Architecture Design

本專案採用創新的雙模型架構來實現高品質的中文新詩創作：

This project adopts an innovative dual-model architecture to achieve high-quality Chinese poetry generation:

```
📸 照片 → 🤖 OpenAI GPT-4o → 📝 描述文字 → 🎭 DeepSeek → ✍️ 中文新詩
Photo   → OpenAI GPT-4o    → Description → DeepSeek   → Chinese Poetry
```

#### 架構流程 / Architecture Flow

1. **照片輸入 / Photo Input**: 用戶拍攝照片或上傳圖片
2. **視覺分析 / Visual Analysis**: OpenAI GPT-4o 分析照片內容並生成詳細描述
3. **文字傳遞 / Text Transfer**: 將描述文字傳遞給 DeepSeek API
4. **詩歌生成 / Poetry Generation**: DeepSeek 根據描述生成中文新詩
5. **輸出列印 / Output Printing**: 將生成的詩歌列印到熱敏紙上

#### 為什麼選擇雙模型？/ Why Dual-Model Approach?

**關於我為何使用雙模型的原因：**

由於我的需求是產生中文的新詩，而 DeepSeek 在預訓練期間主要使用大量中文資料進行訓練，經過我的測試，DeepSeek 所編寫的新詩較符合我的需求。但是 DeepSeek 並不是視覺模型，所以選擇透過其他模型解析圖片內容，再轉拋給 DeepSeek。

**About why I chose the dual-model approach:**

Since my requirement is to generate Chinese poetry, and DeepSeek was primarily trained on large amounts of Chinese data during pre-training, through my testing, DeepSeek's generated poetry better meets my requirements. However, DeepSeek is not a visual model, so I chose to use another model to parse image content and then pass it to DeepSeek.

### 模型優勢分析 / Model Advantages Analysis

#### OpenAI GPT-4o 的視覺能力 / OpenAI GPT-4o Visual Capabilities
- **強大的視覺理解**: 能夠準確識別照片中的物體、場景、情感和細節
- **豐富的描述能力**: 生成詳細、生動的中文描述文字
- **多語言支援**: 支援多種語言的視覺分析

#### DeepSeek 的中文詩歌優勢 / DeepSeek Chinese Poetry Advantages
- **中文預訓練**: 在大量中文文學作品上進行預訓練
- **詩歌風格**: 更符合中文詩歌的韻律和意境
- **文化理解**: 對中國文化和詩歌傳統有深入理解
- **創作品質**: 生成的詩歌更具文學性和藝術性

### 技術實現細節 / Technical Implementation Details

#### API 整合 / API Integration
```python
# 1. OpenAI 視覺分析 / OpenAI Visual Analysis
def analyze_image_with_openai(image_path):
    """使用 OpenAI GPT-4o 分析圖片內容"""
    # 將圖片轉換為 base64 編碼
    # 調用 OpenAI API 進行視覺分析
    # 返回詳細的中文描述

# 2. DeepSeek 詩歌生成 / DeepSeek Poetry Generation  
def generate_poetry_with_deepseek(description):
    """使用 DeepSeek 根據描述生成中文詩歌"""
    # 構建詩歌生成提示詞
    # 調用 DeepSeek API 生成詩歌
    # 返回格式化的中文詩歌
```

#### 錯誤處理與重試機制 / Error Handling and Retry Mechanism
- **API 重試**: 自動重試失敗的 API 調用
- **降級方案**: 當某個 API 不可用時的備用方案
- **錯誤日誌**: 詳細記錄錯誤信息便於調試

### 性能優化 / Performance Optimization

#### 並行處理 / Parallel Processing
- **異步 API 調用**: 減少等待時間
- **緩存機制**: 避免重複的 API 調用
- **批量處理**: 提高處理效率

#### 成本控制 / Cost Control
- **智能提示詞**: 優化 API 調用次數
- **結果緩存**: 避免重複生成相同內容
- **使用監控**: 實時監控 API 使用量

## 🎮 使用方式 / Usage

### 手勢控制 / Gesture Control

1. **OK 手勢** 👌: 拇指與食指形成圓圈，其他手指伸直 / Thumb and index finger form a circle, other fingers extended
2. **YA 手勢** ✌️: 食指與中指伸直（勝利手勢）/ Index and middle fingers extended (victory gesture)

### 操作流程 / Operation Flow

1. 🚀 啟動程式，播放開機音效 / Start program, play startup sound
2. 🎯 選擇操作模式（Teachable Machine / MediaPipe / 手動）/ Select operation mode (Teachable Machine / MediaPipe / Manual)
3. ✋ 做出手勢或按下按鈕 / Make gesture or press button
4. ⏰ 5 秒倒數計時（配音效提示）/ 5-second countdown (with sound prompts)
5. 📸 自動拍照（快門音效）/ Automatic photo capture (shutter sound)
6. 🤖 AI 分析照片內容 / AI analyzes photo content
7. 📝 生成個性化詩歌 / Generate personalized poetry
8. 🖨️ 自動列印詩歌 / Automatically print poetry
9. ✅ 完成提示音 / Completion sound

## 🌐 雙語介面特色 / Bilingual Interface Features

### 自動雙語轉換 / Automatic Bilingual Conversion
- 所有狀態訊息自動顯示中英文 / All status messages automatically display in Chinese and English
- 按鈕文字支援雙語顯示 / Button text supports bilingual display
- 錯誤訊息和提示訊息都支援雙語 / Error messages and prompts support bilingual display

### 介面元素 / Interface Elements
- **LCD 觸控螢幕 / LCD Touch Screen**: 按鈕、狀態、提示都支援雙語
- **控制台輸出 / Console Output**: 重要訊息都支援中英文並存
- **印表機輸出 / Printer Output**: 頁頭頁尾都支援雙語格式

### 狀態訊息範例 / Status Message Examples
```
系統就緒 / System Ready
正在分析照片並生成詩歌 / Analyzing Photo and Generating Poetry
詩歌列印完成 / Poem Printing Completed
```

## 🎵 音效系統 / Sound System

專案包含豐富的音階提示音 / The project includes rich musical prompts:

- **開機音效 / Startup Sound**: Do Re Mi 上升音階，營造愉快啟動體驗 / Do Re Mi ascending scale for pleasant startup experience
- **倒數音效 / Countdown Sound**: 每秒短促中音提醒 / Short mid-tone reminder every second
- **拍照音效 / Photo Sound**: 專業相機快門聲模擬 / Professional camera shutter sound simulation
- **手勢確認 / Gesture Confirmation**: 雙音符確認手勢成功 / Two-note confirmation of successful gesture
- **成功音效 / Success Sound**: 上升三和弦表示操作成功 / Ascending triad indicating successful operation
- **錯誤警告 / Error Warning**: 下降音階提醒錯誤 / Descending scale for error reminder
- **處理音效 / Processing Sound**: 輕柔循環音表示系統工作中 / Gentle loop sound indicating system working
- **列印音效 / Print Sound**: 機械感音效模擬印表機 / Mechanical sound simulating printer
- **系統就緒 / System Ready**: 和諧和弦表示準備完畢 / Harmonious chord indicating ready

## 📊 測試環境 / Test Environment

本專案在以下環境中測試通過 / This project has been tested in the following environment:

> **注意 / Note**: 執行 `python3 environment_check.py` 來生成您系統的詳細環境報告 / Run `python3 environment_check.py` to generate detailed environment report for your system

- **硬體 / Hardware**: Raspberry Pi 5 Model B
- **系統 / System**: Raspberry Pi OS (Debian 12)
- **Python**: 3.11.2
- **主要套件 / Main Packages**: OpenCV 4.11.0.86, MediaPipe 0.10.18, Pygame 2.1.2, RPi.GPIO 0.7.1

詳細環境資訊請參閱 [ENVIRONMENT.md](ENVIRONMENT.md) / For detailed environment information, see [ENVIRONMENT.md](ENVIRONMENT.md)

## 🗂️ 專案結構 / Project Structure

```
poetry-camera/
├── main.py                    # 主程式入口 / Main program entry
├── environment_check.py       # 環境檢查工具 / Environment check tool
├── install_dependencies.sh    # 一鍵安裝腳本 / One-click installation script
├── requirements_tested.txt    # 測試過的 Python 依賴 / Tested Python dependencies
├── BILINGUAL_UI_SUMMARY.md   # 雙語介面修改總結 / Bilingual UI modification summary
├── ENVIRONMENT.md            # 環境測試報告 / Environment test report
├── modules/                   # 核心模組 / Core modules
│   ├── camera.py             # 相機控制 / Camera control
│   ├── config.py             # 配置管理 / Configuration management
│   ├── gesture.py            # Teachable Machine 手勢識別 / Teachable Machine gesture recognition
│   ├── mediapipe_gesture.py  # MediaPipe 手勢識別 / MediaPipe gesture recognition
│   ├── gpio_control.py       # GPIO 與音效控制 / GPIO and sound control
│   ├── lcd_display.py        # LCD 顯示控制（雙語支援）/ LCD display control (bilingual support)
│   ├── poem_api.py           # AI 詩歌生成 / AI poetry generation
│   └── printer.py            # 印表機控制（雙語支援）/ Printer control (bilingual support)
├── models/                    # ML 模型文件 / ML model files
│   ├── model_unquant.tflite  # Teachable Machine 模型 / Teachable Machine model
│   └── labels.txt            # 手勢標籤 / Gesture labels
├── static/                    # 靜態文件 / Static files
│   ├── index.html            # Web 介面 / Web interface
│   └── floating_window.js    # 浮動視窗腳本 / Floating window script
├── photos/                    # 照片和詩歌存儲 / Photo and poem storage
├── logs/                      # 日誌文件 / Log files
└── venv/                      # Python 虛擬環境 / Python virtual environment
```

## 🛠️ 故障排除 / Troubleshooting

### 常見問題 / Common Issues

**相機無法初始化 / Camera cannot initialize**
```bash
# 檢查相機是否啟用 / Check if camera is enabled
sudo raspi-config
# 選擇 Interface Options > Camera > Enable / Select Interface Options > Camera > Enable

# 測試相機 / Test camera
libcamera-hello --list-cameras
```

**GPIO 權限錯誤 / GPIO permission error**
```bash
# 將用戶加入 gpio 群組 / Add user to gpio group
sudo usermod -a -G gpio $USER
sudo usermod -a -G i2c $USER
sudo usermod -a -G spi $USER

# 重新登入或重啟系統 / Re-login or restart system
```

**印表機無法連接 / Printer cannot connect**
```bash
# 檢查 USB 設備 / Check USB devices
lsusb

# 檢查印表機權限 / Check printer permissions
sudo chmod 666 /dev/usblp0
```

**API 錯誤 / API errors**
```bash
# 檢查 API 金鑰設置 / Check API key configuration
cat .env

# 測試網路連接 / Test network connection
ping api.openai.com
```

**雙語顯示問題 / Bilingual display issues**
```bash
# 檢查中文字體 / Check Chinese fonts
fc-list | grep -i chinese

# 安裝中文字體 / Install Chinese fonts
sudo apt install fonts-wqy-zenhei fonts-wqy-microhei
```

## 🤝 貢獻 / Contributing

歡迎提交 Issue 和 Pull Request！/ Welcome to submit Issues and Pull Requests!

### 開發環境設置 / Development Environment Setup

```bash
# 克隆專案 / Clone project
git clone https://github.com/YOUR_USERNAME/poetry-camera.git
cd poetry-camera

# 建立開發分支 / Create development branch
git checkout -b feature/your-feature-name

# 安裝開發依賴 / Install development dependencies
pip install -r requirements_tested.txt

# 運行測試 / Run tests
python3 environment_check.py
```

### 代碼風格 / Code Style

- 使用 Black 格式化 Python 代碼 / Use Black to format Python code
- 遵循 PEP 8 編碼規範 / Follow PEP 8 coding standards
- 添加適當的註釋和文檔字符串 / Add appropriate comments and docstrings
- 支援中英文雙語註釋 / Support bilingual Chinese-English comments

### 雙語支援指南 / Bilingual Support Guidelines

當添加新功能時，請確保支援雙語 / When adding new features, please ensure bilingual support:

```python
# 在 LCD 顯示模組中添加新的狀態訊息 / Add new status messages in LCD display module
self.status_messages["新狀態"] = "New Status"

# 在主程式中使用 / Use in main program
lcd_display.update_status("新狀態")  # 自動轉換為雙語 / Automatically convert to bilingual
```

## 📄 授權 / License

本專案採用 MIT 授權條款 / This project is licensed under the MIT License - 詳見 [LICENSE](LICENSE) 文件 / see the [LICENSE](LICENSE) file for details.

## 🙏 致謝 / Acknowledgments

### 專案來源 / Project Origin
本專案參考並衍伸自 poetry-camera-rpi，原始專案詳見 [poetry.camera](https://poetry.camera) 官方網站。感謝原作者的開源貢獻與靈感啟發。

This project is inspired by and extends from poetry-camera-rpi. For the original project, please visit the [poetry.camera](https://poetry.camera) official website. We thank the original author for their open source contribution and inspiration.

### 技術致謝 / Technical Acknowledgments
- [OpenAI](https://openai.com/) - 提供 GPT-4o API / Providing GPT-4o API
- [DeepSeek](https://www.deepseek.com/) - 提供詩歌生成 API / Providing poetry generation API
- [MediaPipe](https://mediapipe.dev/) - 手勢識別框架 / Gesture recognition framework
- [Teachable Machine](https://teachablemachine.withgoogle.com/) - 機器學習模型訓練 / Machine learning model training
- [Raspberry Pi Foundation](https://www.raspberrypi.org/) - 硬體平台 / Hardware platform

## 📞 聯絡方式 / Contact

- 專案主頁 / Project Homepage: [GitHub Repository](https://github.com/ergeargwer/gesture_camera)
- 問題回報 / Issue Report: [GitHub Issues](https://github.com/ergeargwer/gesture_camera/issues)
- 電子郵件 / Email: peter0910@gmail.com

## 📚 相關文檔 / Related Documentation

- [雙語介面修改總結](BILINGUAL_UI_SUMMARY.md) / [Bilingual UI Modification Summary](BILINGUAL_UI_SUMMARY.md)
- [環境測試報告](ENVIRONMENT.md) / [Environment Test Report](ENVIRONMENT.md)
- [安裝依賴腳本](install_dependencies.sh) / [Installation Dependencies Script](install_dependencies.sh)

![image](https://github.com/user-attachments/assets/5628465a-00bc-440e-b7c1-0cbd60c2380b)

-https://youtu.be/R8WCF3kJ_Rg


⭐ 如果這個專案對您有幫助，請給我們一個星標！/ If this project helps you, please give us a star! ⭐
