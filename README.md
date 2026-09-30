# HỆ THỐNG QUẢN LÝ NHÂN SỰ VÀ NGHỈ PHÉP (HR & LEAVE MANAGEMENT SYSTEM)

Dự án phần mềm hoàn chỉnh dành cho đồ án đại học / hệ thống doanh nghiệp thực tế, tích hợp kiến trúc phân tầng chuẩn mực giữa **Frontend Next.js (TypeScript, Tailwind CSS, shadcn/ui)**, **Backend Python (FastAPI, SQLAlchemy ORM, Pydantic v2)** và **Trung tâm Mô hình hóa UML 2.5 Activity Diagrams trực quan**.

---

## 1. CÔNG NGHỆ SỬ DỤNG (TECH STACK)

- **Frontend**:
  - Framework: **Next.js 14** (App Router)
  - Ngôn ngữ: **TypeScript** (Kiểu dữ liệu tĩnh, type-safe 100%)
  - Giao diện: **Tailwind CSS**, phong cách **shadcn/ui**, biểu tượng **Lucide React**
  - Quản lý trạng thái & Auth: **React Context API**, JWT lưu trữ an toàn
  - Trực quan hóa quy trình: **Interactive Canvas UML Activity Diagram Viewer** (hỗ trợ Pan, Zoom, Bước mô phỏng, Phân vùng Swimlane)
- **Backend**:
  - Framework: **Python FastAPI** (Chuẩn RESTful API, tự động sinh tài liệu Swagger UI & ReDoc)
  - Xác thực & Phân quyền: **JWT (JSON Web Token)** với thuật toán mã hóa `HS256`, băm mật khẩu chuẩn bảo mật `bcrypt`
  - ORM & Validation: **SQLAlchemy 2.0** + **Pydantic v2**
- **Cơ sở dữ liệu**:
  - **SQLite / SQLAlchemy** (Không yêu cầu cài đặt server phức tạp, tính toàn vẹn quan hệ chặt chẽ với Khóa ngoại Foreign Keys, hỗ trợ chuyển đổi sang PostgreSQL / SQL Server chỉ qua 1 biến cấu hình `DATABASE_URL`).

---

## 2. TÀI KHOẢN TRẢI NGHIỆM HỆ THỐNG (DEMO ACCOUNTS)

Hệ thống đã nạp sẵn dữ liệu mẫu thực tế, hỗ trợ đăng nhập 1-click ngay trên trang Đăng nhập (`/login`):

| Vai trò (Role) | Username | Password | Nhân sự đại diện | Chức vụ | Phòng ban |
| :--- | :---: | :---: | :--- | :--- | :--- |
| **ADMIN** | `admin` | `123456` | Nguyễn Văn An | Giám Đốc Điều Hành | Ban Giám Đốc |
| **HR** | `hr_manager` | `123456` | Trần Thị Bích | Trưởng Phòng Nhân Sự | Phòng Nhân Sự |
| **MANAGER** | `it_manager` | `123456` | Lê Hoàng Long | Trưởng Phòng IT | Phòng Công Nghệ TT |
| **EMPLOYEE** | `emp_nam` | `123456` | Phạm Văn Nam | Kỹ Sư Phần Mềm Cao Cấp | Phòng Công Nghệ TT |
| **EMPLOYEE** | `emp_hoa` | `123456` | Hoàng Thu Hoa | Chuyên Viên Kinh Doanh | Phòng Kinh Doanh |

---

## 3. CẤU TRÚC THƯ MỤC DỰ ÁN

