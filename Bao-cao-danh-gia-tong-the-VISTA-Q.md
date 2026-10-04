# Báo cáo đánh giá tổng thể khóa luận VISTA Q

Hệ thống hỗ trợ quyết định rủi ro cổ phiếu Việt Nam kết hợp học máy có giải thích, khoảng dự báo và phân bổ danh mục định lượng

Bản trình giảng viên hướng dẫn • Tổng hợp ngày 04 tháng 10 năm 2026 • Dữ liệu đến 21 tháng 09 năm 2026

## Kết luận đánh giá

Khóa luận đã có một chuỗi nghiên cứu và phần mềm hoạt động từ dữ liệu, dự báo biến động, hiệu chỉnh độ bất định, xếp ưu tiên review đến mô phỏng phân bổ danh mục và DSS có lưu quyết định của analyst. Giá trị rõ nhất hiện tại nằm ở khả năng hỗ trợ ưu tiên xem xét rủi ro và kiểm tra toàn vẹn quy trình. Chưa đủ bằng chứng để kết luận mô hình thống kê vượt trội mọi đối chứng, giảm tải an toàn hoặc tạo lợi nhuận giao dịch thực tế.

| Kết quả nổi bật | Bằng chứng hiện tại |
| --- | --- |
| Dự báo và đánh giá | 15 mô hình hoặc chính sách; hai horizon 5 và 20 phiên; 79.488 dòng forecast; annual universe 30 mã |
| Khoảng dự báo | CQR pooled coverage 89,73–91,12% trên bốn mẫu; còn undercoverage theo regime |
| Hỗ trợ review | Top-5 đạt precision lift 2,75–3,13 lần; capture toàn event có nhãn 45,22–51,41% |
| Kiểm soát rủi ro danh mục | S3 giảm breach so với S2 sau Holm; chưa tách được lợi ích riêng của uncertainty khỏi giảm exposure |
| DSS hiện có | Queue, chi tiết mã, gamma SHAP, review và override có lịch sử, dashboard danh mục và export |

## Phạm vi của bản báo cáo

Số liệu chính dùng V92 đã audit ở Phase 1.3.1, backtest Phase 1.4.1 đã xác nhận trên Colab và phần mềm Phase 1.5. Báo cáo này không refit mô hình, chọn lại tham số hoặc thay đổi kết quả. Các lần thử V2–V91 được xem là lịch sử phát triển, không ghép vào mẫu hiện tại.

Đề nghị trao đổi với Thầy về trọng tâm MIS, tiêu chí đánh giá người dùng và kế hoạch xác nhận tương lai. Đây là báo cáo tiến độ có kết quả, chưa phải luận văn đã hoàn thành nghiên cứu người dùng hoặc triển khai giao dịch.

# 1 Mục tiêu và câu hỏi nghiên cứu

Bài toán quản trị là phân bổ năng lực review hữu hạn cho các mã có khả năng biến động cao, đồng thời nhận diện trường hợp thiếu dữ liệu hoặc dự báo. Người dùng mục tiêu là analyst hoặc nhóm quản trị rủi ro; đầu ra là thứ tự xem xét, bằng chứng giải thích và lịch sử quyết định.

| Câu hỏi | Kết quả đã có | Phần còn cần chứng minh |
| --- | --- | --- |
| RQ1 Dự báo | So sánh 15 mô hình bằng QLIKE, MAE, RMSE, CC sensitivity và HAC/Holm | Lợi thế kiến trúc ML dưới cùng điều kiện huấn luyện và mẫu tương lai |
| RQ2 Độ bất định | Rolling, asymmetric và CQR; kiểm tra coverage, width và interval score | Coverage theo regime và ticker; robustness ngoài mẫu đã xem |
| RQ3 Quyết định danh mục | S0–S4, exposure matched và blind control; phí và block sensitivity | Thông tin uncertainty có giá trị thêm ngoài giảm exposure hay không |
| RQ4 Giá trị MIS | Top-K và availability audit; DSS có review và override | Chất lượng, thời gian và hành vi quyết định của người dùng thực tế |

## Vai trò của thành phần quant

Quant được đặt vào tầng quyết định và quản trị rủi ro: dự báo volatility, covariance, phân bổ và theo dõi vượt target. Nghiên cứu hiện tại không xác lập chiến lược dự báo chiều giá hoặc alpha. Cách định vị này giữ được bản sắc MIS vì mô hình được chuyển thành quy trình review và quyết định có thể kiểm tra.

## Chuỗi nghiên cứu đã triển khai

Dữ liệu và audit → đặc trưng và nhãn tương lai → walk-forward forecast → delayed calibration → queue và giải thích → mô phỏng portfolio → analyst review → audit và export. Quyết định sau khi đóng cửa được tách khỏi thông tin của phiên thực thi tiếp theo.

Đóng góp nên được mô tả là tích hợp phương pháp, audit toàn request grid và thiết kế DSS trong phạm vi dự án. Chưa có tổng quan hệ thống đủ để tuyên bố một thuật toán mới có tính mới toàn cầu.

# 2 Dữ liệu và chất lượng nguồn

Nguồn đầu vào gồm mười snapshot CafeF: OHLCV của HSX, HNX, UPCOM; Index có VN-Index; sáu bảng CC và NN. Các bảng CC liên quan lệnh cung cầu; trường high/low của NN chỉ giữ theo ý nghĩa khối lượng khớp lệnh/thỏa thuận đã đối chiếu, không tự diễn giải thành giao dịch nhà đầu tư nước ngoài.

