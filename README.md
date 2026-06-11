# 🌤️ Phân Loại Hình Ảnh Thời Tiết Bằng Học Kết Hợp (Weather Image Classification using Ensemble Learning)

Dự án này ứng dụng các kỹ thuật Học sâu (Deep Learning) và Học kết hợp (Ensemble Learning) để phân loại tự động 11 hiện tượng thời tiết khác nhau thông qua hình ảnh.

## 📋 Giới Thiệu
Mục tiêu của dự án là xây dựng một hệ thống nhận diện thời tiết có độ chính xác và tính ổn định cao. Bằng cách tận dụng sức mạnh của kỹ thuật **Stacking Ensemble**, hệ thống sử dụng một mô hình Meta-Classifier để tổng hợp và tối ưu hóa các dự đoán từ 4 kiến trúc Mạng nơ-ron tích chập (CNN) cơ sở, qua đó khắc phục hiệu quả hiện tượng nhiễu và sự nhầm lẫn giữa các lớp thời tiết có đặc điểm thị giác tương đồng.

## 📊 Tập Dữ Liệu (Dataset)
Tập dữ liệu bao gồm hình ảnh của **11 lớp thời tiết** khác nhau:
1. `dew` (Sương)
2. `fogsmog` (Sương mù / Khói bụi)
3. `frost` (Sương giá)
4. `glaze` (Băng mỏng)
5. `hail` (Mưa đá)
6. `lightning` (Sấm sét)
7. `rain` (Mưa)
8. `rainbow` (Cầu vồng)
9. `rime` (Sương muối)
10. `sandstorm` (Bão cát)
11. `snow` (Tuyết)

## 🏗️ Kiến Trúc Hệ Thống
Hệ thống được chia làm 2 cấp độ chính:

### Level 0: Các mô hình cơ sở (Base Classifiers)
Bao gồm 4 kiến trúc mạng CNN có nhiệm vụ trích xuất đặc trưng hình ảnh và xuất ra 44 xác suất dự đoán (11 xác suất/mô hình):
* **Custom CNN** (Mô hình CNN tự xây dựng)
* **ResNet-50** (Áp dụng Transfer Learning)
* **EfficientNet-B3** (Áp dụng Transfer Learning)
* **CNN Cải tiến**

### Level 1: Mô hình giám khảo (Meta-Classifier)
Dữ liệu xác suất từ Level 0 được ghép nối (concatenate) thành vector 44 chiều và đưa vào Meta-Classifier để đưa ra dự đoán cuối cùng. Nhóm đã thử nghiệm 3 thuật toán:
* **Decision Tree** (`max_depth=10`)
* **Random Forest** (`n_estimators=200, max_depth=15`)
* **Multi-Layer Perceptron - MLP** (Mạng nơ-ron Keras với lớp ẩn 32 units, Dropout 0.2)

## 🏆 Kết Quả Thực Nghiệm

| Phương pháp | Thuật toán Meta-Model | Độ chính xác (Accuracy) | F1-Score (Macro Avg) |
| :--- | :--- | :---: | :---: |
| Weighted Voting | Tổng tuyến tính cố định | 92.87% | 0.93 |
| Stacking Ensemble | Decision Tree | 93.82% | 0.95 |
| Stacking Ensemble | Random Forest | 96.36% | 0.97 |
| **Stacking Ensemble**| **Mạng nơ-ron MLP (Keras)**| **94.91%** | **0.96** |
