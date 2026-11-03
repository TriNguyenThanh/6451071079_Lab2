# TÀI LIỆU HƯỚNG DẪN THỰC HÀNH LAB 2 - KHAI PHÁ DỮ LIỆU
**Môn học**: Khai phá dữ liệu (IT1.231.3)  
**Sinh viên**: Nguyễn Thành Trí - **MSSV**: `6451071079`  
**Giảng viên hướng dẫn**: ThS. Nguyễn Thiện Dương  
**Tài liệu tham chiếu**: [docs/Lab02.pdf](Lab02.pdf), [docs/task.md](task.md), [docs/Template nộp bài LAB2.docx](Template%20nộp%20bài%20LAB2.docx)

---

## 1. MỤC TIÊU & NỘI DUNG TRỌNG TÂM

Lab 2 tập trung vào các giải thuật và kỹ thuật phân lớp dữ liệu (Classification) trong Khai phá dữ liệu:
1. **Cây quyết định (Decision Tree)**:
   - Thuật toán **ID3**: Tiêu chí phân nhánh dựa trên độ đo hỗn loạn thông tin (*Entropy*) và độ lợi thông tin (*Information Gain*).
   - Thuật toán **CART**: Tiêu chí phân nhánh dựa trên chỉ số bất thuần *Gini Index*.
2. **Thuật toán Naïve Bayes**:
   - Sử dụng định lý Bayes với giả định các thuộc tính độc lập có điều kiện.
   - Ứng dụng kỹ thuật làm trơn Laplace (*Laplace Smoothing*) và mô hình `GaussianNB` cho dữ liệu thuộc tính liên tục.
3. **Đánh giá và so sánh mô hình**:
   - Ma trận nhầm lẫn (*Confusion Matrix*): $TP, FP, FN, TN$.
   - Các chỉ số hiệu năng: Accuracy (Độ chính xác tổng thể), Precision (Độ chuẩn xác), Recall (Độ bao phủ/Độ nhạy), và F-measure (F1-score - trung bình điều hòa).
4. **Ứng dụng nhận dạng số viết tay**:
   - Nhận dạng chữ số $8 \times 8$ thang xám từ bộ dữ liệu MNIST (`sklearn.datasets.load_digits`).
   - Tiền xử lý và phân loại ảnh chụp thực tế bằng thư viện Pillow (PIL).

---

## 2. BẢNG ÁNH XẠ CÁC BÀI TẬP (MAPPING MATRIX)

| STT (`task.md`) | Mục trong `Lab02.pdf` | Tên mục trong Template Báo cáo DOCX | Dataset sử dụng | Thuộc tính quyết định (`target`) |
| :--- | :--- | :--- | :--- | :--- |
| **Bài 1** | Mục III.2 (trang 15 – 23) | **Bài 2 (Trang 15-16)** | `Dataset/adult.data.csv`, `adult.test.csv` | `income` (`<=50K`, `>50K`) |
| **Bài 2** | Mục IV.4 (trang 26) | **Bài 4 (Trang 26)** | `Dataset/winequality-red.csv` | `quality` (điểm 3 – 8) |
| **Bài 3** | Mục IV.5 (trang 26 – 27) | **Bài 5 (Trang 26, 27)** | `Dataset/heart.csv` | `target` (0: bình thường, 1: bệnh tim) |
| **Bài 4** | Mục IV.6 (trang 27) | **Bài 6 (Trang 27)** | `Dataset/mushrooms.csv` | `class` (`p`: độc, `e`: ăn được) |
| **Bài 5** | Mục IV.7 (trang 27 – 31) | **Bài 7 (Trang 27 - 31)** | `load_digits()` & `Dataset/test_image_Bai7.jpg` | Chữ số `0 – 9` |
| **Bài 6** | Mục V.2 (trang 32) | **Bài 2 (Trang 32)** | Dữ liệu tính tay cơ bản (Tín dụng, Đồ chơi, Cảm xúc) | $F\text{-measure} = 2 \times \frac{P \times R}{P + R}$ |

