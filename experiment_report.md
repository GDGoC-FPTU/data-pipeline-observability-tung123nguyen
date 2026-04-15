# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-202600247
**Name:** Nguyễn Xuân Tùng
**Date:** 15/4/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario                          | Agent Response                                                          | Accuracy (1-10) | Notes                                                   |
| --------------------------------- | ----------------------------------------------------------------------- | --------------- | ------------------------------------------------------- |
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200.            | 9               | Dữ liệu đã qua pipeline, category chuẩn hóa, giá hợp lệ |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 3               | Dữ liệu nhiễu chứa giá sai, duplicate ID, null category |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Dữ liệu nhiễu đã làm hỏng kết quả của agent vì nó không được kiểm tra hoặc loại bỏ trước khi đưa vào mô hình. Trong `garbage_data.csv` có nhiều vấn đề:

- Duplicate ID: hai bản ghi có cùng `id` = 1, làm mất tính duy nhất trong bộ dữ liệu và dễ gây nhầm lẫn khi hệ thống tìm kiếm.
- Wrong data types: giá trị `ten dollars` trong cột `price` không phải số, khiến pandas đọc cột này là chuỗi và làm sai lệch tính toán.
- Outlier / extreme value: bản ghi `Nuclear Reactor` có giá 999999, là một ngoại lệ lớn và được chọn làm kết quả tốt nhất theo thuật toán đơn giản của agent.
- Null values / missing data: bản ghi không có `id` và không có `category`, chứng tỏ dữ liệu đầu vào không đầy đủ.

Những lỗi này trực tiếp dẫn tới phản hồi sai của agent vì `agent_simulation.py` chỉ tìm giá trị lớn nhất trong nhóm category `electronics` mà không kiểm tra tính hợp lệ của từng dòng. Do đó, một bản ghi nhiễu với giá cực lớn vẫn được chọn, trong khi các sản phẩm điện tử hợp lệ khác bị bỏ qua.

Bên cạnh đó, pipeline trong `solution.py` cho thấy kiểm tra chất lượng dữ liệu là bước quan trọng: khi chạy với dữ liệu sạch, 3 bản ghi được giữ lại và 2 bản ghi bị loại do giá <= 0 hoặc thiếu category.

---

## 3. Ket luan

**Quality Data > Quality Prompt?**

Đồng ý. Một prompt tốt không bù đắp cho dữ liệu xấu. Nếu dữ liệu đầu vào nhiễu, thiếu hoặc sai định dạng, agent vẫn có thể trả lời sai dù prompt rõ ràng.

Kết luận: chất lượng dữ liệu là yếu tố nền tảng trong pipeline AI/Data. Data observability và validation giúp phát hiện và loại bỏ dữ liệu không hợp lệ trước khi chuyển sang giai đoạn truy vấn hoặc huấn luyện, giảm rủi ro trả lời sai và nâng cao độ tin cậy của hệ thống.