| Hạng mục | Kết quả audit |
| --- | --- |
| Dòng giá đầu vào / sau xử lý | 3.337.754 / 3.322.900 |
| Mã trong toàn snapshot / union nghiên cứu | 3.910 / 60 |
| Sửa nhỏ bất đẳng thức OHLC | 911.081 dòng |
| Magnitude sửa p50 / p95 / max | 0,0008 / 0,0039 / 0,02 theo đơn vị giá CSV |
| Quarantine trong union nghiên cứu | 463 dòng: 448 OHLC không hợp lệ, 10 gap lớn, 5 lệch lịch |
| Overnight gap trên 20% | 10 trường hợp cần xác minh nguyên nhân |
| Bảng cung cầu trong union | 179.206 dòng; 60 mã |

## Những điểm đã cải thiện

Lịch thị trường được đối chiếu giữa chỉ số và cổ phiếu; một số giá VN-Index thiếu được vá bằng nguồn có dẫn chứng. Phiên thiếu giữ NaN thay vì nén thời gian; feature cung cầu thiếu không bị biến thành thị trường cân bằng. Audit tách repair nhỏ, quarantine và trạng thái lịch sử chưa đủ.

## Giới hạn còn ảnh hưởng kết luận

Chưa có OHLC raw, adjusted và total-return tách biệt với provenance độc lập; lịch chính thức của sở giao dịch chưa được xác minh đầy đủ bên ngoài. Cùng một series được dùng ở các vai trò giá không chứng minh adjustment đúng. PIT được xác nhận cho quy tắc universe và cutoff trên snapshot, chưa cho một cơ sở dữ liệu giá có phiên bản lịch sử hoàn chỉnh.

3,32 triệu dòng là quy mô dữ liệu nguồn sau xử lý, không phải số quan sát hiệu dụng của các kiểm định. Snapshot ngày 21/09/2026 đã cũ 13 ngày tại ngày báo cáo; không phải feed trực tiếp.

# 3 Universe và mẫu đánh giá

Mỗi năm chọn 30 mã bằng thanh khoản lịch sử tại cutoff trước năm hoạt động, dùng 252 phiên nhìn lại và tối thiểu 180 quan sát giá/khối lượng dương. Không lọc theo việc mã còn tồn tại ở cuối test. Union 2021–2026 gồm 60 mã.

| Năm | Requests mỗi h | Có forecast | Coverage | Thiếu origin | Feature chưa đủ |
| --- | --- | --- | --- | --- | --- |
| 2021 | 7.500 | 6.923 | 92,31% | 5 | 572 |
| 2022 | 7.470 | 6.730 | 90,09% | 189 | 551 |
| 2023 | 7.470 | 7.042 | 94,27% | 17 | 411 |
| 2024 | 7.500 | 7.316 | 97,55% | 14 | 170 |
| 2025 | 7.440 | 6.483 | 87,14% | 12 | 945 |
| 2026 | 5.250 | 5.250 | 100,00% | 0 | 0 |

Toàn lịch có 85.260 request–horizon nhưng chỉ 79.488 forecast, tức 93,23%. Có 5.772 request không có point forecast; 4.929 trong số đó vẫn có outcome đã biết, trong đó 1.620 vượt ngưỡng. Các horizon và origin chồng lấn nên đây không phải 1.620 sự kiện thị trường độc lập.

| Giai đoạn | h | Forecast | Có nhãn để chấm | Ngày được chấm |
| --- | --- | --- | --- | --- |
| 2023–2025 | 5 | 20.841 | 20.759 | 747 |
| 2023–2025 | 20 | 20.841 | 20.504 | 747 |
| 2026 đến 21 tháng 9 | 5 | 5.250 | 5.100 | 170 |
| 2026 đến 21 tháng 9 | 20 | 5.250 | 4.650 | 155 |

Hai giai đoạn báo cáo chính là 2023–2025 và phần sau theo thời gian năm 2026. Researcher đã xem dữ liệu và kết quả qua nhiều vòng; năm 2026 không được gọi là prospective holdout chưa từng xem. Outcome chưa mature không được gán event=False.

# 4 Phương pháp hiện tại

## Nhãn và đặc trưng

Nhãn chính là Yang–Zhang tính trực tiếp trên block tương lai h phiên, kết hợp overnight variance, open-to-close variance và Rogers–Satchell. Không lấy trung bình các trailing YZ 20 phiên rồi gọi là realized target tương lai. Mean daily variance được đổi sang annualized volatility với 252 phiên/năm. Close-to-close được giữ để kiểm tra sensitivity.

Đặc trưng gồm lịch sử volatility, range, downside risk, gap, thanh khoản, cung cầu và market factor lấy từ VN-Index. Regime Low/Normal/High dùng các ngưỡng một phần ba và hai phần ba của market volatility học trên 2014–2020; percentile 60 phiên là feature riêng. Cờ trần/sàn dùng band và giá close proxy, chưa là giá tham chiếu thực thi được xác minh.

## Forecast và refit