---

## 3. PHÂN TÍCH VÀ HƯỚNG DẪN GIẢI CHI TIẾT TỪNG BÀI

### 3.1. BÀI 1: Phân lớp thu nhập Adult Income
- **Dữ liệu**:
  - Huấn luyện: `adult.data.csv` (32,561 mẫu gốc, 15 thuộc tính).
  - Kiểm thử: `adult.test.csv` (16,281 mẫu gốc, 14 thuộc tính - ban đầu không có cột `fnlwgt`).
- **Quy trình từng câu**:
  - **Câu 1**: Đọc 2 file bằng Pandas. In số lượng mẫu gốc.
  - **Câu 2 (Tiền xử lý)**:
    - Giá trị rỗng biểu diễn bằng `'?'` (hoặc `' ?'`). Cần strip chuỗi hoặc dùng `na_values=['?']` với `skipinitialspace=True`.
    - Xóa các dòng rỗng: tập train còn **30,162 dòng**, tập test còn **15,060 dòng**.
    - Xóa cột `fnlwgt` trong tập train.
    - Nối tập train và test bằng `pd.concat([df_train, df_test], ignore_index=True)` thành DataFrame chung **45,222 dòng, 14 cột**.
  - **Câu 3 (Tương quan Pearson)**:
    - Tính ma trận tương quan Pearson trên các cột số (`select_dtypes(include=['int64'])`).
    - Vẽ Heatmap bằng `sns.heatmap(..., annot=True)`.
    - Kết luận: Không có cặp thuộc tính số nào có tương quan quá cao ($|r| > 0.8$), không cần loại bỏ cột.
  - **Câu 4 (Tách features & labels)**:
    - `features = data.drop('income', axis=1)`
    - `labels = data['income']`
  - **Câu 5 (One-hot encoding)**:
    - Tìm các cột `object`: `workclass`, `education`, `marital-status`, `occupation`, `relationship`, `race`, `sex`, `native-country`.
    - Dùng `pd.get_dummies(features, columns=...)` chuyển đổi thành ma trận 103 cột đặc trưng.
  - **Câu 6 (Tách lại train/test ban đầu)**:
    - `X_train = features_onehot.iloc[:30162]`, `X_test = features_onehot.iloc[30162:]`
    - `y_train = labels.iloc[:30162]`, `y_test = labels.iloc[30162:]`
  - **Câu 7 (ID3 Decision Tree)**:
    - `tree.DecisionTreeClassifier(criterion="entropy", random_state=0)`.
    - Fit trên `X_train, y_train`, predict trên `X_test`.
    - In `accuracy_score` (~81.75%), `classification_report`.
    - Vẽ Confusion Matrix Heatmap (`sns.heatmap(tree_cm, annot=True, fmt='d', cmap='Blues_r')`).
    - Trực quan hóa cây quyết định bằng `tree.plot_tree` và lưu ảnh `dpi=300`.
  - **Câu 8 (CART Decision Tree)**:
    - `tree.DecisionTreeClassifier(criterion="gini", random_state=0)`. Thực hiện tương tự Câu 7.
  - **Câu 9 (Gaussian Naïve Bayes)**:
    - `GaussianNB()`. Fit, predict, accuracy (~80.29%), classification report, confusion matrix heatmap.
  - **Câu 10 (So sánh kết quả)**:
    - Lập bảng tổng hợp so sánh: Accuracy, Precision, Recall, F1-Score (macro/weighted) giữa ID3, CART và GaussianNB.

---

