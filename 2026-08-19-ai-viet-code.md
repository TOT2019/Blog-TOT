---
layout: post
title: "AI viết code nhanh hơn, nhưng ai chịu trách nhiệm khi lỗi?"
date: 2026-08-19
---
# AI viết code nhanh hơn, nhưng ai chịu trách nhiệm khi lỗi?

Bạn giao một task cho AI, code chạy pass test local sau năm phút. Đến buổi review chiều cùng ngày, tech lead phát hiện đoạn xử lý transaction thiếu rollback, và không ai nhớ chính xác dòng đó do AI sinh ra ở bước nào. Đây không phải tình huống hiếm khi dùng **AI viết code**: tốc độ tạo mã tăng rõ rệt, nhưng phần trách nhiệm khi mã sai lại không tự động đi kèm.

Vấn đề không nằm ở việc AI có viết được code hay không. Các mô hình ngôn ngữ lớn hiện tại đã chứng minh khả năng sinh mã cho phần lớn tác vụ CRUD, script xử lý dữ liệu hay boilerplate. Vấn đề nằm ở khoảng cách giữa **code chạy được** và **code đúng nghiệp vụ, an toàn để đưa vào production**. Khoảng cách đó thường bị bỏ qua khi team chạy theo tốc độ demo.

Bài viết này không liệt kê lại tính năng marketing của từng công cụ. Nó tập trung vào cách một lập trình viên có kinh nghiệm nên đặt câu hỏi, kiểm soát và sử dụng AI như một cộng sự thực sự, thay vì một cỗ máy tạo code tự động rồi copy paste.

## Vì sao code do AI tạo ra thường qua được test nhưng vẫn tiềm ẩn rủi ro

Một mô hình AI dự đoán token tiếp theo dựa trên xác suất, không dựa trên việc hiểu ý định nghiệp vụ của bạn. Điều này tạo ra một nhóm lỗi đặc trưng, khác hẳn lỗi cú pháp thông thường.

- **Đúng cú pháp, sai logic nghiệp vụ**: hàm tính giảm giá chạy không lỗi nhưng áp dụng sai điều kiện biên khi percent bằng 0 hoặc vượt 100.
- **Thiếu ngữ cảnh bảo mật**: AI có xu hướng tạo query nối chuỗi trực tiếp nếu prompt không nêu rõ yêu cầu dùng prepared statement.
- **Ảo giác API (hallucination)**: gợi ý gọi một method hoặc tham số không tồn tại trong phiên bản thư viện đang dùng, đặc biệt với các package cập nhật nhanh.
- **Sao chép pattern lỗi thời**: mô hình được huấn luyện trên dữ liệu công khai, bao gồm cả code cũ chứa best practice đã lạc hậu.

Điểm chung của bốn nhóm lỗi này là chúng thường không làm chương trình crash ngay lập tức. Test đơn vị viết vội có thể vẫn xanh, CI vẫn pass, nhưng lỗi chỉ lộ ra khi dữ liệu thực tế đi vào một nhánh điều kiện mà AI chưa từng được cung cấp ngữ cảnh.

## Ba nguyên tắc dùng AI viết code mà không đánh đổi chất lượng

### Không giao cả hệ thống, chỉ giao từng module có ranh giới rõ

Yêu cầu AI xây dựng nguyên một tính năng đăng nhập trong một prompt duy nhất gần như chắc chắn bỏ sót bước hash password hoặc xử lý token hết hạn. Cách kiểm soát rủi ro tốt hơn là chia nhỏ: xác thực, sinh token, middleware bảo vệ route, mỗi phần một prompt, kèm review riêng trước khi ghép lại.

### Bắt AI lập kế hoạch trước khi viết code

Yêu cầu mô hình mô tả luồng xử lý, các trường hợp biên và cấu trúc dữ liệu trước khi sinh mã thật sự giúp lộ ra thiếu sót sớm hơn, khi chi phí sửa còn thấp. Đây là bước bị bỏ qua nhiều nhất bởi những người mới dùng AI viết code, vì cảm giác gõ prompt và nhận code ngay tạo ảo tưởng về tốc độ.

### Review như review code của một junior mới vào team

AI không có bối cảnh tổ chức, không biết lịch sử tại sao một đoạn logic cũ được viết theo cách kỳ lạ. Vì vậy code AI tạo ra cần qua đúng quy trình review, kiểm thử biên và đánh giá hiệu năng như code do người viết, không có ngoại lệ.

