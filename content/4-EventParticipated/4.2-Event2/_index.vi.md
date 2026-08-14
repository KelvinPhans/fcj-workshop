---
title: "Event 2 - Agentic AI Build Week"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Event 2 - FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!

## Tổng quan sự kiện

**FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!** là phiên chia sẻ giải pháp và báo cáo kết quả cuộc thi Hackathon do cộng đồng **AWS First Cloud AI Journey (FCAJ)** phối hợp cùng quỹ đầu tư **JI Fund** tổ chức.

Sự kiện quy tụ đông đảo các sinh viên, lập trình viên, kỹ sư điện toán đám mây và chuyên gia trí tuệ nhân tạo (AI). Tại sự kiện, các đội thi Hackathon đã trực tiếp trình diễn sản phẩm (demo), thuyết trình sơ đồ kiến trúc hệ thống, chia sẻ phương pháp triển khai thực tế và thảo luận về những thách thức khi xây dựng ứng dụng tích hợp Agentic AI.

Với nhiệm vụ phụ trách **Frontend React** và biên soạn **tài liệu Hugo Workshop** cho dự án Startups Blogs, mục tiêu tham dự của em là quan sát cách các đội biến ý tưởng thành sản phẩm AI hoạt động được trong khoảng 48 giờ. Em đặc biệt chú ý cách các đội trình bày luồng người dùng, giải thích kiến trúc AWS và xác định những yêu cầu bổ sung để đưa một bản Proof of Concept (PoC) tiến gần hơn đến môi trường Production.

---

## Mục tiêu đạt được khi tham gia

- Tìm hiểu khái niệm Agentic AI và các ứng dụng thực tế trong hệ thống phần mềm.
- Nắm vững quy trình phát triển từ bản mẫu PoC sang giải pháp có khả năng mở rộng (Scalable Solution).
- Học hỏi kinh nghiệm triển khai dự án thực tế từ các đội thi Hackathon.
- Hiểu rõ các yếu tố kỹ thuật cần thiết khi đưa ứng dụng AI vào môi trường Production.
- Quan sát cách kết hợp các dịch vụ Đám mây AWS trong một kiến trúc hệ thống AI hoàn chỉnh.

---

## Danh sách diễn giả tiêu biểu

- **Mr. Nguyễn Gia Hưng** – Head of Solution Architect, AWS Vietnam | Founder of AWS First Cloud AI Journey (FCAJ).
- **Mr. Joseph Marazota** – Head of Technology, Amazon ASEAN.
- Các đội thi xuất sắc tham gia Hackathon.

---

## Nội dung chính của sự kiện

Sự kiện tập trung chuyên sâu vào chủ đề **Agentic AI**. Khác với các ứng dụng Generative AI truyền thống chỉ phản hồi các câu lệnh prompt từ người dùng, một hệ thống Agentic AI có khả năng:
- Lập kế hoạch thực hiện tác vụ (Task planning).
- Tự động gọi và sử dụng các công cụ bên ngoài (Tool calling).
- Thực hiện quy trình xử lý đa bước (Multi-step workflows).
- Đánh giá kết quả trung gian để điều chỉnh hành vi.
- Tương tác với các thành phần khác trong hệ thống phần mềm.

Bên cạnh chức năng AI, các bài thi Hackathon còn được đánh giá dựa trên tiêu chí sẵn sàng cho môi trường Production:
- **Guardrails**: Kiểm soát và giới hạn hành vi của mô hình AI.
- **Role-Based Access Control (RBAC)**: Phân quyền truy cập dữ liệu nghiêm ngặt.
- **Human-in-the-loop**: Sự tham gia của con người trong các bước quyết định quan trọng.
- **Tối ưu hóa chi phí API (API Cost Optimization)**: Quản lý ngân sách gọi mô hình AI.
- **Scalability, Security & Reliability**: Khả năng mở rộng, bảo mật và độ tin cậy hệ thống.

Các đội thi đã hoàn thiện sản phẩm trong khoảng thời gian **48 giờ** và có thời lượng thuyết trình giới thiệu bài toán, ý tưởng giải pháp, kiến trúc AWS, quá trình triển khai và demo sản phẩm trực tiếp.

---

## Trình bày giải pháp tiêu biểu của các đội thi

### A. Agentic AI cho Hệ thống Đặt hàng Trực tuyến (Online Ordering)