### 3.2. BÀI 2: Đánh giá chất lượng rượu vang Red Wine Quality
- **Dữ liệu**: `winequality-red.csv` (1,599 mẫu, 12 cột số).
- **Thuộc tính mục tiêu**: `quality` (điểm nguyên từ 3 đến 8).
- **Quy trình từng câu**:
  - **Câu 3**: Khảo sát Pearson heatmap giữa 11 chỉ số hóa lý. Phân tích các cặp tương quan tự nhiên (như `fixed acidity` vs `citric acid`, `fixed acidity` vs `density`).
  - **Câu 4**: Tách `features` (11 cột hóa lý) và `labels` (`quality`).
  - **Câu 5**: Do các cột đều là số thực, One-hot encoding giữ nguyên không đổi.
  - **Câu 6**: Chia train 70% / test 30%:
    ```python
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
    ```
  - **Câu 7 & 8**: Xây dựng ID3 (`criterion="entropy"`) và CART (`criterion="gini"`), đánh giá đa lớp (Multi-class Confusion Matrix).
  - **Câu 9**: Xây dựng mô hình `GaussianNB()`.
  - **Câu 10**: So sánh hiệu năng 3 mô hình.

---

### 3.3. BÀI 3: Chẩn đoán bệnh tim Heart Disease
- **Dữ liệu**: `heart.csv` (303 mẫu, 14 thuộc tính).
- **Thuộc tính mục tiêu**: `target` (0: Bình thường, 1: Có bệnh tim).
- **Quy trình từng câu**:
  - **Câu a**: Đọc dữ liệu, kiểm tra xóa dòng trống. Đổi tên 14 cột theo chuẩn:
    `['age', 'sex', 'chest_pain_type', 'resting_blood_pressure', 'cholesterol', 'fasting_blood_sugar', 'rest_ecg', 'max_heart_rate_achieved', 'exercise_induced_angina', 'st_depression', 'st_slope', 'num_major_vessels', 'thalassemia', 'target']`.
  - **Câu b**: Lặp lại từ Câu 3 đến Câu 10 (Heatmap tương quan Pearson $\rightarrow$ Tách $X, y$ $\rightarrow$ One-hot $\rightarrow$ Chia train 70% / test 30% $\rightarrow$ Huấn luyện ID3, CART, GaussianNB $\rightarrow$ Đánh giá ma trận nhầm lẫn $\rightarrow$ So sánh).

---

### 3.4. BÀI 4: Phân loại nấm độc hay ăn được (Mushroom)
- **Dữ liệu**: `mushrooms.csv` (8,124 mẫu, 23 cột toàn bộ là biến chuỗi categorical).
- **Thuộc tính mục tiêu**: `class` (`p`: poisonous, `e`: edible).
- **Quy trình từng câu**:
  - Áp dụng từ Câu 4 đến Câu 10:
    - **Câu 4**: Tách `features` (22 thuộc tính hình thái) và `labels` (`class`).
    - **Câu 5**: Chuyển toàn bộ 22 cột sang One-hot vector bằng `pd.get_dummies()`.
    - **Câu 6**: Chia dữ liệu train 70% / test 30% (`random_state=42`).
    - **Câu 7 & 8**: Huấn luyện ID3 và CART (độ chính xác đạt gần 100% do cây quyết định phân tách rất triệt để các đặc tính nấm).
    - **Câu 9**: Huấn luyện `GaussianNB()`.
    - **Câu 10**: So sánh và đánh giá.

---

### 3.5. BÀI 5: Nhận dạng chữ số viết tay MNIST
- **Dữ liệu**: `sklearn.datasets.load_digits()` (1,797 ảnh chữ số $8 \times 8$) và ảnh thực tế `Dataset/test_image_Bai7.jpg`.
- **Quy trình từng bước**:
  1. Tải dữ liệu bằng `datasets.load_digits()`.
  2. Hiển thị 4 mẫu huấn luyện đầu tiên đại diện cho các chữ số 0, 1, 2, 3 bằng `plt.imshow(..., cmap=plt.cm.gray_r)`.
  3. Duỗi ma trận ảnh $8 \times 8$ thành vector $1 \times 64$ bằng `data = digits.images.reshape((n_samples, -1))`.
  4. Chia tập dữ liệu 80% train, 20% test:
     ```python
     X_train, X_test, y_train, y_test = train_test_split(data, digits.target, test_size=0.2, shuffle=False)
     ```
  5. Xây dựng cây quyết định: `DecisionTreeClassifier(criterion="entropy", random_state=33)`.
  6. Dự đoán trên tập kiểm thử, hiển thị trực quan 4 mẫu dự đoán đầu tiên, in bảng `classification_report` và vẽ ma trận nhầm lẫn $10 \times 10$.
  7. **Mở rộng nhận dạng ảnh thực tế**:
     - Đọc ảnh bên ngoài (`Dataset/test_image_Bai7.jpg`) bằng thư viện `PIL.Image`.
     - Chuyển ảnh xám (`convert("L")`), nghịch đảo màu (`ImageOps.invert`), resize về $8 \times 8$.
     - Chuẩn hóa mức xám/phân ngưỡng để chữ số nổi bật trên nền đen.
     - Reshape thành $(1, 64)$ và dùng mô hình ID3 đã huấn luyện để dự đoán chữ số.

