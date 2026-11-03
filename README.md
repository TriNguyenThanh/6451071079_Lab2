# LAB 1: TIỀN XỬ LÝ DỮ LIỆU & KHAI PHÁ LUẬT KẾT HỢP

### MÔN HỌC: KHAI PHÁ DỮ LIỆU (DATA MINING - IT1.231.3)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12%2B-blue?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.12+"/>
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter Notebook"/>
  <img src="https://img.shields.io/badge/MLxtend-Association%20Rules-green?style=for-the-badge" alt="MLxtend"/>
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" alt="Status Completed"/>
</p>

---

## THÔNG TIN SINH VIÊN & HỌC PHẦN

- **Họ và tên sinh viên**: Nguyễn Thành Trí
- **Mã số sinh viên (MSSV)**: `6451071079`
- **Lớp**: CQ.64.CNTT
- **Mã học phần**: `IT1.231.3` - Khai phá dữ liệu (Data Mining)
- **Tài liệu học phần**: [docs/lab1.pdf](docs/lab1.pdf), [docs/bai_tap_lab1.md](docs/bai_tap_lab1.md), [docs/Template nộp bài LAB1.docx](docs/Template%20n%E1%BB%99p%20b%C3%A0i%20LAB1.docx)

---

## TỔNG QUAN DỰ ÁN

Kho lưu trữ này chứa toàn bộ mã nguồn, tài liệu và hình ảnh trực quan hóa cho bài thực hành **Lab 1: Khai phá dữ liệu**. Nội dung dự án tập trung vào hai trụ cột nền tảng của Data Mining:

1. **Phần 1 - Tiền xử lý dữ liệu (Data Preprocessing)**:
   - Xử lý giá trị khuyết thiếu (Missing Values, ngầm định `?`, `'unknown'`), làm sạch và lọc dữ liệu lỗi/nhiễu.
   - Rời rạc hóa (Binning: `pd.cut`, `pd.qcut`), biến đổi thang đo và kỹ thuật trích xuất đặc trưng mới.
   - Mã hóa thuộc tính định danh (One-Hot Encoding, Label Encoding).
   - Khảo sát trực quan hóa tương quan giữa các thuộc tính nhân khẩu học/môi trường với biến mục tiêu.

2. **Phần 2 - Khai phá tập phổ biến & Luật kết hợp (Frequent Itemset & Association Rule Mining)**:
   - Làm sạch hóa đơn bán lẻ, loại bỏ giao dịch hoàn hủy (`'C'`) và cước bưu chính (`'POSTAGE'`).
   - Xây dựng ma trận giao dịch One-Hot (Basket Matrix).
   - Áp dụng hai giải thuật kinh điển: **Apriori** và **FP-Growth** (Frequent Pattern Growth).
   - Phân tích và đánh giá chất lượng luật kết hợp thông qua các chỉ số **Support**, **Confidence** và **Lift**.
   - Trực quan hóa phân bố luật kết hợp và so sánh hiệu năng thực thi giữa các thuật toán.

---

## CẤU TRÚC THƯ MỤC DỰ ÁN

Cấu trúc cây thư mục được quy hoạch chuẩn mực, phân tách rõ ràng giữa dữ liệu nguồn, tài liệu, mã nguồn và tài nguyên biểu đồ:

```text
khai-pha-du-lieu-lab1/
├── datasets/                            # Thư mục lưu trữ toàn bộ tập dữ liệu nguồn
│   ├── Bank/                            # Dữ liệu chiến dịch tiếp thị ngân hàng
│   │   ├── bank.csv                     # Tập dữ liệu mẫu nhỏ (4,521 dòng)
│   │   └── bank-full.csv                # Tập dữ liệu đầy đủ (45,211 dòng)
│   ├── groceries.csv                    # Dữ liệu giỏ hàng siêu thị (9,835 giao dịch)
│   ├── Online Retail.xlsx               # Lịch sử bán lẻ trực tuyến (541,909 dòng)
│   ├── online_retail_clean.pkl          # Dữ liệu Online Retail đã làm sạch (cache tăng tốc)
│   ├── tb_tracking.xlsx                 # Dữ liệu chuỗi thời gian cảm biến nhà xưởng
│   └── titanic.csv                      # Dữ liệu hành khách tàu Titanic
│
├── docs/                                # Tài liệu hướng dẫn & biểu mẫu báo cáo
│   ├── bai_tap_lab1.md                  # Bản đặc tả chi tiết yêu cầu từng bài tập
│   ├── lab1.pdf                         # Giáo trình hướng dẫn thực hành gốc
│   └── Template nộp bài LAB1.docx       # Mẫu văn bản báo cáo nghiệm thu chính thức
│
├── images/                              # Thư mục tập trung lưu trữ ảnh biểu đồ xuất báo cáo
│   ├── Phan1/                           # Ảnh đồ thị Phần 1
│   │   ├── Bai1/                        # Biểu đồ phân tích dữ liệu Titanic (Age, Sex, Pclass...)
│   │   ├── Bai2/                        # Biểu đồ phân tích Bank Marketing (Job, Marital, Campaign...)
│   │   └── Bai3/                        # Đồ thị phân tán cảm biến CO, Độ ẩm, Nhiệt độ
│   └── Phan2/                           # Ảnh đồ thị Phần 2
│       ├── Bai2/                        # Đồ thị phân tán luật kết hợp thị trường Anh (UK)
│       ├── Bai5a/                       # Đồ thị phân tán luật kết hợp thị trường Đức (Germany)
│       └── Bai6/                        # Biểu đồ so sánh thời gian chạy Apriori vs FP-Growth
│
├── Phan1/                               # Bài làm Phần 1: Tiền xử lý dữ liệu
│   ├── Bai1/                            # Bài 1 (Tr. 15-17): Tiền xử lý dữ liệu Titanic
│   │   └── 6451071079_LAB1_Bai1.ipynb   # Notebook tổng hợp toàn bộ 8 câu hỏi
│   ├── Bai2/                            # Bài 2 (Tr. 28-29): Tiền xử lý dữ liệu Bank Marketing
│   │   └── 6451071079_LAB1_Bai2.ipynb   # Notebook tổng hợp toàn bộ 6 câu hỏi
│   └── Bai3/                            # Bài 3 (Tr. 30): Tiền xử lý dữ liệu cảm biến tb_tracking
│       └── 6451071079_LAB1_Bai3.ipynb   # Notebook tổng hợp toàn bộ các yêu cầu a, b, c, d
│
├── Phan2/                               # Bài làm Phần 2: Khai phá tập phổ biến & Luật kết hợp
│   ├── Bai2/                            # Bài 2 (Tr. 37-38): Khai phá dữ liệu Online Retail (UK)
│   │   └── 6451071079_LAB1_Bai2.ipynb   # Notebook tổng hợp toàn bộ 12 câu hỏi
│   ├── Bai5a/                           # Bài 5a (Tr. 45): Phân tích thị trường Đức (Germany)
│   │   └── 6451071079_LAB1_Bai5a.ipynb  # Notebook tổng hợp toàn bộ 12 câu hỏi
│   └── Bai6/                            # Bài 6 (Tr. 45): So sánh hiệu năng Apriori vs FP-Growth
│       └── 6451071079_LAB1_Bai6.ipynb   # Notebook tổng hợp toàn bộ các yêu cầu a, b, c
│
├── scripts/                             # Scripts công cụ tự động hóa & kiểm thử notebook (gitignored)
│
├── requirements.txt                     # Danh sách thư viện Python phụ thuộc
├── AGENTS.md                            # Quy chuẩn quốc tế dành cho tất cả các AI Coding Agent
├── CLAUDE.md                            # Chỉ dẫn vận hành chuyên biệt cho Claude Code CLI
└── README.md                            # Tài liệu tổng quan dự án (file này)
```

---

## BẢNG ÁNH XẠ BÀI TẬP & TẬP DỮ LIỆU (DATASETS)