Nhóm đối chứng gồm historical, EWMA, log-HAR và ridge-HAR; GARCH, GJR-GARCH, EGARCH có fallback; LightGBM legacy, full, gamma, gamma có identity, gamma recent; equal, adaptive và convex stacked ensemble. Historical refit mỗi 60 phiên, fold tách khi đổi năm. Phiên snapshot cuối có fit mới tại cutoff phiên trước; điều này chưa xác nhận một policy retrain hàng ngày.

Train/validation được purge theo ngày nhãn mature; validation gần nhất 60 phiên dùng early stopping. Các bản gamma không identity và recent giúp khảo sát phụ thuộc mã và lịch sử gần. Stacking dùng geometric pool trên log variance với trọng số convex, cập nhật theo nhãn đã mature; các lựa chọn lịch sử có audit.

## Conformal và suy luận thống kê

Rolling log-residual, asymmetric và CQR chỉ dùng score đã có feedback; giữ 126 origin dates gần nhất, tối thiểu 100 scores và 30 dates. Nominal coverage 90%. Đây là time-series calibration thực nghiệm với dữ liệu phụ thuộc, không là bảo đảm IID. HAC trên loss trung bình theo ngày và block bootstrap giữ cấu trúc thời gian; Holm hiệu chỉnh family đã định trước.

V92 hiện tại không dùng scaled exponential intervals làm primary và không gọi rolling CQR là τ-separated ACI. Các kết quả sensitivity cũ là lịch sử phát triển, không phải thử nghiệm đã chạy lại trên V92.

# 5 Kết quả dự báo điểm và kiểm định

| Giai đoạn | h | Stack | LGB legacy | HAR legacy | GARCH policy |
| --- | --- | --- | --- | --- | --- |
| 2023–2025 | 5 | 0,2698 | 0,2816 | 0,3049 | 0,3893 |
| 2023–2025 | 20 | 0,2430 | 0,2528 | 0,2357 | 0,3502 |
| 2026 đến 21 tháng 9 | 5 | 0,2100 | 0,2327 | 0,2739 | 0,2412 |
| 2026 đến 21 tháng 9 | 20 | 0,1490 | 0,1643 | 0,1834 | 0,1839 |

QLIKE thấp hơn tốt hơn. GARCH là policy có EWMA fallback. Bảng này dùng mean loss theo stock-day; HAC bên dưới dùng loss trung bình theo date, nên mean difference không nhất thiết bằng phép trừ các mean pooled.

| Giai đoạn | h | Stack so với LGB | So với HAR | So với GARCH |
| --- | --- | --- | --- | --- |
| 2023–2025 | 5 | 0,4788 | 0,0005 | 0,0002 |
| 2023–2025 | 20 | 0,4788 | 0,7653 | 0,2030 |
| 2026 đến 21 tháng 9 | 5 | 0,4659 | 0,0781 | 0,0010 |
| 2026 đến 21 tháng 9 | 20 | 0,5954 | 0,4659 | 0,0079 |

Stack có QLIKE point estimate thấp hơn legacy LightGBM trong cả bốn mẫu, nhưng không có p Holm dưới 0,05 khi so với LightGBM. Stack tốt hơn HAR ở development h=5 và tốt hơn GARCH policy ở development h=5, 2026 h=5/20 theo family sáu kiểm định mỗi period. Không suy ra dominance với tất cả 15 mô hình.

Mô hình có QLIKE pooled thấp nhất thay đổi theo mẫu: equal ensemble ở development h=5; ridge-HAR ở development h=20; gamma_id ở 2026 h=5; adaptive ensemble ở 2026 h=20. Stacking không đứng đầu ở bất kỳ ô nào. MAE volatility của stack cao hơn legacy LGB trong cả bốn ô.

Kết luận phù hợp là các mô hình cải thiện những metric hoặc mẫu cụ thể, chưa có một cấu hình thắng toàn diện. Không chọn lại primary theo kết quả test trong bản tổng hợp này.

# 6 Bảng đầy đủ dự báo điểm horizon 5 phiên

| Mô hình | QLIKE 2023–25 | MAE 2023–25 | QLIKE 2026 | MAE 2026 |
| --- | --- | --- | --- | --- |
| egarch | 0,4056 | 0,1142 | 0,2359 | 0,1027 |
| ensemble_adaptive | 0,2606 | 0,0895 | 0,2037 | 0,0908 |
| ensemble_equal | 0,2590 | 0,0887 | 0,2038 | 0,0899 |
| ensemble_stack | 0,2698 | 0,0914 | 0,2100 | 0,0942 |
| ewma94 | 0,4355 | 0,0998 | 0,2747 | 0,1002 |
| gamma | 0,2695 | 0,0941 | 0,1996 | 0,0965 |
| gamma_id | 0,2604 | 0,0935 | 0,1992 | 0,0947 |
| gamma_recent | 0,2727 | 0,0964 | 0,2048 | 0,0970 |
| garch | 0,3893 | 0,1092 | 0,2412 | 0,1015 |
| gjr_garch | 0,3962 | 0,1118 | 0,2353 | 0,1009 |
| har_ridge | 0,2770 | 0,1013 | 0,2330 | 0,1030 |
| har_v8 | 0,3049 | 0,0910 | 0,2739 | 0,0932 |
| historical | 0,3600 | 0,1016 | 0,3047 | 0,1113 |
| lgb_full | 0,2865 | 0,0851 | 0,2262 | 0,0874 |
| lgb_v8 | 0,2816 | 0,0840 | 0,2327 | 0,0879 |