- **Bài toán:** Các hệ thống đặt hàng trực tuyến truyền thống yêu cầu người dùng qua nhiều bước thủ công: tạo tài khoản, nhập thông tin thanh toán, duyệt menu và chọn món.
- **Giải pháp PoC:** Xây dựng AI Agent hỗ trợ đặt hàng qua giao tiếp ngôn ngữ tự nhiên.
- **Tính năng báo cáo:**
  - Tự động thu thập dữ liệu thực đơn từ website chính thức.
  - Lưu trữ dữ liệu liên quan trên hạ tầng AWS.
  - Lưu lịch sử đơn hàng và sở thích của người dùng.
  - Tự động tạo đơn hàng và thêm sản phẩm vào giỏ hàng qua luồng hội thoại.
- **Ý nghĩa:** Minh họa cách AI Agent có thể thay mặt người dùng thực thi tác vụ thay vì chỉ trả về văn bản phản hồi.

{{< blog-image src="images/events/agentic-ai-online-ordering-architecture.jpg" alt="Kiến trúc AWS của đội Agentic AI cho đặt món trực tuyến" caption="Hình 1. Kiến trúc AWS được trình bày bởi đội Agentic AI cho đặt món trực tuyến." >}}

---

### B. Agentic AI cho Phân tích Dữ liệu (Data Analysis)

- **Bài toán:** Chuyên viên phân tích dữ liệu (Data Analysts) tốn nhiều thời gian cho các báo cáo định kỳ và công việc phân tích lặp đi lặp lại.
- **Giải pháp PoC:**
  - Tiếp nhận yêu cầu phân tích dữ liệu qua câu lệnh.
  - Tự động tạo báo cáo phân tích ban đầu.
  - Tích hợp **Agent Loop** giúp cải thiện kết quả dựa trên phản hồi của chuyên viên.
  - Áp dụng **Guardrails** để kiểm tra và xác thực tính chính xác của dữ liệu trả về.
- **Ý nghĩa:** Minh họa mô hình hợp tác hiệu quả giữa con người (Data Analyst) và AI Agent.

{{< blog-image src="images/events/agentic-ai-data-analysis-architecture.jpg" alt="Kiến trúc AWS của đội Agentic AI cho phân tích dữ liệu" caption="Hình 2. Kiến trúc AWS được trình bày bởi đội Agentic AI cho phân tích dữ liệu." >}}

---

### C. Agentic AI cho Giám sát Lưu lượng Hành khách (Passenger Traffic Tracking)

- **Kiến trúc các dịch vụ AWS được đội thi sử dụng:**
  Amazon Kinesis Video Streams, Amazon ECS, Amazon ECR, Amazon SageMaker Endpoints, Amazon S3, Amazon DynamoDB, Amazon CloudFront, Amazon API Gateway, AWS Lambda, AgentCore Runtime, Amazon Bedrock, Amazon Cognito, AWS IAM, AWS Secrets Manager, AWS CloudTrail, Amazon CloudWatch.
- **Mô tả giải pháp:**
  - Luồng video/hình ảnh được thu nhận vào hệ thống qua Amazon Kinesis Video Streams.
  - Các thành phần xử lý (ECS/SageMaker) phân tích khung hình và trích xuất chỉ số lưu lượng hành khách.
  - Dữ liệu và kết quả phân tích được lưu trữ an toàn trên S3 và DynamoDB.
  - API Gateway và Lambda phục vụ dữ liệu cho ứng dụng Frontend qua CloudFront.
  - Tích hợp dịch vụ bảo mật (Cognito, IAM, Secrets Manager) và giám sát (CloudWatch, CloudTrail).
  - Thành phần Agentic AI (Bedrock / AgentCore) hỗ trợ truy vấn và phân tích thông minh trên dữ liệu lưu lượng đã xử lý.

*(Lưu ý: Sơ đồ kiến trúc trên thuộc bài trình bày của đội thi Hackathon tại sự kiện, không phải kiến trúc của dự án Startups Blogs).*

{{< blog-image src="images/events/agentic-ai-traffic-tracking-architecture.jpg" alt="Kiến trúc AWS của đội theo dõi lưu lượng hành khách" caption="Hình 3. Kiến trúc AWS được trình bày bởi đội theo dõi lưu lượng hành khách." >}}

---

## Kiến thức thu nhận được (Knowledge Gained)

