# Quyết định (ADR rút gọn)

> File này trả lời: **vì sao chọn cách này**. Quyết định đã "Chấp nhận" thì không sửa nội dung; muốn đổi thì thêm mục mới và đánh dấu mục cũ "Thay thế bởi D-xxx".
> Mẫu: Trạng thái · Ngày → Bối cảnh → Phương án → Quyết định → Hệ quả.

## D-001 · Vật kiểm tra là vòng đệm kim loại
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Cần vật rẻ, dễ đo, dễ tạo lỗi thật, gần với kiểm tra chi tiết cơ khí trong nhà máy.
- **Phương án:** vòng đệm · ốc vít · nắp chai · linh kiện điện tử.
- **Quyết định:** Vòng đệm: hình tròn dễ đo chính xác, có cả đường kính ngoài/trong, lệch tâm; dễ tự làm hỏng; MVTec AD có nhóm `metal_nut` cùng chủ đề.
- **Hệ quả:** Thuật toán đo tối ưu cho hình tròn. Muốn mở rộng sang chi tiết khác (ốc, chi tiết dập) cần thêm bộ đo đường thẳng/góc.

## D-002 · Python để prototype, C# cho ứng dụng chạy thật
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Phần mềm vision trong nhà máy thường viết bằng C#. Nhưng thử thuật toán bằng Python nhanh hơn.
- **Phương án:** chỉ Python · chỉ C# · Python prototype + C# runtime.
- **Quyết định:** Python (OpenCV, anomalib) để thử và train; C# (.NET 10, WinForms, OpenCvSharp4) cho app; model chuyển qua ONNX.
- **Hệ quả:** Phải viết pipeline đo hai lần → bắt buộc có test so khớp kết quả Python và C# (P2-02).

## D-003 · Đèn nền (backlight) cho đo kích thước
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Kim loại phản sáng, ánh sáng chiếu trực tiếp làm biên không ổn định.
- **Phương án:** đèn nền · đèn vòng · đèn bàn chiếu trực tiếp.
- **Quyết định:** Đo kích thước bằng đèn nền (vật thành bóng đen, biên sắc). Lỗi bề mặt (P3) dùng đèn chiếu trực tiếp.
- **Hệ quả:** Đèn nền không thấy được lỗi bề mặt → cần hai chế độ chiếu sáng nếu làm P3.

## D-004 · Modbus TCP + PLC giả lập bằng pymodbus
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Không có PLC thật; cần thể hiện giao tiếp PLC kiểu nhà máy.
- **Phương án:** Modbus TCP · Siemens S7 (Snap7 + PLCSIM) · TCP socket tự định nghĩa.
- **Quyết định:** Modbus TCP: chuẩn mở, PLC hãng nào cũng hỗ trợ, giả lập dễ. App dùng NModbus; PLC giả lập dùng pymodbus.
- **Bản đồ thanh ghi:**

  | Địa chỉ | Loại | Ý nghĩa | Ai ghi |
  |---|---|---|---|
  | Coil 0 | bool | Trigger: yêu cầu kiểm tra | PLC |
  | Coil 1 | bool | Busy: đang xử lý | App |
  | Coil 2 | bool | Done: có kết quả | App |
  | HR 0 | int16 | Kết quả: 1 = OK, 2 = NG, 3 = lỗi | App |
  | HR 1 | int16 | Đường kính ngoài × 100 (mm) | App |
  | HR 2 | int16 | Đường kính trong × 100 (mm) | App |

- **Hệ quả:** Muốn demo với PLC Siemens thật thì cần bật Modbus server trên PLC hoặc đổi sang S7 (ADR mới).

## D-005 · PatchCore (anomalib) cho lỗi bề mặt, chạy bằng ONNX
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Lỗi bề mặt đa dạng, ít mẫu lỗi; nhà máy thường chỉ có nhiều mẫu tốt.
- **Phương án:** PatchCore (chỉ cần ảnh tốt) · YOLO/segmentation có nhãn lỗi · luật xử lý ảnh cổ điển.
- **Quyết định:** PatchCore qua anomalib, export ONNX, chạy trong C# bằng ONNX Runtime.
- **Hệ quả:** Phase 3 tùy chọn; nếu thiếu thời gian, hệ thống vẫn hoàn chỉnh với kiểm tra hình học.

## D-006 · Giữ Python 3.12, không nâng lên 3.14
- **Trạng thái:** Chấp nhận · **Ngày:** 2026-11-25
- **Bối cảnh:** Máy đang có Python 3.12.4. Câu hỏi: có nên nâng lên 3.14?
- **Phương án:** 3.12 (có sẵn) · 3.14 (mới nhất).
- **Quyết định:** Giữ 3.12, dùng venv riêng cho dự án. Dự án không cần tính năng nào của 3.14; thư viện ML (PyTorch, anomalib) thường hỗ trợ bản Python mới chậm hơn.
- **Hệ quả:** Xem lại khi dự án kết thúc hoặc khi một thư viện cần dùng bỏ hỗ trợ 3.12.