---

### 3.6. BÀI 6: Tính toán F-measure cho các bài tập cơ bản
- **Công thức chuẩn**:
  $$F\text{-measure} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$
- **Nội dung thực hiện**:
  1. **Bài cơ bản 1 (trang 1–15 PDF)**: Đánh giá hồ sơ tín dụng 15 dòng:
     - Cây quyết định: $TP=2, FP=1, FN=0, TN=0 \Rightarrow Precision = 67\%, Recall = 100\% \Rightarrow F\text{-measure} \approx 80.24\%$.
     - Naïve Bayes: $TP=2, FP=0, FN=0, TN=1 \Rightarrow Precision = 100\%, Recall = 100\% \Rightarrow F\text{-measure} = 100\%$.
  2. **Bài cơ bản 2 - Thực hành 1 (trang 23–25 PDF)**: Dự đoán doanh số bán đồ chơi (Thấp/Cao).
  3. **Bài cơ bản 3 - Thực hành 2 (trang 25–26 PDF)**: Phân tích cảm xúc mạng xã hội (tốt/xấu/bình thường).
  4. Lập trình tính toán tự động bằng hàm Python, hiển thị bảng kết quả và biểu đồ cột so sánh F-measure.

---

## 4. QUY CHUẨN TRÌNH BÀY MÃ NGUỒN VÀ NOTEBOOK

1. **Định danh sinh viên**:
   - Mọi notebook phải có tiêu đề và cell code in ra họ tên:
     ```python
     # Nguyễn Thành Trí - MSSV: 6451071079
     print("Họ tên: Nguyễn Thành Trí - MSSV: 6451071079")
     ```
2. **Quy tắc đường dẫn tương đối an toàn**:
   ```python
   import os

   def get_data_path(rel_path):
       candidates = [
           rel_path,
           os.path.join("..", rel_path),
           os.path.join("..", "..", rel_path),
           os.path.join("Dataset", os.path.basename(rel_path)),
           os.path.join("..", "Dataset", os.path.basename(rel_path)),
           os.path.join("..", "..", "Dataset", os.path.basename(rel_path))
       ]
       for p in candidates:
           if os.path.exists(p):
               return p
       raise FileNotFoundError(f"Không tìm thấy file: {rel_path}")

   def get_img_dir(subpath=""):
       for base in [".", "..", "../.."]:
           img_root = os.path.join(base, "images")
           if os.path.exists(img_root) and (os.path.exists(os.path.join(base, "Dataset")) or os.path.exists(os.path.join(base, "CLAUDE.md"))):
               target = os.path.join(img_root, subpath) if subpath else img_root
               os.makedirs(target, exist_ok=True)
               return target
       target = os.path.join("images", subpath) if subpath else "images"
       os.makedirs(target, exist_ok=True)
       return target
   ```
3. **Lưu trữ đồ thị**:
   - Tất cả hình ảnh biểu đồ (Heatmap, Decision Tree, Confusion Matrix, MNIST digits) phải lưu tại `images/Bai<X>/` với `dpi=300, bbox_inches='tight'` để chèn vào báo cáo Word chất lượng cao nhất.
