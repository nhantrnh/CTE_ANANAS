Thời gian lấy data từ 09/2022 - 09/2025

Quy trình bao gồm 5 bước chính được thực thi tuần tự trong notebook:
1.  **Lấy Danh sách Cổ phiếu:** Tải danh sách các mã cổ phiếu từ 3 sàn (HOSE, HNX, UPCOM).
2.  **Thu thập và Xử lý Dữ liệu:** Tải dữ liệu giá lịch sử, tính toán hàng chục chỉ số kỹ thuật (TA).
3.  **Làm giàu Dữ liệu:** Tải dữ liệu tài chính cơ bản (FA) và gộp vào bộ dữ liệu chính.
4.  **Huấn luyện Mô hình:** Xây dựng một mô hình dự đoán (LightGBM) để tìm ra các cơ hội giao dịch tiềm năng.
5.  **Kiểm thử Chiến lược (Backtest):** Mô phỏng việc thực hiện chiến lược giao dịch trên dữ liệu quá khứ để đánh giá hiệu quả.

## Cấu trúc Dự án
*   `problem2_ANANAS.ipynb`: File Jupyter Notebook chính chứa toàn bộ mã nguồn và logic.
*   `.env`: File cấu hình chứa thông tin API **(tự tạo)**.
*   `ticker_lists/`: Thư mục chứa danh sách mã cổ phiếu (sẽ được tạo ra ở bước 1).
*   `final_data/`: Thư mục chứa dữ liệu đã được làm giàu với chỉ số TA & FA (sẽ được tạo ra ở bước 3).
*   `model_artifacts/`: Thư mục chứa mô hình đã huấn luyện và các file phụ trợ cho backtest (sẽ được tạo ra ở bước 4).

## Hướng dẫn Cài đặt & Cấu hình

### 1. Yêu cầu
*   Python 3.9+
*   Jupyter Notebook / JupyterLab / VS Code
*   Tài khoản API từ **FiinGroup** (cần thiết để chạy lại toàn bộ pipeline).

### 2. Tải mã nguồn
```bash
git clone https://github.com/nhantrnh/CTE_ANANAS
cd CTE_ANANAS
```

### 3. Tạo file `.env` để Cung cấp Thông tin Đăng nhập (Bước Bắt buộc)
Notebook này sử dụng file `.env` để đọc thông tin đăng nhập API một cách an toàn mà không cần viết trực tiếp vào code. (tự tạo)

1.  Trong thư mục gốc của dự án (cùng cấp với file Notebook), hãy tạo một file mới.
2.  Đặt tên cho file chính xác là `.env` (có dấu chấm ở đầu, không có phần mở rộng).
3.  Mở file `.env` và dán nội dung sau vào, thay thế bằng thông tin tài khoản của bạn:
    ```
    API_USERNAME="ten_dang_nhap_cua_ban"
    API_PASSWORD="mat_khau_cua_ban"
    ```

## Hướng dẫn Chạy Notebook
Mở file `problem_ANANAS.ipynb` và làm theo một trong hai kịch bản dưới đây.

---
### Kịch bản 1: Chạy Toàn bộ Pipeline (Yêu cầu API)
Kịch bản này sẽ thực hiện tất cả 5 bước từ đầu, tự động lấy dữ liệu mới nhất, huấn luyện lại mô hình và chạy backtest.

**Cách thực hiện:**
1.  Đảm bảo hoàn thành tất cả các bước **Cài đặt & Cấu hình** ở trên, đặc biệt là đã **tạo file `.env`**.
2.  Trong giao diện Jupyter, chọn **"Kernel" -> "Restart & Run All"** (hoặc tùy chọn tương đương) để chạy tất cả các cell từ đầu đến cuối.
3.  Quá trình có thể mất một thời gian đáng kể, tùy thuộc vào tốc độ mạng và cấu hình máy.
4.  Kết quả cuối cùng sẽ là các chỉ số hiệu suất và biểu đồ backtest ở cuối notebook.

---
### Kịch bản 2: Chỉ Chạy Phần Backtest (Sử dụng Dữ liệu Tải sẵn)
Sử dụng kịch bản này nếu **không có tài khoản API** hoặc chỉ muốn kiểm tra lại nhanh kết quả backtest mà không cần chạy lại toàn bộ quá trình.

**Cách thực hiện:**

**Bước 1: Tải Dữ liệu Đã Xử lý**
Tải file `.zip` chứa toàn bộ dữ liệu và mô hình đã được chuẩn bị sẵn tại đây:

**Link truy cập: ** https://drive.google.com/drive/u/0/folders/1lBdqcssDtcjMww3z5YtXum_4GxR_Uh4s

**Bước 2: Giải nén và Sắp xếp Thư mục**
1.  Giải nén file vừa tải.
2.  Các thư mục: `ticker_lists`, `final_data`, và `model_artifacts`, các file `X_all_by1d.parquet` chứa thông tin của dữ liệu của mỗi sàn lấy theo ngày trong 3 năm, `X_with_indicators_patterns` là file chứa thông tin của mỗi sàn sau khi tính chỉ số FA.
3.  Copy cả 3 thư mục này và dán chúng vào thư mục gốc của dự án (cùng cấp với file `.ipynb`).

