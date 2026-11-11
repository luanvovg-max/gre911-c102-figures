# C102 — audit Product schema và phương án bổ sung

**Trạng thái: Draft, tài liệu và dữ liệu kiểm tra; chưa thêm schema vào ứng dụng.** Không merge/deploy nếu Founder chưa duyệt. Phạm vi yêu cầu: kiểm tra ảnh, SKU, giá của schema so với nội dung production, chuẩn bị Draft PR nếu cần thay đổi.

## Kết luận đã xác minh

Production `https://c102.gre911.com/` hiện không có Product schema: **0 JSON-LD, 0 Microdata, 0 RDFa** tại lúc mở trang và sau khi lần lượt chọn đủ 100 sản phẩm. Vì vậy, đối chiếu **schema ↔ nội dung live là N/A**, không được báo là schema đã PASS hoặc đã sửa.

Đối chiếu riêng **UI live ↔ dữ liệu ứng dụng** đạt **100/100 sản phẩm**: tên, SKU, giá hiển thị, ảnh hero thuộc đúng gallery và thứ tự toàn bộ thumbnail khớp. **75 mẫu có giá số**, **25 mẫu có `price: null` và hiển thị “Giá liên hệ”**. Không có SKU trùng hoặc giá số lệch chuỗi hiển thị. Đây là kiểm tra tính nhất quán với live, không phải xác nhận SKU/giá theo Product Master hoặc bảng giá đối tác.

## Phạm vi bằng chứng

- Main/production source: `50adbcaf3fe5fb642169a821c6c30549cdd01cfe`.
- Bundle live: `assets/index-CpU-j9AU.js`, SHA-256 `6749a0fa0fdd7e8c93a29da2a42ab6ab39dbb46c1e7ae2339fba4e5ed0af4cda`; tải lại và so khớp trong phiên audit này.
- Dữ liệu: `src/figures-data.json`, 100 SKU, 474 tham chiếu ảnh / 374 file khác nhau.
- Ảnh: đường dẫn `/images/products/Partners/C102/Figures/HighRes/`; so ảnh gốc với URL hero/thumbnail sau khi bỏ tham số resize ImageKit. Tham số resize khác nhau không phải ảnh khác sản phẩm.
- Trình duyệt: chọn từng SKU qua tìm kiếm thật, đọc các trường DOM đang hiển thị; đồng thời quét schema sau mỗi lựa chọn.
- URL luôn giữ `/`; các link được quan sát dùng `#`, không có đường dẫn sản phẩm riêng trong UI. Không thấy canonical tag trong DOM.

| Bằng chứng | Tệp |
|---|---|
| Tóm tắt đã pin source/hash/thời gian | [summary](evidence/C102_SCHEMA_AUDIT_SUMMARY_20261111.json) |
| Toàn bộ 100 lần đọc UI và schema | [live parity](evidence/C102_LIVE_PRODUCT_PARITY_20261111.json) |
| Schema/link/canonical ban đầu | [baseline](evidence/C102_LIVE_SCHEMA_BASELINE_20261111.json) |
| Ảnh, SKU và giá từng sản phẩm | [ma trận CSV](evidence/C102_PRODUCT_FIELD_MATRIX_20261111.csv) |

## Vì sao Draft này chưa chèn 100 Product vào trang chủ