```
quanlynhansu/
├── backend/                             # Python REST API Server
│   ├── app/
│   │   ├── main.py                      # FastAPI app entrypoint, CORS & Routers mount
│   │   ├── config.py                    # Cấu hình Secret key, DB URL, CORS
│   │   ├── database.py                  # SQLAlchemy Engine & SessionLocal
│   │   ├── security.py                  # Hashing bcrypt & cấp phát JWT token
│   │   ├── dependencies.py              # Dependencies xác thực và RBAC
│   │   ├── seed_data.py                 # Script nạp dữ liệu mẫu ban đầu
│   │   ├── models/                      # 10 Mô hình ORM (User, Employee, Leave...)
│   │   ├── schemas/                     # Pydantic Schemas xác thực dữ liệu vào/ra
│   │   └── routers/                     # 10 REST API Controllers
│   ├── requirements.txt                 # Danh mục thư viện Python
│   ├── run.py                           # Khởi chạy máy chủ port 8000
│   └── test_backend.py                  # Bộ kiểm thử API endpoints tự động
│
├── frontend/                            # Next.js Web Application
│   ├── src/
│   │   ├── app/
│   │   │   ├── layout.tsx               # Root layout & AuthProvider
│   │   │   ├── login/page.tsx           # Trang đăng nhập với phím tắt chọn nhanh Role
│   │   │   └── (dashboard)/
│   │   │       ├── layout.tsx           # Shell Layout (Sidebar + Header + Breadcrumb)
│   │   │       ├── page.tsx             # Dashboard KPI & Chấm công nhanh
│   │   │       ├── employees/page.tsx   # Quản lý nhân viên (CRUD, tìm kiếm, lọc)
│   │   │       ├── departments/page.tsx # Quản lý phòng ban & cơ cấu tổ chức
│   │   │       ├── positions/page.tsx   # Quản lý chức vụ & phụ cấp
│   │   │       ├── contracts/page.tsx   # Quản lý hợp đồng lao động & mức lương
│   │   │       ├── attendance/page.tsx  # Chấm công check-in/out & lịch sử
│   │   │       ├── leaves/page.tsx      # Quản lý đơn nghỉ phép cá nhân & tạo đơn
│   │   │       ├── approvals/page.tsx   # Phê duyệt / từ chối đơn nghỉ phép
│   │   │       ├── reports/page.tsx     # Báo cáo & thống kê nhân sự chuyên sâu
│   │   │       └── diagrams/page.tsx    # Trung tâm mô hình hóa UML Activity Diagrams
│   │   ├── components/
│   │   │   ├── ui/                      # Button, Card, Badge, Modal, FormControls
│   │   │   ├── layout/                  # Sidebar, Header, Notifications
│   │   │   └── diagrams/                # ActivityViewer canvas, LoginActivity, etc.
│   │   ├── context/                     # AuthContext quản lý phiên đăng nhập
│   │   ├── lib/                         # api.ts (REST client), utils.ts
│   │   └── types/                       # Định nghĩa TypeScript toàn dự án
│   ├── package.json
│   ├── tailwind.config.ts
│   └── tsconfig.json
└── README.md
```

---

## 4. HƯỚNG DẪN KHỞI CHẠY (QUICK START)

### Bước 1: Khởi động Backend (Python)
```bash
cd backend
python -m pip install -r requirements.txt
python app/seed_data.py     # Nạp dữ liệu mẫu ban đầu (nếu cần)
python run.py               # Chạy server tại http://localhost:8000
```
- Tài liệu API tương tác Swagger UI: `http://localhost:8000/docs`
- Kiểm tra trạng thái máy chủ: `http://localhost:8000/api/health`

### Bước 2: Khởi động Frontend (Next.js)
```bash
cd frontend
npm install
npm run dev                 # Chạy website tại http://localhost:3000
```
Mở trình duyệt truy cập: `http://localhost:3000`

---

## 5. MÔ HÌNH HÓA UML ACTIVITY DIAGRAMS (CHUẨN UML 2.5)

Hệ thống trực quan hóa 6 quy trình nghiệp vụ cốt lõi ngay trên website:

1. **A. Activity Diagram — Đăng nhập hệ thống**:
   - Swimlane: `Người dùng` | `Giao diện Next.js` | `Backend FastAPI` | `Database`.
   - Kiểm tra thông tin -> Kiểm tra User tồn tại -> Xác thực mật khẩu Bcrypt -> Kiểm tra cờ kích hoạt `is_active` -> Sinh JWT Token -> Phân quyền truy cập Dashboard.
2. **B. Activity Diagram — Gửi đơn nghỉ phép**:
   - Swimlane: `Nhân viên` | `Giao diện` | `Backend` | `Database`.
   - Validate ngày kết thúc >= ngày bắt đầu -> Tra cứu `leave_balances` -> Kiểm tra số ngày phép còn lại -> Tạo đơn `PENDING` -> Tạm giữ số ngày phép (`pending_days`) -> Gửi thông báo tới Trưởng phòng.
3. **C. Activity Diagram — Duyệt đơn nghỉ phép**:
   - Swimlane: `Trưởng phòng/HR` | `Giao diện` | `Backend` | `Database` | `Nhân viên nhận`.
   - Xem xét đơn -> Nếu ĐỒNG Ý: chuyển `APPROVED`, trừ chính thức vào `used_days` -> Nếu TỪ CHỐI: bắt buộc nhập lý do, chuyển `REJECTED`, hoàn trả lại `pending_days` -> Gửi thông báo cho nhân viên.
4. **D. Activity Diagram — Quản lý nhân viên**:
   - Thêm nhân viên (kèm tài khoản & quỹ phép), Chỉnh sửa, Vô hiệu hóa an toàn (Soft delete `RESIGNED`), Tìm kiếm & bộ lọc.
5. **E. Activity Diagram — Quản lý phòng ban**:
   - Quản lý sơ đồ phòng ban, bổ nhiệm trưởng phòng, ràng buộc không cho xóa phòng ban đang có nhân sự trực thuộc.
6. **F. Activity Diagram — Chấm công thời gian thực**:
   - Check-in: Phân loại đúng giờ / đi muộn (mốc 08:30:00), ngăn chặn check-in lặp lại -> Check-out: Tính toán tổng giờ làm việc (`work_hours`).
"# quanlynhansu" 
