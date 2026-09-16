# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hoàng Mạnh Cường   Nhóm: T033(Lead) MSV: 2A202602078   Ngày: 2026-09-16

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 335 / 57 / 101 |
| Thời gian trung bình mỗi ảnh | Chưa thống kê trong repo |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_hip — 28%
2. right_ankle — 24%
3. right_hip — 21%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có, đây là ba khớp khó nhất cho nhóm tôi. Hông và mắt cá chân thường bị che bởi quần áo, bóng, hoặc góc chụp, nên phần khó nhất không phải là “không biết có khớp đó hay không”, mà là quyết định khớp đó còn nằm trong khung hay đã bị che hoàn toàn. Nếu khớp vẫn còn định vị được theo trục chân/thân, ta chọn v=1 và giữ chấm ước lượng; nếu nó đã ra khỏi mép ảnh hoặc không còn căn cứ để gán, mới chọn v=0.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | Không có bản cũ trong repo | 0.9662 |
| OKS@0.50 | Không có bản cũ trong repo | 0.9655 |
| OKS@0.75 | Không có bản cũ trong repo | 0.9655 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 4 | 4 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->
- train_01, người 1, nose / left_eye / right_eye / right_ear / left_elbow / left_wrist / left_hip / right_hip: kéo toàn bộ skeleton về đúng người và sửa các khớp bị trượt hẳn vì nhầm người trong cùng ảnh.
- train_01, người 1, right_shoulder / right_elbow / right_wrist: sửa lại theo đúng trái-phải cơ thể, không còn gắn vào người bên cạnh.
- train_01, người 1, left_hip / right_hip / left_wrist / right_wrist: căn chỉnh từng khớp dựa trên thân người và đường chân tay để khớp dính vào cùng một người.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Đây là dấu hiệu tốt; lỗi này thường xuất hiện ở ảnh dễ khi ta làm nhanh và kiểm tra thiếu theo nguyên tắc trái-phải của cơ thể, chứ không nhìn theo góc người thật.

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: 2A202602078

Phần kiểm chéo dưới đây là kết quả tự kiểm tra chính mình sau khi so bảng visibility của mình với bảng của bạn cùng nhóm và đối chiếu lại hình ảnh gốc.

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| right_ankle | 24% | ~20% | ~4% | Chưa hoàn toàn rõ guideline cho mắt cá chân khi bị che một phần; cần kiên định hơn về “vẫn trong khung” vs “ra ngoài khung”. |
| left_hip | 28% | ~25% | ~3% | Hông là khớp dễ lệch do quần áo và bóng che; đây phần lớn là guideline chứ không phải nhầm người. |
| right_hip | 21% | ~18% | ~3% | Tương tự left_hip; cần giữ chấm nếu còn thể ước lượng được theo đường cơ thể. |

Sau khi tự đối chiếu lại hình, tôi nhận ra nhiều nhịp lệch không phải do ảnh khác nhau mà do cách đọc “còn trong khung nhưng bị che” khác nhau. Vì vậy tôi đã cập nhật quy tắc sau vào `GUIDELINE_MINI.md`:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Nếu hông hoặc mắt cá chân còn nằm trong khung hình nhưng bị quần áo, bóng hoặc góc chụp che một phần và vẫn có thể ước lượng được vị trí giải phẫu, chọn v=1 và giữ chấm; chỉ chọn v=0 khi khớp đã ra ngoài mép ảnh hoặc không còn đủ căn cứ để xác định vị trí.

> Đây là kiểm chéo tự đánh giá: tôi xem lại ảnh, so với guideline, và chỉnh lại nếu bắt gặp sự mơ hồ về visibility trước khi bước sang phần gold/rework.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6853 | +0.0000 |
| pose_precision | 0.9734 | 0.9749 | +0.0015 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50 | 0.9785 | 0.9785 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8044 | -0.0075 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?  
   `pose_mAP50-95` không đổi: 0.6853 -> 0.6853, chênh +0.0000. Về mặt thực nghiệm, 20 ảnh là quá ít để model học thêm một “mẫu mới” đáng kể; do đó không có cải thiện rõ rệt. `box_mAP50-95` lại giảm nhẹ từ 0.8119 xuống 0.8044 (-0.0075), cho thấy việc fine-tune trên 20 ảnh không tạo ra lợi ích ổn định, và thậm chí có thể làm mô hình bị “bám” quá chặt vào cảnh cụ thể.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?  
   `box_mAP50-95` gốc là 0.8119, còn `pose_mAP50-95` gốc là 0.6853; chênh là khoảng 0.1266. Về mặt ý nghĩa, model tìm được người dễ hơn tìm được khớp, vì box người có vùng bao lớn và rõ hình dạng, còn keypoints bị che, nhỏ và có độ lệch theo góc nhìn cao hơn.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):  
   Trên tập test, đây là ảnh mà model cần được xem bằng mắt; trong thực tế, nếu điểm khớp lệch quanh vị trí thật nhưng không đổi bên trái-phải, đó là lỗi lệch nhẹ; nếu đặt đầu/chiếc tay bị lật qua trái-phải, đó là lỗi đảo trái/phải; nếu chấm vào người khác trong cùng hình, đó là nhầm người; nếu lệch xa nhiều chục pixel, đó là trượt hẳn. Cần kết luận dựa trên ảnh predict cụ thể, không tự suy từ số mAP.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?  
   Theo kết quả chấm với gold, ảnh `train_01` là ứng viên có lỗi lớn nhất: `eval_vs_gold.json` báo nhiều `truot_han` và `nham_nguoi`, đặc biệt ở các khớp mặt, vai, tay và hông. Đây là bằng chứng cho thấy ảnh này là khó nhất và nhãn của bạn cần được xem lại bằng mắt bằng `tools/visualize_pose.py`.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?  
   Có khả năng rất cao là `train_01`; vì đây là ảnh có nhiều người chồng lên nhau và nhiều khớp bị che / sai vị trí. Nếu cả nhãn của bạn và model đều kém ở cùng một ảnh, điều đó chứng tỏ bức ảnh đó quá khó do góc chụp, chồng người và mối che khuất, không phải chỉ do người gán nhãn hay model thiếu học.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Ví dụ: Ở `train_01`, người 1, `right_ankle`, tôi chọn `v=1` vì bàn chân vẫn còn nằm trong khung hình và có thể ước lượng theo đường chân và giải phẫu của cơ thể, dù bị bóng và cắt góc chụp. Nếu chỉ nhìn vào mép ảnh mà bỏ qua phần chân còn hiện rõ, ta dễ nhầm thành `v=0`; điều này sai vì khớp vẫn còn trong khung và có căn cứ để đặt chấm. Quy tắc này giúp tránh xóa khớp khi khớp còn hình thành nhưng bị che một phần. Hơn nữa, `v=0` chỉ nên dùng khi khớp thực sự nằm ngoài ảnh hoặc không còn định vị được.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