**Bước 3: Chạy các Cell Cần thiết**
1.  Mở file notebook.
2.  **Bỏ qua (không chạy)** các cell trong các phần sau:
    *   Phần 1: Lấy Danh sách Cổ phiếu
    *   Phần 2: Thu thập và Xử lý Dữ liệu TA
    *   Phần 3: Làm giàu Dữ liệu FA
    *   Phần 4: Huấn luyện Mô hình
3.  Chạy các cell trong phần **Cài đặt Thư viện** và **Import Thư viện** ở đầu notebook.
4.  Cuộn xuống cuối notebook và **chỉ chạy các cell trong Phần 5: Backtest & Đánh giá Hiệu suất**.

Script backtest sẽ tự động tải các "sản phẩm" cần thiết từ thư mục `model_artifacts` và thực hiện mô phỏng giao dịch.

PHỤ LỤC:

### 1. Chỉ số Phân tích Kỹ thuật (TA)
Đây là toàn bộ các chỉ số dùng để phân tích biểu đồ và hành động giá.

#### a. Chỉ báo Xu hướng (Trend Indicators)
*   `MA20`, `MA50`, `MA200`: Đường trung bình động đơn giản (Simple Moving Average).
*   `EMA10`, `EMA20`, `EMA50`: Đường trung bình động hàm mũ (Exponential Moving Average).
*   `WMA14`: Đường trung bình động có trọng số (Weighted Moving Average).
*   `MACD`, `MACD_signal`, `MACD_hist`: Trung bình động hội tụ phân kỳ.
*   `ADX`, `DMP`, `DMN`: Chỉ số Sức mạnh Xu hướng (Average Directional Index).
*   `PSAR`, `PSAR_up`, `PSAR_down`: Parabolic SAR.
*   `Tenkan_sen`, `Kijun_sen`, `Senkou_Span_A`, `Senkou_Span_B`, `Chikou_Span`: Các thành phần của mây Ichimoku.
*   `Aroon_Up`, `Aroon_Down`, `Aroon_Oscillator`: Chỉ báo Aroon.
*   `Supertrend`, `Supertrend_direction`: Chỉ báo Supertrend.

#### b. Chỉ báo Động lượng (Momentum Indicators)
*   `RSI14`: Chỉ số Sức mạnh Tương đối (Relative Strength Index).
*   `Stoch_K`, `Stoch_D`: Stochastic Oscillator.
*   `CCI20`: Chỉ số Kênh hàng hóa (Commodity Channel Index).

#### c. Chỉ báo Biến động (Volatility Indicators)
*   `BB_upper`, `BB_middle`, `BB_lower`: Dải Bollinger (Bollinger Bands).
*   `ATR14`: Khoảng dao động thực trung bình (Average True Range).

#### d. Chỉ báo Khối lượng (Volume Indicators)
*   `MFI`: Chỉ số Dòng tiền (Money Flow Index).
*   `OBV`: Khối lượng Cân bằng (On-Balance Volume).
*   `VWAP`: Giá trung bình theo khối lượng (Volume-Weighted Average Price).

#### e. Mẫu hình Giá & Nến (Price & Candlestick Patterns)
*   `Gap`: Khoảng trống giá.
*   `Doji`, `Hammer`, `Bullish_Engulfing`, `Bearish_Engulfing`: Các mẫu hình nến Nhật.

### 2. Chỉ số Phân tích Cơ bản (FA)
Đây là các chỉ số thể hiện "nội tại" của doanh nghiệp.

#### a. Chỉ số Định giá (Valuation Ratios)
*   `PriceToBook`: Tỷ lệ Giá trên Giá trị sổ sách (P/B).
*   `PriceToEarning`: Tỷ lệ Giá trên Lợi nhuận (P/E).

#### b. Chỉ số Hiệu quả & Lợi nhuận (Efficiency & Profitability Ratios)
*   `EBITMargin`: Biên lợi nhuận trước thuế và lãi vay.
*   `ROA`: Tỷ suất sinh lời trên tổng tài sản (Return on Assets).
*   `ROE`: Tỷ suất sinh lời trên vốn chủ sở hữu (Return on Equity).
*   `ROIC`: Tỷ suất sinh lời trên vốn đầu tư (Return on Invested Capital).
*   `BasicEPS`: Lãi cơ bản trên cổ phiếu (Earnings Per Share).

#### c. Các Nhóm Chỉ số (Thường chứa trong dictionary `{}`)
*   `SolvencyRatio`: Nhóm chỉ số về khả năng thanh toán nợ (ví dụ: `DebtToEquity`).
*   `Growth`: Nhóm chỉ số về tăng trưởng (ví dụ: tăng trưởng doanh thu, lợi nhuận).

### 3. Dữ liệu Gốc (Raw Data)
Đây là dữ liệu đầu vào cơ bản nhất.
*   `timestamp`: Mốc thời gian (ngày).
*   `ticker`: Mã cổ phiếu.
*   `open`, `high`, `low`, `close`: Giá Mở/Cao/Thấp/Đóng cửa.
*   `volume`: Khối lượng giao dịch.
*   
### 4. Các Cột Phụ trợ / Siêu dữ liệu (Helper / Metadata)
Đây là các cột dùng để xử lý hoặc cung cấp thông tin bổ sung.
*   `Prev_Close`: Giá đóng cửa của phiên trước đó (dùng để tính `Gap`).
*   `exchange`: Tên sàn giao dịch (HOSE, HNX, UPCOM).
*   `year`, `quarter`: Năm và Quý của báo cáo tài chính được sử dụng.
