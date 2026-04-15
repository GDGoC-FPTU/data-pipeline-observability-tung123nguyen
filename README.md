[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23574281&assignment_repo_type=AssignmentRepo)

# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** tung.nx20042024@gmail.com
**Name:** Nguyễn Xuân Tùng

---

## Mo ta

Trong bài lab Day 10, em xây dựng một Data Pipeline theo mô hình ETL (Extract – Transform – Load) và áp dụng Data Observability (giám sát chất lượng dữ liệu) để đánh giá độ ổn định của hệ thống khi xử lý dữ liệu sạch và dữ liệu nhiễu.

Các bước em đã thực hiện:

Thiết lập môi trường Python, cài đặt thư viện cần thiết và clone repository của bài lab.
Hoàn thiện file solution.py để xây dựng pipeline xử lý dữ liệu gồm 4 bước:

1. Extract

Đọc dữ liệu JSON từ file raw_data.json.
Xử lý lỗi khi file không tồn tại.

2. Validate (Data Quality Check)

Kiểm tra các bản ghi có:
Price > 0
Có category hợp lệ
Các bản ghi sai được log lại để phục vụ observability.

3. Transform

Tạo cột discounted_price = price \* 0.9
Chuẩn hoá định dạng category
Thêm metadata processed_at để theo dõi thời gian xử lý dữ liệu.

4. Load

Xuất dữ liệu đã xử lý ra file processed_data.csv.

Ngoài ra em còn:

Tạo dữ liệu nhiễu bằng generate_garbage.py.
Chạy stress test bằng agent_simulation.py trên:
Clean data
Garbage data
Quan sát kết quả để hiểu vai trò của Data Observability trong việc phát hiện dữ liệu lỗi và đảm bảo pipeline hoạt động ổn định.

Bài lab giúp em hiểu rõ cách xây dựng một data pipeline cơ bản và tầm quan trọng của việc giám sát chất lượng dữ liệu trong hệ thống AI/Data.

---

## Cach chay (How to Run)

### Prerequisites

```bash
pip install pandas
```

### Chay ETL Pipeline

```bash
python solution.py
```

Sau khi chay xong, file `processed_data.csv` se duoc tao ra tu `raw_data.json`.

### Tao du lieu nhiem (Garbage Data)

```bash
python generate_garbage.py
```

Lenh tren se tao file `garbage_data.csv` co nhieu loi thuong gap nhu duplicate ID, wrong type, outlier, va null values.

### Chay Agent Simulation (Stress Test)

```bash
python agent_simulation.py
```

Script nay se test hai bo du lieu:

- `processed_data.csv` (clean data)
- `garbage_data.csv` (noisy data)

Nó se in ra phan hoi cua agent cho tung bo du lieu va cho thay su khac biet giua du lieu sach va du lieu nhiễu.

---

## Cau truc thu muc

```
├── agent_simulation.py      # Simulation script kiểm tra agent với clean và garbage data
├── generate_garbage.py      # Tạo bộ dữ liệu garbage_data.csv có lỗi chất lượng dữ liệu
├── garbage_data.csv         # Dữ liệu nhiễu dùng để thử nghiệm observability
├── raw_data.json            # Dữ liệu thô đầu vào cho pipeline
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output của pipeline sau khi chạy solution.py
├── experiment_report.md     # Báo cáo thí nghiệm
└── README.md                # File này
```

---

## Ket qua

Khi chay `solution.py`, pipeline da xu ly thanh cong 3 ban ghi va loai bo 2 ban ghi khong hop le.

- `raw_data.json` ban dau chua 5 ban ghi.
- `processed_data.csv` duoc tao voi 3 ban ghi hop le.
- 2 ban ghi bi loai bo vi loi: mot ban ghi co `price <= 0`, mot ban ghi thieu `category`.

Khi chay `agent_simulation.py`:

- Voi `processed_data.csv`, agent tra loi: "Agent: Based on my data, the best choice is Laptop at $1200."
- Voi `garbage_data.csv`, agent tra loi: "Agent: Based on my data, the best choice is Nuclear Reactor at $999999."

Ket qua nay cho thay rang du lieu chat luong cao la yeu to quan trong de dam bao agent khong bi anh huong boi data nhiem.