- **Bản chất của Agentic AI:** Một AI Agent hiệu quả cần bao gồm lập kế hoạch, thực thi, sử dụng công cụ và đánh giá kết quả, thay vì chỉ dựa vào một câu prompt đơn lẻ.
- **Tầm quan trọng của Guardrails:** Cần thiết lập cơ chế kiểm soát (Guardrails) để đảm bảo mô hình AI hoạt động đúng phạm vi và an toàn.
- **Khoảng cách giữa PoC và Production:** Hệ thống PoC và Production khác biệt rất lớn về tiêu chuẩn bảo mật, khả năng mở rộng, độ tin cậy, giám sát (observability) và chi phí vận hành.
- **Giải quyết bài toán người dùng thực:** Công nghệ AI chỉ thực sự có giá trị khi giúp đơn giản hóa quy trình công việc thực tế của người dùng.
- **Tích hợp dịch vụ Đám mây AWS:** Cách kết hợp linh hoạt các dịch vụ AWS để tạo nên một kiến trúc AI end-to-end hoàn chỉnh.

---

## Ứng dụng tiềm năng vào dự án Startups Blogs (Future Possibilities)

*(Lưu ý: Đây là những định hướng nghiên cứu và khả năng mở rộng trong tương lai dựa trên bài học từ sự kiện. Hệ thống Startups Blogs hiện tại không triển khai Agentic AI, Amazon Bedrock hay SageMaker).*

- **Tìm kiếm thông minh:** Nghiên cứu khả năng hỗ trợ nhà đầu tư tìm kiếm các cơ hội gọi vốn qua truy vấn ngôn ngữ tự nhiên.
- **Hỗ trợ tạo nội dung:** Định hướng tính năng gợi ý cấu trúc mô tả doanh nghiệp và bài viết gọi vốn cho các Business Owner.
- **Tóm tắt thông tin công khai:** Hỗ trợ nhà đầu tư tóm tắt các thông tin doanh nghiệp và cơ hội đầu tư công khai trên nền tảng.
- **Hỗ trợ kiểm duyệt nội dung:** Nghiên cứu công cụ hỗ trợ Admin rà soát và kiểm duyệt bài viết tự động.
- **Áp dụng Guardrails & RBAC:** Đảm bảo tính năng AI trong tương lai tuân thủ phân quyền chặt chẽ, không truy cập trái phép vào dữ liệu riêng tư của nhà đầu tư hoặc doanh nghiệp.

---

## Trải nghiệm cá nhân (Personal Experience)

Đây là lần đầu tiên em có cơ hội quan sát một Hackathon quy mô lớn tập trung vào **Agentic AI**. Ở góc nhìn của người phụ trách Frontend, em chú ý cách mỗi đội chuyển một quy trình agent phức tạp thành giao diện dễ hiểu, có trạng thái rõ ràng và đủ thuyết phục trong thời gian demo ngắn.

Sự kiện cũng giúp em thay đổi cách biên soạn tài liệu kỹ thuật. Một phần trình bày tốt cần kết nối được bài toán người dùng, luồng thao tác, kiến trúc AWS, cơ chế bảo mật, rủi ro vận hành và kết quả đạt được thay vì chỉ đưa ra một sơ đồ độc lập. Em sẽ áp dụng bài học này khi tiếp tục hoàn thiện giao diện Startups Blogs và viết các bước thực hành trong Hugo Workshop.

---

## Bài học rút ra (Lessons Learned)

- Xuất phát từ quy trình thực tế của người dùng trước khi lựa chọn mô hình AI hoặc dịch vụ AWS.
- Thiết kế trạng thái Frontend rõ ràng cho tác vụ agent kéo dài, lỗi, bước xác nhận và điểm cần con người phê duyệt.
- Tài liệu phải thể hiện quan hệ giữa thao tác giao diện, Backend API và dịch vụ AWS thay vì mô tả các bước rời rạc.
- Xem RBAC, bảo vệ dữ liệu, observability và kiểm soát chi phí là yêu cầu Production ngay từ đầu.
- Phân biệt rõ tính năng Startups Blogs đã triển khai với những ý tưởng Agentic AI chỉ mới ở định hướng nghiên cứu.

<!-- Event 2 personal photos will be added later. -->

---

## Kết luận

Sự kiện **FCAJ x Agentic AI Build Week** đã mang lại những góc nhìn công nghệ rất hiện đại về Agentic AI và kiến trúc hệ thống Đám mây AWS. Những bài học về sự khác biệt giữa PoC và Production, việc áp dụng Guardrails, phân quyền RBAC và quản lý chi phí sẽ là nền tảng kiến thức quý báu cho em trong hành trình phát triển phần mềm và xây dựng các hệ thống scalable trong tương lai.
