# LAB 2: CÂY QUYẾT ĐỊNH & THUẬT TOÁN PHÂN LỚP DỮ LIỆU

### MÔN HỌC: KHAI PHÁ DỮ LIỆU (DATA MINING - IT1.231.3)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.14%2B-blue?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.14+"/>
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter Notebook"/>
  <img src="https://img.shields.io/badge/Scikit--Learn-Classification-green?style=for-the-badge&logo=scikitlearn&logoColor=white" alt="Scikit-Learn"/>
  <img src="https://img.shields.io/badge/Status-In%20Progress-yellow?style=for-the-badge" alt="Status In Progress"/>
</p>

---

## THÔNG TIN SINH VIÊN & HỌC PHẦN

- **Họ và tên sinh viên**: Nguyễn Thành Trí
- **Mã số sinh viên (MSSV)**: `6451071079`
- **Lớp**: CQ.64.CNTT
- **Mã học phần**: `IT1.231.3` - Khai phá dữ liệu (Data Mining)
- **Giảng viên hướng dẫn**: ThS. Nguyễn Thiện Dương
- **Tài liệu tham chiếu**: [docs/Lab02.pdf](docs/Lab02.pdf), [docs/task.md](docs/task.md), [docs/huong_dan_lab2.md](docs/huong_dan_lab2.md), [docs/Template nộp bài LAB2.docx](docs/Template%20n%E1%BB%99p%20b%C3%A0i%20LAB2.docx)

---

## TỔNG QUAN DỰ ÁN

Kho lưu trữ này chứa toàn bộ mã nguồn, tài liệu và hình ảnh trực quan hóa cho bài thực hành **Lab 2: Phân lớp dữ liệu (Data Classification)**. Nội dung tập trung vào:

1. **Cây quyết định (Decision Tree)**:
   - Thuật toán **ID3** dựa trên tiêu chí Information Gain / Entropy.
   - Thuật toán **CART** dựa trên tiêu chí Gini Index.
   - Trực quan hóa cấu trúc phân nhánh cây.
2. **Thuật toán Naïve Bayes**:
   - Ứng dụng mô hình xác suất điều kiện và `GaussianNB`.
3. **Đánh giá mô hình phân lớp**:
   - Ma trận nhầm lẫn (Confusion Matrix), Accuracy, Precision, Recall và F-measure.
4. **Nhận dạng chữ số viết tay**:
   - Khai phá bộ dữ liệu chữ số MNIST của Scikit-Learn và nhận dạng ảnh chụp thực tế bằng thư viện Pillow.

---

## DANH SÁCH BÀI TẬP VÀ MÃ NGUỒN

| Bài       | Tên bài tập               | Notebook                          | Dataset                                 | Mô hình áp dụng           |
| :-------- | :------------------------ | :-------------------------------- | :-------------------------------------- | :------------------------ |
| **Bài 1** | Adult Income              | `Bai1/6451071079_LAB2_Bai1.ipynb` | `adult.data.csv`, `adult.test.csv`      | ID3, CART, GaussianNB     |
| **Bài 2** | Red Wine Quality          | `Bai2/6451071079_LAB2_Bai2.ipynb` | `winequality-red.csv`                   | ID3, CART, GaussianNB     |
| **Bài 3** | Heart Disease             | `Bai3/6451071079_LAB2_Bai3.ipynb` | `heart.csv`                             | ID3, CART, GaussianNB     |
| **Bài 4** | Mushroom Classification   | `Bai4/6451071079_LAB2_Bai4.ipynb` | `mushrooms.csv`                         | ID3, CART, GaussianNB     |
| **Bài 5** | MNIST Hand-written Digits | `Bai5/6451071079_LAB2_Bai5.ipynb` | `load_digits()` + `test_image_Bai7.jpg` | ID3, Pillow Preprocessing |
| **Bài 6** | F-measure Calculation     | `Bai6/6451071079_LAB2_Bai6.ipynb` | Dữ liệu cơ bản                          | F-measure Formula & Code  |

---

## CẤU TRÚC THƯ MỤC

```text
lab2/
├── Dataset/                            # Thư mục lưu trữ toàn bộ tập dữ liệu nguồn
│   ├── adult.data.csv
│   ├── adult.test.csv
│   ├── winequality-red.csv
│   ├── heart.csv
│   ├── mushrooms.csv
│   └── test_image_Bai7.jpg
├── docs/                               # Tài liệu hướng dẫn & tài liệu tham chiếu
│   ├── Lab02.pdf
│   ├── task.md
│   ├── huong_dan_lab2.md
│   └── Template nộp bài LAB2.docx
├── images/                             # Hình ảnh, biểu đồ lưu trữ phục vụ báo cáo
│   ├── Bai1/
│   ├── Bai2/
│   ├── Bai3/
│   ├── Bai4/
│   ├── Bai5/
│   └── Bai6/
├── Bai1/                               # Thư mục bài làm từng phần
│   └── 6451071079_LAB2_Bai1.ipynb
├── Bai2/
│   └── 6451071079_LAB2_Bai2.ipynb
├── ...
├── .venv/                              # Môi trường ảo Python
├── requirements.txt                    # Danh sách các thư viện phụ thuộc
├── CLAUDE.md                           # Quy tắc chỉ dẫn cho Claude Code
├── AGENTS.md                           # Quy chuẩn phổ quát cho tất cả AI Agent
└── README.md                           # Tài liệu tổng quan dự án
```

---

## HƯỚNG DẪN CÀI ĐẶT & CHẠY DỰ ÁN

1. **Khởi tạo và kích hoạt môi trường ảo**:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```
2. **Cài đặt các thư viện phụ thuộc**:
   ```bash
   pip install -r requirements.txt
   ```
3. **Cài đặt Jupyter Kernel**:
   ```bash
   python -m ipykernel install --user --name venv_lab2 --display-name "Python 3 (.venv)"
   ```
4. **Khởi chạy Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```