Trang hiện tại là catalog 100 sản phẩm độc lập trên cùng một URL. Google khuyến nghị Product rich results cho trang tập trung vào một sản phẩm hoặc các biến thể của cùng sản phẩm, thay vì trang danh mục. Việc không có Product markup trên catalog tự nó chưa chứng minh một lỗi schema. [Hướng dẫn Product snippet của Google](https://developers.google.com/search/docs/appearance/structured-data/product-snippet#guidelines).

Thêm một Product cố định của FIG-C102-001 rồi để UI chuyển sang FIG-C102-017 sẽ tạo nguy cơ schema sai sản phẩm. Chèn 100 Product/Offer để biến catalog thành trang rich result cũng không giải quyết được yêu cầu trang tập trung vào một sản phẩm. Schema phải phản ánh đúng nội dung người đọc thấy. [Chính sách structured data của Google](https://developers.google.com/search/docs/appearance/structured-data/sd-policies).

Vì vậy, Draft này đưa ra bằng chứng, quy tắc ánh xạ và mẫu JSON-LD để Founder duyệt hướng bổ sung trước khi mở rộng URL/ứng dụng. **Không có bản sửa runtime, không thay đổi giá/SKU và không hứa có rich results.**

## Quy tắc bổ sung Product sau khi duyệt cấu trúc trang

| Trường | Nguồn và điều kiện |
|---|---|
| `name`, `description`, `sku` | Cùng bản ghi FIGURES mà trang chi tiết dùng để render; không gõ cứng một SKU cho cả catalog |
| `image` | Danh sách `gallery`, cover trước, URL tuyệt đối hiện hoạt động; không dùng prefix cũ hoặc ảnh sản phẩm khác |
| `brand.name` | Theo dữ liệu và tên thương hiệu được hiển thị; cần Product Master xác nhận trước phát hành nội dung kinh doanh |
| `offers.price` | Chỉ khi có giá số hợp lệ và giá hiển thị tương ứng; ví dụ `450000`, không dùng `450.000₫` làm giá máy đọc |
| `offers.priceCurrency` | `VND` cho giá đang hiển thị trên website |
| Giá liên hệ | Bỏ `offers`; tuyệt đối không đổi `null` thành `0`, không đặt giá tối thiểu hoặc dùng giá của SKU khác |
| Tồn kho và chính sách | Không tự thêm `InStock`, `PreOrder`, ngày hết hạn giá, vận chuyển, đổi trả hoặc trạng thái hàng mới khi chưa có nguồn được duyệt |
| Đánh giá và mã quốc tế | Không tạo rating/review/GTIN/MPN từ SKU nội bộ |
| `url`, `@id`, canonical | Chỉ thêm khi đường dẫn sản phẩm riêng đã được duyệt, truy cập trực tiếp đúng SKU và được crawl; không quảng bá URL giả hoặc chỉ dùng fragment để giả lập trang sản phẩm |

`Offer.price = 0` biểu thị sản phẩm miễn phí, không phải “liên hệ báo giá”. [Tài liệu giá Offer của Google](https://developers.google.com/search/docs/appearance/structured-data/product-snippet#offer).

Các Product không có offer/review/rating có thể mô tả đúng thực thể nhưng không đủ các thuộc tính để đạt Product snippet của Google. Không bịa thuộc tính để đạt kiểm thử.

### Hai mẫu đề xuất — chưa chạy trên live

- [FIG-C102-017](evidence/PROPOSED_FIG-C102-017_NOT_DEPLOYED.json): SKU FIG-C102-017, đủ 5 URL ảnh, giá `450000` VND, không suy đoán tồn kho.
- [FIG-C102-043](evidence/PROPOSED_FIG-C102-043_NOT_DEPLOYED.json): giá live “Giá liên hệ”, không có `offers` hoặc giá 0.

Hai mẫu cố ý chưa có URL/canonical vì cấu trúc đường dẫn chưa được chốt. Chúng là mẫu để review ánh xạ dữ liệu, không phải schema đã publish hoặc đã được Google nghiệm thu.

## Phương án cần Founder duyệt

1. Giữ `/` là catalog; nếu thêm schema cho trang này, dùng mô tả trang danh mục phù hợp như `CollectionPage`, không coi việc đó là triển khai Product rich results. [Schema.org CollectionPage](https://schema.org/CollectionPage).
2. Chuẩn bị URL sản phẩm riêng theo một quy ước được duyệt. Truy cập trực tiếp, tải lại, quay lại và link chia sẻ phải mở đúng SKU; xử lý SKU không tồn tại bằng 404 thật.
3. Render HTML và Product JSON-LD từ cùng dữ liệu tại build/server để ảnh, giá, SKU không lệch nhau; mỗi trang có canonical riêng. Giữ `null` là không có offer. Trang catalog tìm kiếm được link đến từng trang sản phẩm.
4. Kiểm tra preview toàn bộ 100 trang: đối chiếu từng trường với UI, đủ 374 ảnh, 75 giá số và 25 giá liên hệ; kiểm tra không có schema/metadata của SKU trước khi đổi trang.
5. Sau khi có trang thực tế, chạy Schema.org Validator và Google Rich Results Test cho trang có giá số, trang giá liên hệ và đường dẫn lỗi. Báo rõ kết quả schema hợp lệ so với đủ điều kiện rich result; không tự kết luận Google đã index hoặc hiển thị giá.
6. Founder duyệt HEAD triển khai riêng trước merge/deploy. Sau phát hành mới kiểm tra live và dùng Search Console/URL Inspection theo phạm vi được phép.

## Kiểm tra Draft này

- JSON parse được, đủ 100 bản ghi UI, không có mismatch.
- Mẫu 017 khớp 5 ảnh và 450000 VND trong bản ghi live; mẫu 043 không có offer.
- Không thay đổi `index.html`, `src/figures-data.json`, cấu hình build, workflow hoặc nội dung production.
- Chưa chạy Rich Results Test/Schema.org Validator đối với hai mẫu đề xuất; không báo PASS từ hai công cụ đó.
- Các bằng chứng là snapshot ngày 11/11/2026; phải đọc lại live/source trước triển khai sau này.

**Gate hiện tại:** Founder duyệt hướng trang sản phẩm và ánh xạ schema. Không merge/deploy Draft này theo GO của PR #1; GO đó chỉ áp dụng bản sửa ảnh đã phát hành.