MAE là annualized decimal volatility, không là return. Trang HTML có RMSE, CC-target QLIKE và mọi regime. Giá trị tối thiểu theo test chỉ để mô tả, không tự động là mô hình triển khai.

GARCH fallback ở development khoảng 9,7–9,8%, tăng lên 38,2–39,0% ở năm 2026. Nguyên nhân năm 2026 là thiếu đủ 250 returns liên tiếp ở 13 mã; không được quy toàn bộ cho lỗi hội tụ. Vì vậy kết quả GARCH family phải được gắn nhãn forecast policy có EWMA fallback.

Các booster mới và ridge-HAR có thể được dùng nhiều nhãn mature hơn legacy comparator. Chênh lệch không chỉ phản ánh kiến trúc; nghiên cứu xác nhận tiếp cần đồng nhất training sample, cadence, horizon và availability. HAR fallback thật bằng 0 ở audit nguồn; 55 trường hợp clipping không đồng nghĩa fallback.

# 6 Bảng đầy đủ dự báo điểm horizon 20 phiên

| Mô hình | QLIKE 2023–25 | MAE 2023–25 | QLIKE 2026 | MAE 2026 |
| --- | --- | --- | --- | --- |
| egarch | 0,3718 | 0,1220 | 0,1852 | 0,0928 |
| ensemble_adaptive | 0,2303 | 0,0829 | 0,1459 | 0,0810 |
| ensemble_equal | 0,2264 | 0,0814 | 0,1461 | 0,0804 |
| ensemble_stack | 0,2430 | 0,0842 | 0,1490 | 0,0837 |
| ewma94 | 0,4782 | 0,0989 | 0,2483 | 0,0914 |
| gamma | 0,2391 | 0,0870 | 0,1519 | 0,0864 |
| gamma_id | 0,2449 | 0,0878 | 0,1528 | 0,0850 |
| gamma_recent | 0,2559 | 0,0891 | 0,1573 | 0,0866 |
| garch | 0,3502 | 0,1116 | 0,1839 | 0,0895 |
| gjr_garch | 0,3610 | 0,1158 | 0,1818 | 0,0897 |
| har_ridge | 0,2241 | 0,0903 | 0,1601 | 0,0914 |
| har_v8 | 0,2357 | 0,0826 | 0,1834 | 0,0852 |
| historical | 0,3641 | 0,0999 | 0,2273 | 0,0955 |
| lgb_full | 0,2617 | 0,0802 | 0,1656 | 0,0800 |
| lgb_v8 | 0,2528 | 0,0809 | 0,1643 | 0,0763 |

MAE là annualized decimal volatility, không là return. Trang HTML có RMSE, CC-target QLIKE và mọi regime. Giá trị tối thiểu theo test chỉ để mô tả, không tự động là mô hình triển khai.

GARCH fallback ở development khoảng 9,7–9,8%, tăng lên 38,2–39,0% ở năm 2026. Nguyên nhân năm 2026 là thiếu đủ 250 returns liên tiếp ở 13 mã; không được quy toàn bộ cho lỗi hội tụ. Vì vậy kết quả GARCH family phải được gắn nhãn forecast policy có EWMA fallback.

Các booster mới và ridge-HAR có thể được dùng nhiều nhãn mature hơn legacy comparator. Chênh lệch không chỉ phản ánh kiến trúc; nghiên cứu xác nhận tiếp cần đồng nhất training sample, cadence, horizon và availability. HAR fallback thật bằng 0 ở audit nguồn; 55 trường hợp clipping không đồng nghĩa fallback.

# 7 Khoảng dự báo và độ bất định

| Giai đoạn | h | Method | Coverage | Width | Interval score |
| --- | --- | --- | --- | --- | --- |
| 2023–2025 | 5 | rolling | 89,80% | 0,4079 | 0,5684 |
| 2023–2025 | 5 | cqr | 90,14% | 0,3670 | 0,5375 |
| 2023–2025 | 20 | rolling | 89,18% | 0,3813 | 0,5584 |
| 2023–2025 | 20 | cqr | 89,73% | 0,3618 | 0,5449 |
| 2026 đến 21 tháng 9 | 5 | rolling | 89,57% | 0,4031 | 0,5190 |
| 2026 đến 21 tháng 9 | 5 | cqr | 91,12% | 0,3936 | 0,5042 |
| 2026 đến 21 tháng 9 | 20 | rolling | 88,90% | 0,3470 | 0,4035 |
| 2026 đến 21 tháng 9 | 20 | cqr | 90,58% | 0,3703 | 0,4325 |

CQR pooled coverage gần nominal 90%. Trong 2023–2025, CQR có width và interval score thấp hơn rolling ở cả hai horizon; paired exploratory score CI loại 0. Kết quả này hỗ trợ CQR như một challenger có ích, chưa là xác nhận sau hiệu chỉnh toàn bộ so sánh.

Ở 2026 h=20, CQR nâng coverage từ 88,90% lên 90,58% nhưng width tăng và interval score xấu hơn, từ 0,4035 lên 0,4325. Paired unadjusted score CI khoảng [0,000387; 0,056996] cho thấy trade-off trong mẫu hồi cứu này.

## Cách đọc interval score

Score gồm độ rộng và phạt khi outcome ra ngoài hai phía. Width hoặc score thấp không có ích nếu coverage giảm quá nhiều. Đặc biệt phải xem upper miss vì đánh giá thấp volatility ảnh hưởng quản trị rủi ro. Trang HTML giữ đầy đủ lower/upper penalties để đối chiếu.

