# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 13.52 khớp có v > 0 mỗi người
- Tổng: v=2 335 | v=1 57 | v=0 101

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 1 | 6 | 3% |
| 1 | left_eye | 19 | 1 | 9 | 3% |
| 2 | right_eye | 22 | 0 | 7 | 0% |
| 3 | left_ear | 13 | 1 | 15 | 3% |
| 4 | right_ear | 17 | 4 | 8 | 14% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 22 | 2 | 5 | 7% |
| 8 | right_elbow | 26 | 1 | 2 | 3% |
| 9 | left_wrist | 21 | 2 | 6 | 7% |
| 10 | right_wrist | 20 | 6 | 3 | 21% |
| 11 | left_hip | 20 | 8 | 1 | 28% |
| 12 | right_hip | 22 | 6 | 1 | 21% |
| 13 | left_knee | 15 | 6 | 8 | 21% |
| 14 | right_knee | 16 | 4 | 9 | 14% |
| 15 | left_ankle | 14 | 5 | 10 | 17% |
| 16 | right_ankle | 11 | 7 | 11 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
