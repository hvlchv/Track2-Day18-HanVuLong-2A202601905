# Reflection — Lakehouse Anti-Pattern

Anti-pattern mà dữ liệu của nhóm tôi dễ gặp nhất là **quá nhiều file nhỏ**. Pipeline AI observability thường ghi log theo micro-batch; nếu mỗi batch tạo một file, số object sẽ tăng liên tục. Trong lab, 200 micro-batch tạo 200 file nhỏ. Sau compaction, số file giảm còn 11 (18 lần). Clustering cũng giúp point query chỉ cần mở 1/10 file, thay vì đọc toàn bộ.

Rủi ro không chỉ là truy vấn chậm. Nhiều file làm tăng số request đến object storage, phình transaction log và metadata, kéo dài thời gian lập kế hoạch truy vấn, đồng thời làm chi phí maintenance tăng. Managed service cũng không loại bỏ nguyên nhân này vì chi phí compaction vẫn phụ thuộc số byte và số object được xử lý.

Nhóm tôi sẽ kiểm soát kích thước batch, theo dõi số file và kích thước file trung bình, đặt mục tiêu file 128–512 MB, rồi chạy compaction và clustering theo lịch dựa trên ngưỡng thay vì chờ hệ thống chậm mới xử lý.