Width và score có đơn vị annualized decimal volatility. Coverage marginal stock-level không tạo bảo đảm riêng cho từng mã, từng regime hoặc rủi ro danh mục; CQR interval không phải VaR hoặc ES.

# 8 Kết quả theo regime và xác suất rủi ro

| Mẫu Low h20 | Method | Coverage | Stock-days | Dates | CI có đủ support |
| --- | --- | --- | --- | --- | --- |
| 2023–2025 | rolling | 86,46% | 7.905 | 280 | Có nhưng exploratory |
| 2023–2025 | asymmetric | 85,12% | 7.905 | 280 | Có nhưng exploratory |
| 2023–2025 | cqr | 87,88% | 7.905 | 280 | Có nhưng exploratory |
| 2026 đến 21 tháng 9 | rolling | 75,93% | 270 | 9 | Không |
| 2026 đến 21 tháng 9 | asymmetric | 71,48% | 270 | 9 | Không |
| 2026 đến 21 tháng 9 | cqr | 77,41% | 270 | 9 | Không |

Low regime h=20 vẫn có undercoverage. CQR development đạt 87,88%; CI paired coverage improvement so với rolling còn chứa 0. Năm 2026 chỉ có chín ngày Low để chấm h=20; 270 stock-days không tạo 270 thời điểm độc lập. Không tối ưu thêm theo nhóm nhỏ này để ép coverage 90%.

| Giai đoạn | h | Brier raw | Prevalence | Brier reference |
| --- | --- | --- | --- | --- |
| 2023–2025 | 5 | 0,0750 | 10,95% | 0,0975 |
| 2023–2025 | 20 | 0,0879 | 12,74% | 0,1112 |
| 2026 đến 21 tháng 9 | 5 | 0,1073 | 16,49% | 0,1377 |
| 2026 đến 21 tháng 9 | 20 | 0,1104 | 17,53% | 0,1445 |

Classifier tạo xác suất finite trong [0,1] trên toàn bộ forecasts. Brier thấp hơn prevalence reference ở các mẫu chỉ là đánh giá scoring trên cùng dữ liệu; xác suất vẫn raw và chưa probability-calibrated. Reference dùng prevalence của mẫu đánh giá là mô tả, không là một predictor triển khai đã biết trước.

Reliability theo mười bins và mọi interval/point metric theo Low, Normal, High được giữ trong trang HTML và CSV kèm báo cáo. Probability unavailable ở request không có forecast được giữ riêng, không thay bằng 0.

# 9 Giá trị hỗ trợ quyết định và missing forecast

| Giai đoạn | h | Precision K5 | Lift | Recall có score | Capture toàn event |
| --- | --- | --- | --- | --- | --- |
| 2023–2025 | 5 | 32,94% | 3,01× | 53,61% | 45,22% |
| 2023–2025 | 20 | 39,88% | 3,13× | 54,86% | 47,00% |
| 2026 đến 21 tháng 9 | 5 | 45,29% | 2,75× | 45,78% | 45,78% |
| 2026 đến 21 tháng 9 | 20 | 54,06% | 3,08× | 51,41% | 51,41% |

Queue primary là 50/50 point volatility và CQR upper, chia risk threshold. K=5 giới hạn số mã được ưu tiên trong một ngày/horizon. Ranking thực hiện trước khi loại outcome chưa biết. Precision và lift hỗ trợ giá trị tập trung review; đây không phải tỷ lệ giao dịch có lời.

| h | Event có forecast | Tỷ lệ | Event thiếu forecast | Tỷ lệ |
| --- | --- | --- | --- | --- |
| 5 | 2.274 / 20.759 | 10,95% | 422 / 1.459 | 28,92% |
| 20 | 2.612 / 20.504 | 12,74% | 437 / 1.336 | 32,71% |

Bảng missing forecast chỉ thuộc 2023–2025 và outcomes có nhãn. Sự khác biệt mô tả có thể liên quan thành phần mã và thời gian; chưa là bằng chứng nhân quả.

Chỉ báo cáo recall trên mã có forecast sẽ bỏ qua một nhóm rủi ro. Khi tính tất cả event đã biết, capture development h=5 giảm từ 53,61% xuống 45,22%; h=20 từ 54,86% xuống 47,00%. Analyst vẫn cần luồng review cho request abstain và các mã ngoài Top-5.

Safe skipping bị tắt; mọi request cần review. Workload Reduction Index bằng 0 theo policy hiện tại, không có claim giảm tải an toàn. Costs FN5/FP1 và FN10/FP1 là kịch bản giả định có điều kiện, chưa bao gồm chi phí thực tế của toàn luồng review.

# 10 Thiết kế thử nghiệm danh mục định lượng

| Policy | Quy tắc chính |
| --- | --- |
| S0 | Equal weight; scale bằng historical covariance |
| S1 | Inverse historical volatility |
| S2 | Inverse point-forecast volatility |
| S3 | Inverse CQR upper volatility |
| S4 | S3 với target nhân 0,65 ở High và 0,85 ở Normal |
| EM_S2 và EM_S3 | Đối chứng cùng target gross để tách composition và exposure |
| BLIND_S2 | S2 với haircut học từ allocations 2022, không dùng outcomes test |

