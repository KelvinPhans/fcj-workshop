---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu tuần 6:

- Ứng dụng kiến thức AWS và Generative AI vào một ứng dụng thực tế.
- Tích hợp dịch vụ Amazon Bedrock với các dịch vụ backend.
- Xây dựng và kiểm thử một API tích hợp trí tuệ nhân tạo (AI-powered API).

### Công việc thực hiện trong tuần:
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Xác định ý tưởng dự án và phân tích yêu cầu bài toán <br> - Quy định định dạng dữ liệu đầu vào (input) và đầu ra (output) <br> - Lựa chọn các dịch vụ AWS phù hợp cho dự án <br> - Thiết kế sơ đồ kiến trúc hệ thống cơ bản <br> - Khởi tạo mã nguồn dự án và thiết lập môi trường phát triển | 27/07/2026 | 27/07/2026 | |
| 2 | - Lập trình logic gửi prompt tới Foundation Model qua Amazon Bedrock <br> - Kết nối ứng dụng trực tiếp với Amazon Bedrock SDK <br> - Xử lý gửi request và giải mã (parse) dữ liệu phản hồi từ mô hình <br> - Thử nghiệm với các bộ dữ liệu đầu vào khác nhau | 28/07/2026 | 28/07/2026 | |
| 3 | - Viết mã nguồn cho xử lý backend trên AWS Lambda <br> - Tích hợp dịch vụ Amazon Bedrock vào hàm Lambda <br> - Cấu hình IAM Role với đủ quyền truy cập Bedrock cho Lambda <br> - Thực thi kiểm thử và khắc phục các lỗi liên quan đến phân quyền và runtime | 29/07/2026 | 29/07/2026 | |
| 4 | - Khởi tạo cổng API bằng dịch vụ Amazon API Gateway <br> - Tích hợp API Gateway với hàm Lambda backend <br> - Tạo endpoint cho ứng dụng và sử dụng Postman để kiểm thử API <br> - Viết logic xử lý các phản hồi lỗi từ API | 30/07/2026 | 30/07/2026 | |
| 5 | - Kiểm thử toàn bộ luồng hoạt động tích hợp của ứng dụng (End-to-End) <br> - Kiểm tra log chi tiết trên Amazon CloudWatch <br> - Tối ưu hóa cấu trúc prompt và kiểm tra hợp lệ dữ liệu nhập vào <br> - Sửa các lỗi phát sinh và hoàn thiện phiên bản ứng dụng đầu tiên | 31/07/2026 | 31/07/2026 | |


### Kết quả đạt được tuần 6:

- Thiết kế thành công kiến trúc hệ thống cho ứng dụng AI.
- Tích hợp thành công Amazon Bedrock vào xử lý backend.
- Sử dụng thành thạo AWS Lambda để xử lý logic ứng dụng.
- Kết nối thành công API Gateway với AWS Lambda.
- Triển khai thành công tính năng AI thông qua endpoint API.
- Sử dụng thành thạo CloudWatch Logs để kiểm vết gỡ lỗi.
- Tối ưu hóa chất lượng phản hồi từ mô hình AI.
- Hoàn thành phiên bản hoạt động đầu tiên của sản phẩm.