| Phần / Bài tập                        | Tập dữ liệu yêu cầu     | Đường dẫn dữ liệu                                            | Mục tiêu & Kỹ thuật chính                                                                                                                      | File notebook bài làm                                                                                     |
| :------------------------------------ | :---------------------- | :----------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------- |
| **Phần 1 - Bài 1**<br>_(Trang 15-17)_ | Titanic Dataset         | [datasets/titanic.csv](datasets/titanic.csv)                 | Xử lý missing `?`, trích danh hiệu `title`, tạo biến `age_class`, `fare_per_person`, vẽ biểu đồ tương quan với `survived`.                     | [Phan1/Bai1/6451071079_LAB1_Bai1.ipynb](Phan1/Bai1/6451071079_LAB1_Bai1.ipynb)                           |
| **Phần 1 - Bài 2**<br>_(Trang 28-29)_ | Bank Marketing          | [datasets/Bank/](datasets/Bank/)                             | Nhận diện giá trị `unknown`, mã hóa One-Hot/Label, chia nhóm thời lượng `duration`, phân tích tương quan khách hàng mở sổ tiết kiệm (`y`).     | [Phan1/Bai2/6451071079_LAB1_Bai2.ipynb](Phan1/Bai2/6451071079_LAB1_Bai2.ipynb)                           |
| **Phần 1 - Bài 3**<br>_(Trang 30)_    | Cảm biến môi trường     | [datasets/tb_tracking.xlsx](datasets/tb_tracking.xlsx)       | Đặt tên cột (`id`, `device_id`, `co_level`...), lọc mốc thời gian $\ge 03/01/2018$, bóc tách `minutes` trong ngày, vẽ scatter plots.           | [Phan1/Bai3/6451071079_LAB1_Bai3.ipynb](Phan1/Bai3/6451071079_LAB1_Bai3.ipynb)                           |
| **Phần 2 - Bài 2**<br>_(Trang 37-38)_ | Online Retail (UK)      | [datasets/Online Retail.xlsx](datasets/Online%20Retail.xlsx) | Lọc hóa đơn nước Anh, xóa hóa đơn hủy `'C'` và `'POSTAGE'`, khai phá Apriori & FP-Growth ($\text{min\_sup}=3\%, \text{min\_conf}=50\%$).       | [Phan2/Bai2/6451071079_LAB1_Bai2.ipynb](Phan2/Bai2/6451071079_LAB1_Bai2.ipynb)                           |
| **Phần 2 - Bài 5a**<br>_(Trang 45)_   | Online Retail (Germany) | [datasets/Online Retail.xlsx](datasets/Online%20Retail.xlsx) | Lọc thị trường Đức, xóa `'POSTAGE'` ($81.84\%$), khai phá Apriori & FP-Growth ($\text{min\_sup}=5\%, \text{min\_conf}=50\%$), so sánh đối đầu. | [Phan2/Bai5a/6451071079_LAB1_Bai5a.ipynb](Phan2/Bai5a/6451071079_LAB1_Bai5a.ipynb)                       |
| **Phần 2 - Bài 6**<br>_(Trang 45)_    | Groceries Dataset       | [datasets/groceries.csv](datasets/groceries.csv)             | Mã hóa 9,835 giao dịch qua `TransactionEncoder`, đo thời gian `time.perf_counter()` khi $\text{min\_sup}$ quét từ $0.01\%$ đến $0.1\%$.        | [Phan2/Bai6/6451071079_LAB1_Bai6.ipynb](Phan2/Bai6/6451071079_LAB1_Bai6.ipynb)                           |

---

## HƯỚNG DẪN CÀI ĐẶT & THỰC THI (SETUP & EXECUTION)

### 1. Yêu cầu hệ thống

- Hệ điều hành: Linux (Fedora/Ubuntu), macOS hoặc Windows (WSL2).
- Python: Phiên bản `3.10` trở lên (Khuyến nghị `3.12+`).
- Git & Trình duyệt web / VSCode để xem Notebook.

### 2. Khởi tạo môi trường ảo Python (`.venv`)

Để đảm bảo tính cô lập và tránh xung đột thư viện với hệ điều hành:

```bash
# Clone repository
git clone <repository-url>
cd lab1

# Khởi tạo môi trường ảo .venv
python3 -m venv .venv

# Kích hoạt môi trường ảo
# Trên Linux/macOS:
source .venv/bin/activate
# Trên Windows (PowerShell):
# .venv\Scripts\Activate.ps1
```

### 3. Cài đặt các gói thư viện phụ thuộc

Cài đặt danh sách thư viện từ file [requirements.txt](requirements.txt):

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

_Danh mục thư viện nòng cốt:_

- `numpy`, `pandas`: Thao tác cấu trúc dữ liệu và xử lý bảng ma trận.
- `matplotlib`, `seaborn`: Thiết lập đồ thị phân tán và trực quan hóa phân bố dữ liệu.
- `mlxtend`: Khai phá tập phổ biến (`apriori`, `fpgrowth`) và luật kết hợp (`association_rules`).
- `openpyxl`: Đọc và ghi dữ liệu file định dạng Excel (`.xlsx`).
- `ipykernel`, `nbformat`, `nbclient`: Thực thi và quản lý Jupyter Notebooks.

---

## BẢN QUYỀN & THÔNG TIN LIÊN HỆ

- **Tác giả**: Nguyễn Thành Trí (MSSV: `6451071079`)
- **Khoa / Viện**: Công nghệ thông tin
- **Môn học**: Khai phá dữ liệu (IT1.231.3)
- **Email sinh viên**: `6451071079@st.utc2.edu.vn`