Tất cả dùng annual universe, lịch và readiness chung. Target volatility 15%/năm, max weight 10%, tiền mặt tối thiểu 2%, rebalance mỗi năm phiên. Correlation lookback 252 phiên, tối thiểu 120 complete returns và shrinkage 0,5. Ready sleeve được scale theo tỷ lệ mã sẵn sàng trong annual universe.

Khi thiếu dự báo, policy bảo vệ vị thế đang có, hủy lệnh chờ và không sinh lệnh mới; trạng thái cần manual review được giữ. Giá stale không bị biến thành observed return đã xác minh. Actual gross có thể khác target gross do fills, chi phí hoặc protected holdings.

| Quy mô thử nghiệm | Giá trị |
| --- | --- |
| Cấu hình | 64 = 8 policies × 2 horizons × 4 mức phí |
| Phí sensitivity | 0, 10, 25 và 50 bps trên turnover |
| Lịch / risk windows | 922 phiên / 705 observed windows mỗi cấu hình |
| Giao dịch / position rows | 351.499 / 1.766.080 |
| Portfolio risk window | 20 phiên cho cả forecast h=5 và h=20 |

Mô phỏng dùng fractional economic units và CafeF price proxy. Các metric lợi nhuận, CAGR và Sharpe là proxy descriptors; chưa xác nhận board lots, settlement, participation, corporate actions hoặc fill tại trần/sàn.

# 11 Kết quả danh mục mô tả

| h | Policy | Mean gross | MAE điểm % | Breach | Max DD proxy |
| --- | --- | --- | --- | --- | --- |
| 5 | S0 | 81,32% | 5,856 | 62,41% | -30,30% |
| 5 | S1 | 86,76% | 5,786 | 58,44% | -27,04% |
| 5 | S2 | 86,83% | 5,496 | 63,12% | -26,50% |
| 5 | S3 | 57,96% | 4,299 | 23,97% | -19,86% |
| 5 | S4 | 48,43% | 5,640 | 5,67% | -19,68% |
| 5 | EM_S2 | 57,97% | 4,334 | 23,55% | -19,73% |
| 5 | EM_S3 | 57,96% | 4,299 | 23,97% | -19,86% |
| 5 | BLIND_S2 | 55,66% | 4,534 | 22,98% | -17,50% |
| 20 | S0 | 80,68% | 6,147 | 58,87% | -29,74% |
| 20 | S1 | 86,07% | 6,019 | 55,74% | -26,92% |
| 20 | S2 | 85,67% | 5,411 | 61,56% | -25,87% |
| 20 | S3 | 56,66% | 4,608 | 17,87% | -19,20% |
| 20 | S4 | 47,25% | 5,866 | 7,80% | -18,90% |
| 20 | EM_S2 | 56,71% | 4,650 | 17,16% | -19,29% |
| 20 | EM_S3 | 56,66% | 4,608 | 17,87% | -19,20% |
| 20 | BLIND_S2 | 51,76% | 4,918 | 18,01% | -16,12% |

Phí 25 bps; pooled 2023–21/09/2026. Risk MAE và breach dùng 705 observed windows; mean gross tính trên 922 phiên. Max DD là maximum drawdown NAV proxy, không phải P&L thực thi.

S3 có gross thấp hơn S2 khoảng 29 điểm phần trăm và breach thấp hơn. EM_S2 có breach gần S3, cho thấy việc hạ exposure giải thích phần đáng kể khác biệt. S4 giảm exposure mạnh hơn theo regime; breach thấp không tự đồng nghĩa bám target hoặc hiệu quả kinh tế tốt hơn.

Bảng pooled dùng để mô tả. Kết luận thống kê dùng paired endpoints và giai đoạn tách biệt ở trang tiếp theo. Website có toàn bộ 64 cấu hình, các curve NAV và sensitivity phí.

# 12 Kiểm định danh mục sau Holm

| Giai đoạn | h | A so với B | Breach A | Breach B | Chênh điểm % | p Holm |
| --- | --- | --- | --- | --- | --- | --- |
| 2023–2025 | 5 | S3 / S2 | 21,89% | 60,38% | -38,49 | 0,0240 |
| 2023–2025 | 5 | EM_S3 / EM_S2 | 21,89% | 21,51% | 0,38 | 1,0000 |
| 2023–2025 | 5 | S3 / BLIND_S2 | 21,89% | 22,08% | -0,19 | 1,0000 |
| 2023–2025 | 20 | S3 / S2 | 14,34% | 58,49% | -44,15 | 0,0240 |
| 2023–2025 | 20 | EM_S3 / EM_S2 | 14,34% | 13,58% | 0,75 | 1,0000 |
| 2023–2025 | 20 | S3 / BLIND_S2 | 14,34% | 16,79% | -2,45 | 1,0000 |
| 2026 đến 21 tháng 9 | 5 | S3 / S2 | 30,29% | 71,43% | -41,14 | 0,0240 |
| 2026 đến 21 tháng 9 | 5 | EM_S3 / EM_S2 | 30,29% | 29,71% | 0,57 | 1,0000 |
| 2026 đến 21 tháng 9 | 5 | S3 / BLIND_S2 | 30,29% | 25,71% | 4,57 | 1,0000 |
| 2026 đến 21 tháng 9 | 20 | S3 / S2 | 28,57% | 70,86% | -42,29 | 0,0240 |
| 2026 đến 21 tháng 9 | 20 | EM_S3 / EM_S2 | 28,57% | 28,00% | 0,57 | 1,0000 |
| 2026 đến 21 tháng 9 | 20 | S3 / BLIND_S2 | 28,57% | 21,71% | 6,86 | 1,0000 |

