# Hướng dẫn sử dụng CodeRabbit và quy trình Review Code

## Quy trình tóm tắt

1. [**Quản lý tài khoản CodeRabbit** - Tạo tài khoản, thông báo đến team](#1-quản-lý-tài-khoản-coderabbit)
2. [**Reviewer cấu hình CodeRabbit** - Kiểm tra và cập nhật config phù hợp với dự án](#2-reviewer-cấu-hình-coderabbit)
3. [**Member thực hiện code và review local** - Cài extension, chạy CodeRabbit trên máy trước khi push](#3-member-thực-hiện-code-và-review-local)
4. [**Member tạo Pull Request** - Kiểm tra lại feedback từ CodeRabbit trên Pull Request](#4-member-tạo-pull-request)
5. [**Reviewer kiểm tra Pull Request** - Xem xét code và feedback từ CodeRabbit](#5-reviewer-kiểm-tra-pull-request)
6. [**Member sửa theo feedback** - Sửa code theo feedback (lặp lại bước 4-5 nếu cần)](#6-member-sửa-theo-feedback)

## Chi tiết từng bước

### 1. Quản lý tài khoản CodeRabbit

👤 Người thực hiện: Cấp quản lý

- Tạo tài khoản CodeRabbit và mua gói Subscription.
- Kết nối CodeRabbit với repository.
- Thêm tài khoản Project Leader với role Admin.

👤 Người thực hiện: Project Leader

- Thêm tài khoản của member trong dự án với role Member.
- Thông báo đến tất cả thành viên trong team về việc sử dụng CodeRabbit.
- Chuyển đổi seat giữa các member.

> **Note**: Hiện tại chỉ sử dụng trên các dự án thuộc tổ chức `briswell-ltd`.

### 2. Reviewer cấu hình CodeRabbit

👤 Người thực hiện: Reviewer.

- Kiểm tra file `.coderabbit.yaml` trong repository.
- Copy file coding standards phù hợp (`nodejs/CODING_STANDARDS.md` hoặc `php/CODING_STANDARDS.md`) vào thư mục gốc của dự án với tên `CODING_STANDARDS.md`, sau đó chỉnh sửa cho phù hợp với quy ước của dự án.
  + Loại bỏ các rule đã được ESLint hoặc công cụ lint khác của dự án kiểm tra để tránh CodeRabbit feedback trùng lặp.
- Cập nhật cấu hình cho phù hợp với từng dự án. Chủ yếu là các mục sau:
  + path_filters: Định nghĩa những file CodeRabbit sẽ review hoặc bỏ qua.
  + path_instructions: Thêm hướng dẫn cho CodeRabbit review theo từng rules (nếu cần).
  + pre_merge_checks: Sửa lại rules đặt tên cho Pull Request ở `title` (nếu cần).
  + knowledge_base: Đảm bảo `filePatterns` của `code_guidelines` có chứa `CODING_STANDARDS.md`. Khi thêm các file hướng dẫn khác, cần cập nhật lại mục này.

### 3. Member thực hiện code và review local

👤 Người thực hiện: Member.

#### 3.1. Cài đặt và sử dụng CodeRabbit extension

- Cài đặt CodeRabbit extension cho VSCode hoặc Cursor
- Chạy CodeRabbit review ngay trên máy local sau khi code xong một phần hoặc toàn bộ.
- Không được tạo Pull Request (Ready for review) khi chưa chạy review local.

Lưu ý: 
- Chỉ review các repository dự án được phép sử dụng CodeRabbit. Không dùng CodeRabbit review cho repository cá nhân hoặc repository không thuộc danh sách cho phép.

- Review local có thể bị rate limit, vì vậy chỉ nên chạy khi cần thiết. Khi gặp lỗi, chờ vài phút rồi thử lại.

#### 3.2. Xử lý các lỗi theo mức độ ưu tiên

Có các mức độ sau:

    🔴 CRITICAL - Các vấn đề nghiêm trọng có thể gây ra lỗi hệ thống, vi phạm bảo mật hoặc mất dữ liệu.
    🟠 MAJOR - Các vấn đề nghiêm trọng ảnh hưởng đến chức năng hoặc hiệu suất.
    🟡 MINOR - Các vấn đề cần được giải quyết nhưng không ảnh hưởng nghiêm trọng đến hệ thống.
    🔵 TRIVIAL - Các đề xuất tác động thấp để cải thiện chất lượng mã.
    ⚪ INFO - Các bình luận hoặc ngữ cảnh mang tính thông tin mà không yêu cầu hành động.

##### CRITICAL và CODING_STANDARD (BẮT BUỘC)

- Phải sửa hết tất cả các lỗi thuộc loại này.
- Nếu không rõ cách sửa lỗi CRITICAL → Hỏi Project Leader trước khi tạo Pull Request.
- Không được tạo Pull Request khi còn lỗi CRITICAL hoặc vi phạm CODING_STANDARD.

##### MAJOR / MINOR / TRIVIAL (KHUYẾN NGHỊ)

Không bắt buộc phải sửa ngay.
Nên xem xét và sửa nếu:

  + Rõ ràng về cách sửa.
  + Gợi ý hợp lý và cải thiện chất lượng code.
  + Nếu không chắc chắn → Hỏi Project Leader

#### 3.3. Kiểm tra lại toàn bộ code theo coding rules

### 4. Member tạo Pull Request

👤 Người thực hiện: Member.

#### 4.1. Kiểm tra lại trên Pull Request

- Sau khi tạo Pull Request, chọn label `coderabbit-review` để CodeRabbit tự động review.

  **Bắt buộc**: Nếu còn lỗi CODING_STANDARD và CRITICAL thì cần sửa.

#### 4.2. Trao đổi với CodeRabbit

Lưu ý: Luôn sử dụng tiếng Anh khi trao đổi với CodeRabbit.

Nguyên tắc khi trao đổi:

- Luôn cung cấp thông tin chính xác về quy tắc của dự án.
- Giải thích rõ ràng, cụ thể.
- Không đưa thông tin sai lệch vì CodeRabbit sẽ "học" và áp dụng cho các review sau.

Ví dụ: CodeRabbit review
```
⚠️ Potential issue | 🟠 Minor
The variable name `_` doesn't follow naming conventions. 
Variable names should not start with underscore. Consider renaming to `type`.

Suggested change:
- const { type: _, ...rest } = formModel;
+ const { type, ...rest } = formModel;
```

Feedback đúng:
```
@coderabbit This variable follows our team's naming convention for unused variables.
We use underscore (_) for values that are intentionally destructured but not used.
```

Feedback sai:
```
@coderabbit This is wrong.
❌ (Không giải thích rõ lý do)

@coderabbit We use snake_case for everything.
❌ (Sai sự thật - CodeRabbit sẽ học sai)
```

### 5. Reviewer kiểm tra Pull Request

👤 Người thực hiện: Reviewer.

Xem xét các gợi ý của CodeRabbit. Phân loại:

- Hợp lý: Giữ lại để Member sửa.
- Không hợp lý: Reject và giải thích lý do rõ ràng để CodeRabbit "học".

Ví dụ feedback tốt:
```
@coderabbit This suggestion doesn't apply. We use camelCase 
for all variable names according to our CODING_STANDARDS.md.
```

Ví dụ feedback không nên dùng:
```
This review is wrong. ❌
Not applicable. ❌
```

Note: Reviewer sẽ tiến hành review thủ công và tiến hành feedback để member sửa. Ngoài ra, có thể trao đổi thêm với CodeRabbit (sử dụng tiếng Anh).

Trường hợp đối với 1 rule nhưng CodeRabbit review sai nhiều lần, thì reviewer cần cập nhật lại phần `instruction` và `knowledge_base` của file `.coderabbit.yaml` (trong dự án).

### 6. Member sửa theo feedback

👤 Người thực hiện: Member.

Tiến hành sửa theo feedback của Reviewer như trước giờ sau đó push code mới lên. Lúc này:

  - CodeRabbit sẽ tự động review lại phần code mới (bước 4).
  - Reviewer kiểm tra lại cho đến khi đạt yêu cầu (bước 5).

Lặp lại cho đến khi tất cả feedback đã được xử lý và merge Pull Request.
