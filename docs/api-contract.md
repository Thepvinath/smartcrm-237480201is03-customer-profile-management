API-01 — Tạo hồ sơ khách hàng
Liên kết: UC1 – Tạo hồ sơ khách hàng
Method: POST
URL:
/api/customers
Mục đích:
Tạo một hồ sơ khách hàng mới trong hệ thống.
Request JSON:
{
  "ho_ten": "Nguyen Van A",
  "so_dien_thoai": "0901234567",
  "email": "nguyenvana@example.com",
  "dia_chi": "Ho Chi Minh City"
}

Response thành công — 201 Created:
{
  "customer_id": 1001,
  "ho_ten": "Nguyen Van A",
  "so_dien_thoai": "0901234567",
  "message": "Tạo hồ sơ khách hàng thành công"
}

Validation:
- ho_ten: bắt buộc.
- so_dien_thoai: bắt buộc.
- Số điện thoại phải được chuẩn hóa trước khi lưu.
- Số điện thoại không được trùng với hồ sơ hiện có.
Business Rule ใน Case Study รองรับเรื่อง phone unique และ normalize phone โดยตรง     7320c64b-97de-466c-bbeb-3bfe719…
HTTP Status: 201, 400, 409, 500
API-02 — Tra cứu hồ sơ khách hàng
Liên kết: UC2 – Tra cứu hồ sơ khách hàng
Method: GET
/api/customers?keyword={keyword}

Mục đích:
Tra cứu hồ sơ khách hàng theo họ tên hoặc số điện thoại.
ตัวอย่าง:
GET /api/customers?keyword=0901234567

Response — 200 OK:
{
  "data": [
    {
      "customer_id": 1001,
      "ho_ten": "Nguyen Van A",
      "so_dien_thoai": "0901234567"
    }
  ]
}

ถ้าไม่พบ:
{
  "data": [],
  "message": "Không tìm thấy hồ sơ khách hàng"
}

HTTP Status: 200, 400, 500
API-03 — Xem chi tiết hồ sơ khách hàng
Liên kết: UC3 – Xem chi tiết hồ sơ khách hàng
Method: GET
/api/customers/{id}

ตัวอย่าง:
GET /api/customers/1001

Mục đích:
Xem thông tin chi tiết của một hồ sơ khách hàng.
Response — 200 OK:
{
  "customer_id": 1001,
  "ho_ten": "Nguyen Van A",
  "so_dien_thoai": "0901234567",
  "email": "nguyenvana@example.com",
  "dia_chi": "Ho Chi Minh City"
}

Nếu không tìm thấy:
{
  "message": "Không tìm thấy hồ sơ khách hàng"
}

HTTP Status: 200, 404, 500
API-04 — Phát hiện hồ sơ có khả năng bị trùng
Liên kết: UC4 – Phát hiện hồ sơ có khả năng bị trùng
Method: GET
/api/customers/duplicates

Mục đích:
Hiển thị các hồ sơ khách hàng có khả năng bị trùng để người dùng kiểm tra và so sánh.
Response — 200 OK:
{
  "data": [
    {
      "customer_id": 1001,
      "ho_ten": "Nguyen Van A",
      "so_dien_thoai": "0901234567"
    },
    {
      "customer_id": 1058,
      "ho_ten": "Nguyen Van A",
      "so_dien_thoai": "0901234567"
    }
  ]
}

HTTP Status: 200, 500

API-05 — Gộp hồ sơ khách hàng trùng
Liên kết: UC5 – Gộp hồ sơ khách hàng trùng
Method: POST
/api/customers/merge

Mục đích:
Gộp các hồ sơ được xác nhận là trùng thành một hồ sơ khách hàng.
Request JSON:
{
  "primary_customer_id": 1001,
  "duplicate_customer_ids": [
    1058
  ]
}

Response — 200 OK:
{
  "customer_id": 1001,
  "merged_customer_ids": [
    1058
  ],
  "message": "Gộp hồ sơ khách hàng thành công"
}
Validation:
- primary_customer_id bắt buộc.
- duplicate_customer_ids bắt buộc.
- Hồ sơ chính phải tồn tại.
- Hồ sơ cần gộp phải tồn tại.
- Không được chọn cùng một hồ sơ vừa là hồ sơ chính vừa là hồ sơ trùng.
HTTP Status: 200, 400, 404, 409, 500