Để tránh mất thời gian thử từng công cụ một cách ngẫu nhiên, nhiều lập trình viên chọn tham khảo trước các bảng so sánh tổng hợp về [ai viết code](https://topon.tech/vi/ai-viet-code/), nơi liệt kê chi tiết thế mạnh, giới hạn và mức giá của từng nền tảng theo từng nhóm nhu cầu, thay vì phải tự cài đặt và test thủ công tất cả.

## So sánh nhanh các nhóm công cụ AI viết code theo mục đích sử dụng

Không có công cụ nào tối ưu cho mọi trường hợp. Bảng dưới đây phân loại theo mục đích sử dụng thực tế thay vì chỉ liệt kê tên thương hiệu.

| Nhóm công cụ | Thế mạnh chính | Rủi ro cần lưu ý | Phù hợp với |
|---|---|---|---|
| Trợ lý gợi ý trong IDE | Hoàn thiện code theo ngữ cảnh file đang mở, tốc độ phản hồi nhanh | Ít hiểu kiến trúc toàn dự án | Coding hằng ngày, tác vụ nhỏ |
| Agent làm việc đa tệp | Đọc cấu trúc dự án, sửa nhiều file, chạy lệnh và test | Cần review kỹ vì thay đổi diện rộng cùng lúc | Refactor lớn, tính năng phức tạp |
| Công cụ dòng lệnh mã nguồn mở | Chủ động chọn mô hình, tích hợp Git tốt, kiểm soát chi phí | Yêu cầu quen thuộc với terminal | Developer có kinh nghiệm, tự triển khai |
| Nền tảng kiểm tra chất lượng code | Phát hiện lỗi logic, vi phạm chuẩn khi review pull request | Không thay thế công cụ sinh code | Team cần chuẩn hóa quy trình review |

Nhìn vào bảng này, câu hỏi đúng không phải "công cụ nào tốt nhất" mà là "công cụ nào giải quyết đúng khâu đang tắc nghẽn trong quy trình của team mình".

## Cách viết prompt để giảm tỷ lệ AI đoán sai yêu cầu

Chất lượng code AI tạo ra tỷ lệ thuận với lượng ngữ cảnh được cung cấp, không phải với độ dài prompt. Một prompt hiệu quả thường có bốn phần: vai trò AI cần đóng, bối cảnh dự án hiện tại, ràng buộc kỹ thuật không được phá vỡ, và định dạng đầu ra mong muốn.

Ví dụ, thay vì viết "viết API login", nên nêu rõ: dự án dùng Node.js và MongoDB, cấu trúc user gồm email và password đã hash bằng bcrypt, endpoint cần trả JWT token và tuyệt đối không trả password trong response. Với lượng ngữ cảnh này, AI ít có cơ hội tự suy đoán sai một chi tiết bảo mật quan trọng.

Với debug, nguyên tắc tương tự áp dụng: cung cấp thông báo lỗi đầy đủ, kết quả mong muốn, kết quả thực tế và đoạn code liên quan, thay vì chỉ dán một dòng lỗi rời rạc rồi hỏi "sao code lỗi vậy".

## Câu hỏi thường gặp

**AI viết code có thay thế được lập trình viên không?**
Không, ít nhất ở giai đoạn hiện tại. AI xử lý tốt các tác vụ có cấu trúc rõ, lặp lại hoặc cần bản nháp nhanh. Các quyết định kiến trúc, đánh đổi kỹ thuật và trách nhiệm với hệ thống production vẫn cần con người kiểm soát.

**Làm sao biết code AI tạo ra có an toàn để đưa vào production không?**
Không có cách nào ngoài kiểm thử và review đầy đủ như với code người viết: chạy unit test, kiểm tra edge case, rà soát theo checklist bảo mật cơ bản như OWASP Top 10 trước khi merge.

**Có nên dùng AI viết code cho dự án doanh nghiệp có dữ liệu nhạy cảm không?**
Có thể, nhưng cần kiểm tra chính sách xử lý dữ liệu của từng công cụ, ưu tiên các giải pháp có tùy chọn triển khai riêng hoặc cam kết không dùng code nội bộ để huấn luyện lại mô hình.