Primary block 20; phí 25 bps; negative A−B tốt hơn. Paired dates là 530 trong 2023–2025 và 175 trong 2026. Mỗi period/block dùng Holm family 48 tests: 2 horizons × 4 fees × 3 pairs × 2 endpoints.

S3 giảm breach so với S2 sau Holm ở hai horizons, mọi mức phí và hai periods; kết quả giữ với block sensitivity 60. Tuy nhiên chưa có MAE dominance sau Holm, chưa có endpoint xác nhận EM_S3 tốt hơn EM_S2 hoặc S3 tốt hơn BLIND_S2. Thiếu ý nghĩa thống kê không chứng minh tương đương.

Kết luận hợp lệ: trong mô phỏng hồi cứu, S3 dùng upper bound giảm tỷ lệ vượt target volatility so với S2 point forecast, cùng với exposure thấp hơn. Các đối chứng chưa xác lập giá trị thông tin riêng của uncertainty sau multiple testing.

CI xuất trong bảng nguồn là unadjusted 95%; không dùng CI này để thay quyết định Holm. Các mức phí và block size phụ thuộc nhau, không là những lần xác nhận độc lập.

# 13 XAI và DSS đã triển khai

## Giải thích có đúng phạm vi

Local gamma SHAP đã lưu ở cấp date, ticker và horizon cho 79.488 forecasts; tái dựng log variance với sai số tối đa khoảng 7,11 × 10⁻¹⁵. Các driver thường gồm rv_w, gap_var_20 và market_vol_20. Đây là tính nhất quán của explanation với gamma component, chưa giải thích toàn ensemble, quyết định allocation hoặc quan hệ nhân quả.

## Chức năng hiện có của Phase 1.5

| Tầng | Chức năng đã triển khai |
| --- | --- |
| Xem xét rủi ro | Overview; queue theo h; tìm mã; readiness, missing data và snapshot age |
| Chi tiết mã | Point forecast, CQR, threshold, raw probability, cutoff và 56 local SHAP contributions |
| Quyết định analyst | Review status, ghi chú, actor name và đổi ưu tiên hiển thị kèm lý do |
| Lưu và phối hợp | SQLite persistent; revision kiểm soát conflict; idempotent save; lịch sử append-only |
| Bằng chứng nghiên cứu | 64 NAV curves và 192 inference rows; filters theo period, h, fee và endpoint |
| Audit và export | Run ID, as-of, payload hash; review history JSON/CSV; kiểm tra hash chain cục bộ |

Human override được tách khỏi thứ hạng gốc của mô hình; mỗi review giữ nguồn forecast để truy vết. Hash chain là kiểm tra toàn vẹn cục bộ, chưa ký số hoặc chống chỉnh sửa bởi quản trị viên. Actor name chưa là danh tính xác thực. Thời gian xem chi tiết ghi phía client chưa là thời gian toàn bộ nhiệm vụ.

Phần mềm chạy local loopback; chưa có phân quyền team, xác thực hoặc kiểm thử tải production. Không có order endpoint. Trang HTML đi kèm báo cáo là viewer offline để trình bày kết quả, không thay thế backend review của Phase 1.5.

Nghiên cứu người dùng chưa thực hiện. Chưa có bằng chứng rằng DSS làm analyst nhanh hơn, quyết định tốt hơn hoặc giảm workload trong thực tế.

# 14 Mức độ kiểm chứng và giới hạn bằng chứng

| Hạng mục | Bằng chứng hiện có |
| --- | --- |
| Phase 1.3.1 | 92 tests trên Colab; 45 kiểm tra độc lập; 109 payload files đối chiếu hash; giữ nguyên forecasts gốc |
| Phase 1.4.1 | 120 tests trên Colab; 94 kiểm tra accounting/timing/DSS; 8 kiểm tra bootstrap bao phủ 192 rows; 25 kiểm tra bổ sung |
| Vá full exit | 1.408 filled exits hết đúng số units; không còn tiny stale dust; giữ 2.752 stale position rows thật |
| Tái lập đa môi trường | Windows/Colab cùng masks và quyết định inference; max relative NAV error 2,727 × 10⁻¹⁵ |
| Phase 1.5 | 18 Python tests; JS syntax và browser flow, persistence, conflict, responsive đã kiểm tra |

Các nhóm kiểm tra trên có mục đích khác nhau và có thể chồng lấn; không cộng thành một số tests độc lập để khuếch đại bằng chứng. Code pass, hash khớp và accounting nhất quán xác nhận kỹ thuật, không tự xác nhận prediction utility hoặc lợi ích kinh tế.

## Những kết luận chưa được phép nâng cấp

- Stacking thống kê vượt tất cả benchmark hoặc là model tối ưu.

- CQR bảo đảm coverage 90% cho từng regime, ticker hoặc portfolio.

- Mã ngoài Top-5 hay request thiếu forecast có thể bỏ qua an toàn.

- S3 tạo alpha, lợi nhuận thực thi hoặc chứng minh đóng góp riêng của uncertainty.

