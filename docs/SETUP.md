# Cài đặt

> File này trả lời: **cần gì và dựng thế nào** để chạy dự án từ máy trắng.

## Phần cứng
| Món | Ghi chú | Giá ước tính |
|---|---|---|
| Webcam 1080p (hoặc điện thoại + DroidCam) | Nên chỉnh được exposure/focus thủ công | có sẵn / ~300k |
| Giá đỡ camera nhìn thẳng xuống | Cố định chắc, không rung | ~100k |
| Bảng đèn LED A4 (bảng đồ chữ) | Đèn nền cho đo kích thước | ~150k |
| Đèn bàn LED | Đèn chiếu trực tiếp cho lỗi bề mặt (P3) | có sẵn |
| 20–30 vòng đệm M6/M8/M10 | Tự làm hỏng 5–10 cái | ~30k |
| Thước kẹp điện tử | Số đo chuẩn | ~150k |
| Bàn cờ hiệu chuẩn | In giấy, dán lên tấm phẳng cứng (mica/gỗ) | ~10k |

> TODO: thêm ảnh setup thật vào `docs/img/setup.jpg` và ghi khoảng cách camera–vật sau P0-02.

## Phần mềm
| Thành phần | Phiên bản | Kiểm tra |
|---|---|---|
| Python | 3.12 ([D-006](DECISIONS.md)) | `python --version` |
| .NET SDK | 10.x | `dotnet --list-sdks` |
| Git | bất kỳ | `git --version` |

## Môi trường Python (P0-03)
```powershell
cd python
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -c "import cv2, numpy, pytest; print(cv2.__version__)"
```

## Ứng dụng C#
> TODO: thêm lệnh build/run khi tạo project (P2-01).
