## Bài 1: 2. (trang 15 - 16) Yêu cầu lập trình

Cho Adult Income1 là dữ liệu về thu nhập của những người trẻ tuổi ở Hoa Kỳ với thuộc
tính quyết định ‘Income’. Thuộc tính này chứa hai giá trị là ‘>50K’ (thu nhập lớn hơn
50.000 USD/năm) và ‘<=50K’ (thu nhập bé hơn hoặc bằng 50.000 USD/năm). Sinh viên
hãy thực hiện những yêu cầu sau đây:

- Câu 1: Nhập dữ liệu đầu vào và cho biết số lượng dữ liệu huấn luyện (tương ứng với file
  adult.data.csv) và số lượng dữ liệu kiểm thử (tương ứng với file adult.test.csv)

- Câu 2: Tiến hành tiền xử lý dữ liệu:
  • Xóa những dòng có chứa dữ liệu trống, biết rằng dữ liệu trống được ký hiệu bằng
  dấu ‘?’.
  • Xóa cột final weight ‘fnlwgt’ trong dữ liệu huấn luyện vì cột này không có trong
  dữ liệu kiểm thử.
  • Nối dữ liệu huấn luyện và kiểm thử lại với nhau để phục vụ các bước tiếp theo.
- Câu 3: Khảo sát độ tương đồng giữa các cột với nhau bằng công thức Pearson. Những cột
  nào có giá trị tương đồng cao thì hãy loại bỏ.
- Câu 4: Tách các cột dữ liệu thành hai phần, một phần chứa các thuộc tính bình thường, một
  phần chứa riêng thuộc tính quyết định.
- Câu 5: Chuyển đổi các cột không phải dạng số về dạng one-hot vector để phù hợp với đầu
  vào của thư viện.
- Câu 6: Tách các dòng dữ liệu ra thành hai phần huấn luyện và kiểm thử như ban đầu.
- Câu 7: Xây dựng cây ID3 dựa trên dữ liệu huấn luyện và sau đó tiến hành kiểm thử kết quả
  của cây bằng ma trận nhầm lẫn. Biểu diễn cây vào trong kết quả thực hiện.
- Câu 8: Lặp lại yêu cầu 7 đối với cây CART.
- Câu 9: Xây dựng mô hình phân lớp bằng thuật toán Naïve Bayes và kiểm tra kết quả đạt được.
- Câu 10: So sánh kết quả của các mô hình trên.

## Bài 2: 4. (Lập trình - trang 26) Cho dữ liệu Red Wine Quality2, liên quan đến các mẫu rượu vang Vinho

Verde đỏ từ phía bắc Bồ Đào Nha. Mục tiêu của bài toán là mô hình hóa chất lượng
rượu dựa trên các chỉ số hóa lý đo đạc được.
Sử dụng câu lệnh sau để chia dữ liệu đầu vào thành hai phần huấn luyện 70% và
kiểm thử 30%.

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
```

Sinh viên hãy thực hiện lại từ yêu cầu 3 trở đi trong phần Yêu cầu lập trình, áp dụng
với dữ liệu trên. Lưu ý, thuộc tính quyết định là ‘quality’.

- Câu 3: Khảo sát độ tương đồng giữa các cột với nhau bằng công thức Pearson. Những cột
  nào có giá trị tương đồng cao thì hãy loại bỏ.
- Câu 4: Tách các cột dữ liệu thành hai phần, một phần chứa các thuộc tính bình thường, một
  phần chứa riêng thuộc tính quyết định.
- Câu 5: Chuyển đổi các cột không phải dạng số về dạng one-hot vector để phù hợp với đầu
  vào của thư viện.
- Câu 6: Tách các dòng dữ liệu ra thành hai phần huấn luyện và kiểm thử như ban đầu.
- Câu 7: Xây dựng cây ID3 dựa trên dữ liệu huấn luyện và sau đó tiến hành kiểm thử kết quả
  của cây bằng ma trận nhầm lẫn. Biểu diễn cây vào trong kết quả thực hiện.
- Câu 8: Lặp lại yêu cầu 7 đối với cây CART.
- Cau 9: Xây dựng mô hình phân lớp bằng thuật toán Naïve Bayes và kiểm tra kết quả đạt
  được.
- Câu 10: So sánh kết quả của các mô hình trên.

## Bài 3:(trang 26) 5. (Lập trình) Cho dữ liệu Heart Disease3 chứa 14 thuộc tính thu thập ở Cleveland, Mỹ.

Thuộc tính ‘target’ là số nguyên có giá trị 0 (không có sự hiện diện) hoặc 1 (có bệnh
tim ở bệnh nhân).
a) Nhập dữ liệu đầu vào và tiến hành tiền xử lý xóa các dòng dữ liệu trống, sửa tên
các cột dữ liệu lại như sau để dễ theo dõi: 'age', 'sex', 'chest_pain_type',
'resting_blood_pressure', 'cholesterol', 'fasting_blood_sugar', 'rest_ecg',
'max_heart_rate_achieved', 'exercise_induced_angina', 'st_depression', 'st_slope',
'num_major_vessels', 'thalassemia', 'target'.
b) Thực hiện lại từ yêu cầu 3 trở đi trong phần Yêu cầu lập trình, áp dụng với dữ liệu
trên. Lưu ý, thuộc tính quyết định là ‘target’. (Các yêu cầu này giống với các yêu cầu của bài 2)

## Bài 4: (trang 27) 6. (Lập trình) Cho dữ liệu Mushroom4, chứa các thông tin đặc điểm của nhiều loại nấm

cùng với phân loại ‘class’ là nấm độc (poisonous=p) hay ăn được (edible=e).
Sử dụng câu lệnh sau để chia dữ liệu đầu vào thành hai phần huấn luyện 70% và
kiểm thử 30%.
Sinh viên hãy thực hiện lại từ yêu cầu 4 trở đi trong phần Yêu cầu lập trình, áp dụng
với dữ liệu trên. Lưu ý, thuộc tính quyết định là ‘class’.(các yêu cầu của bài 2)

## Bài 5: (trang 27-31): (Lập trình) Xây dựng ứng dụng nhận dạng số viết tay dựa trên dữ liệu MNIST5. Đây

là thư viện chữ số viết tay được gán nhãn sẵn và được hỗ trợ trực tiếp trong sklearn.
Sinh viên có thể viết ứng dụng nhận dạng theo các bước sau.
Chuẩn bị các thư viện cần thiết
Download dữ liệu có sẵn trong thư viện sklearn
Biểu diễn 4 mẫu dữ liệu huấn luyện tương ứng với 4 lớp 0, 1, 2, 3

## Bài 6: (trang 32) Thông thường để dễ so sánh hai giá trị độ chính xác (precision) và độ phủ (recall)

của những phương pháp với nhau người ta sử dụng một giá trị gọi là F-measure. Fmeasure là trung bình điều hòa (harmonic mean) của hai giá trị độ chính xác và độ
phủ được tính bằng công thức:
𝐹 − 𝑚𝑒𝑎𝑠𝑢𝑟𝑒 = 2 × 𝑝𝑟𝑒𝑐𝑖𝑠𝑖𝑜𝑛 × 𝑟𝑒𝑐𝑎𝑙𝑙
𝑝𝑟𝑒𝑐𝑖𝑠𝑖𝑜𝑛 + 𝑟𝑒𝑐𝑎𝑙𝑙
Hãy tính giá trị F-measure của các bài tập cơ bản.
SV có thể lập trình để cho ra kết quả
