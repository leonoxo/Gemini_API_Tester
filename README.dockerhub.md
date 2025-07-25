# Gemini API Tester

這是一個用於批量驗證 Google Gemini API 金鑰有效性的 Python 工具。此 Docker 映像檔包含所有必要的依賴項，讓您可以免於設定 Python 環境，直接執行測試。

**GitHub 儲存庫**: [https://github.com/leonoxo/gemini_api_tester](https://github.com/leonoxo/gemini_api_tester)

## 如何拉取映像檔

```bash
docker pull leonoxo/gemini_api_tester:latest
```

## ✨ 如何使用

此工具包含兩個主要腳本，您需要透過掛載本地目錄的方式來讓容器讀寫您的金鑰檔案。

### 工作流程

1.  **準備金鑰檔案**
    在您電腦的任何一個工作目錄下，建立 `check_keys.txt` 和 `api_keys.txt` 這兩個檔案。

2.  **步驟一：清理金鑰**
    將您收集到的所有原始 API 金鑰貼到 `check_keys.txt` 檔案中。然後在該目錄下執行以下指令來清理和去除重複的金鑰：
    ```bash
    docker run --rm -v "$(pwd):/app" leonoxo/gemini_api_tester python check_for_duplicate_keys.py
    ```
    此指令會將整理好的、不重複的金鑰列表輸出到您的終端機。

3.  **步驟二：準備測試**
    將上一步從終端機輸出的乾淨金鑰列表，複製並貼到 `api_keys.txt` 檔案中。

4.  **步驟三：執行驗證**
    執行以下指令來開始驗證 `api_keys.txt` 中的所有金鑰：
    ```bash
    docker run --rm -v "$(pwd):/app" leonoxo/gemini_api_tester python gemini_api_tester.py
    ```

5.  **步驟四：獲取結果**
    測試完成後，一個名為 `api_keys_verified_YYYYMMDD.txt` 的新檔案將會出現在您的工作目錄中，裡面包含了所有通過驗證的 API 金鑰。

### 指令詳解

-   `docker run`: 執行 Docker 容器。
-   `--rm`: 容器執行完畢後自動刪除，保持系統乾淨。
-   `-v "$(pwd):/app"`: **(關鍵部分)** 這會將您當前的主機目錄（`$(pwd)`）掛載到容器內的 `/app` 目錄。這使得容器內的腳本可以讀取您本地的 `api_keys.txt` 和 `check_keys.txt`，並將結果 `api_keys_verified_...txt` 寫回到您本地的目錄。
-   `leonoxo/gemini_api_tester`: 您要執行的映像檔名稱。
-   `python <script_name>.py`: 在容器內要執行的指令。

## ⚠️ 安全提醒

請務必妥善保管您的 API 金鑰。不要將包含金鑰的 `.txt` 檔案上傳到任何公開的程式碼儲存庫。此工具的設計初衷就是在本地環境安全地處理您的金鑰。