- SHAP giải thích ensemble hoặc chứng minh tác động nhân quả.

- PIT prices, official calendar, corporate actions và model binaries đã được kiểm chứng đầy đủ.

Operational readiness, safe skip và order routing đều giữ false. Model binaries và ledger thực tế trên Drive không nằm đầy đủ trong review bundle; bản báo cáo không tuyên bố đã kiểm tra độc lập những phần thiếu này.

# 15 Đánh giá tổng thể và kế hoạch hoàn thiện

## Đánh giá ở trạng thái hiện tại

Đề tài đã vượt mức một notebook dự báo đơn lẻ: có dữ liệu Việt Nam, benchmark, delay-aware calibration, kiểm định, đối chứng exposure, audit toàn request và DSS có ghi quyết định. Nền tảng đủ cụ thể để trình kết quả và thảo luận khóa luận MIS kết hợp định lượng. Phần còn thiếu để chốt giá trị thực tiễn là đánh giá người dùng và xác nhận trên dữ liệu chưa được xem, cùng kiểm chứng nguồn giá.

| Ưu tiên | Công việc | Tiêu chí hoàn thành |
| --- | --- | --- |
| 1 Nguồn dữ liệu | Xác minh OHLC/adjustment, corporate actions, total return và lịch chính thức | Có provenance theo mã/ngày; unresolved gaps được xử lý có bằng chứng |
| 2 Protocol xác nhận | Định trước model, intervals, K, costs và outcome của giai đoạn mới | Forecast/decision ledger khóa trước khi outcome xuất hiện |
| 3 Đánh giá MIS | Thử nghiệm nhiệm vụ có đối chứng với và không có DSS | Đo chất lượng quyết định, thời gian, lỗi bỏ sót, usability và cách dùng XAI |
| 4 So sánh công bằng | Đồng nhất sample/refit rồi so equal, gamma, HAR và stack | Báo cả điểm mạnh, variance và null results; không tối ưu theo test cũ |
| 5 Vận hành team | Xác thực, phân quyền, workflow review và monitoring | Kiểm tra conflict, truy vết và bảo vệ dữ liệu; vẫn tách khỏi order routing |

## Nội dung xin ý kiến giảng viên

Chốt tên và trọng tâm thành hệ thống hỗ trợ quyết định quản trị rủi ro; thống nhất liệu portfolio là RQ riêng hay tầng đánh giá bổ sung. Thảo luận đối tượng tham gia, tiêu chí chất lượng quyết định và nguồn dữ liệu xác minh. Giữ các null findings vì chúng giúp tránh diễn giải giảm exposure thành giá trị thông tin.

Không cần refit hoặc thử tham số thêm chỉ để làm p-value nhỏ hơn. Một khóa luận có bằng chứng minh bạch về triage, abstention và hành vi analyst có thể có đóng góp MIS rõ kể cả khi stacking không thắng.

# 16 Nguồn số liệu và cách đọc tài liệu

| Nguồn | Identity dùng trong báo cáo |
| --- | --- |
| Forecast foundation | V92_PIT_v1 • run 7defc440585dee06 |
| Audit foundation | Phase 1.3.1 release 0.4.1 • run 4ea42ef8f6c5022b |
| Quant integration | Phase 1.4.1 release 0.5.1 • run e751fc0f0c7991fd |
| DSS phần mềm | Phase 1.5 • nguồn snapshot giữ đúng integration run |
| Phạm vi thời gian | Snapshot 21/09/2026; đánh giá tổng hợp 04/10/2026 |

Bảng nguồn chính: V92_point_metrics, V92_primary_hac_tests, V92_interval_metrics, V92_interval_paired_comparison, V92_dss_topk_cost, V92_classifier_metrics, V92_probability_reliability, V92_PIT_forecast_coverage, V92_universe_outcome_audit, VQ14_metrics và VQ14_inference. SHA-256 từng file được lưu trong evidence_data.json để đối chiếu.

## Thuật ngữ cần phân biệt

| Thuật ngữ | Cách hiểu trong báo cáo |
| --- | --- |
| h và stock-day | h là số phiên dự báo; stock-day là một mã ở một origin date, không là quan sát độc lập |
| Event | Outcome volatility vượt threshold tiền định; không phải tăng/giảm giá hoặc sinh lời |
| Precision lift | Precision Top-K chia prevalence trong nhóm có score/nhãn |
| Capture toàn request | Phần Top-K bắt trong mọi event có nhãn, kể cả nhóm không có forecast |
| Breach | Realized proxy portfolio volatility 20 phiên vượt target 15%/năm |
| Proxy P&L | Kết quả accounting trên series giá supplied; chưa là lợi nhuận có thể giao dịch |
| pp và bps | pp là điểm phần trăm; 100 bps bằng 1 điểm phần trăm |

Trang HTML mở trực tiếp và không tải nguồn bên ngoài. Bộ lọc cho phép xem toàn bộ aggregated results, latest snapshot và NAV proxy; nút tải bảng xuất đúng dòng đang xem. Thư mục tables giữ CSV gốc, evidence_data.json giữ số liệu và lineage. Số ở phần tóm tắt được làm tròn; bảng nguồn giữ độ chính xác gốc.

Không có dữ liệu review riêng của người dùng trong gói trình bày. Các thay đổi nghiên cứu sau buổi báo cáo nên được tạo thành phiên bản mới để tránh sửa kết quả đã trình.
