Phân loại bệnh lá cây Cà phê bằng mô hình học sâu CNN

Đây là dự án thực hiện trong khuôn khổ **bài tập lớn môn Ứng dụng Trí tuệ Nhân tạo trong Nông nghiệp**, với mục tiêu nhận diện bệnh trên lá cây lúa bằng ảnh sử dụng mô hình học sâu Convolutional Neural Network (CNN).

##  Mục tiêu

- Sử dụng mô hình học sâu MobileNetV2 (transfer learning) để phân loại các loại bệnh trên lá cà phê.
- Sử dụng bộ dữ liệu ảnh thật từ Kaggle.
- Trực quan hóa dữ liệu, quá trình huấn luyện và hiệu suất mô hình.
- Đánh giá hiệu quả mô hình bằng Accuracy, Loss và Confusion Matrix.
- Dự đoán ảnh mới từ tập dữ liệu hoặc do người dùng tải lên.


##  Bộ dữ liệu

- **Nguồn**: RoCoLe: A Robusta Coffee Leaf Images Dataset: https://www.kaggle.com/datasets/nirmalsankalana/rocole-a-robusta-coffee-leaf-images-dataset/data)
- **Tổng số ảnh**: Tập dữ liệu bao gồm 1560 hình ảnh lá cà phê Robusta
  - Cấu trúc thư mục: mỗi lớp bệnh nằm trong một thư mục con, ví dụ
  - coffee_data/
    - coffee__healthy/
    - coffee__rust/
    - coffee__red_spider_mite/

- **Gồm 3 lớp bệnh**:
  - Cà phê khỏe mạnh: Hình ảnh lá cà phê khỏe mạnh, không có dấu hiệu bệnh tật.
  - Bệnh rỉ sắt trên cây cà phê: Hình ảnh lá cà phê bị nhiễm bệnh rỉ sắt do một loại nấm gây ra.
  - Nhện đỏ cà phê: Hình ảnh lá cà phê bị nhện đỏ cà phê xâm nhập, đây là loài côn trùng nhỏ có thể gây hại cho lá cà phê.
**Tỷ lệ chia dữ liệu**:
- 80% cho tập huấn luyện (Train)
- 20% cho tập kiểm định (Validation)

##  Kiến trúc mô hình

Base model: MobileNetV2 (không huấn luyện lại các tầng gốc)

Các tầng đầu ra:

- GlobalAveragePooling2D

- Dropout

- Dense với activation softmax để phân loại đa lớp

Huấn luyện:

- Optimizer: Adam

- Loss: CategoricalCrossentropy

- Epochs: 20

##  Kết quả huấn luyện

- Accuracy:
  Tập huấn luyện: Đạt khoảng 67% vào epoch cuối (tăng đều từ khoảng 55% ở epoch đầu).
  Tập validation: Dao động quanh 65–70%, với xu hướng tăng nhẹ nhưng không ổn định.
- Loss:
  Tập huấn luyện: Giảm mạnh từ 1.1 xuống khoảng 0.65 sau 10 epoch, cho thấy mô hình học tốt trên tập huấn luyện.
  Tập validation: Giảm từ 0.9 xuống khoảng 0.65, nhưng có dao động lớn trong các epoch đầu, sau đó ổn định hơn.
- Loss theo epoch: Đường màu xanh (train loss) giảm đều, trong khi đường màu cam (val loss) dao động nhưng có xu hướng giảm.
- Accuracy theo epoch: Đường màu xanh (train accuracy) tăng đều, trong khi đường màu cam (val accuracy) dao động quanh 65–70%.
##  Dự đoán ảnh thực tế
- Tải ảnh và thay đổi kích thước thành 224x224 pixels.
- Chuẩn hóa giá trị pixel (chia cho 255 để đưa về khoảng [0, 1]).
- Sử dụng mô hình MobileNetV2 để dự đoán, lấy nhãn lớp có xác suất cao nhất.
- Hiển thị ảnh cùng nhãn dự đoán

