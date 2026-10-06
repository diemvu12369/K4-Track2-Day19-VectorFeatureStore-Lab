# Reflection — Lab 19

**Tên:** Vũ Minh Điềm
**Cohort:** A20-K4
**Path đã chạy:** lite (Windows 11, Python 3.13, `bge-small-en-v1.5`)

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Precision@10 trung bình: hybrid 78.6% > BM25 77.8% > vector 73.2%.

- **exact** (n=15): BM25 = hybrid = 96.7%, vector 88.7%. Thuật ngữ kỹ thuật
  xuất hiện nguyên văn trong doc nên tín hiệu từ khoá là đủ.
- **paraphrase** (n=15): mọi mode đều yếu (BM25 33.3%, hybrid 32.0%, vector
  24.0%). `bge-small-en` được train cho tiếng Anh, nên câu tiếng Việt diễn đạt
  lại không gần doc đúng trong không gian vector. Chỗ cần sửa là embedding đa
  ngữ (bge-m3), không phải cách fuse.
- **mixed** (n=20): hybrid 100% so với 97.0% / 98.5%. RRF đưa lên đầu các doc
  mà cả hai retriever cùng xếp cao.

**Khi không dùng hybrid:** tra mã lỗi, SKU, tên hàm, ID — chỉ cần khớp chính
xác, nên dùng BM25 (rẻ, P99 ~10 ms so với 18 ms). Pure vector phù hợp khi query
và doc khác ngôn ngữ/từ vựng hoàn toàn (cross-lingual, tìm ảnh/đoạn tương tự)
với embedding mạnh. Hybrid cũng không đáng làm nếu ngân sách latency rất chặt,
vì nó gọi hai retriever cho mỗi query.

---

## Điều ngạc nhiên nhất khi làm lab này

Trên Feast 0.66, join key không khai báo `value_type` bị suy luận thành `JSON`
và `materialize` vỡ (NB8). Ngoài ra, PIT join bỏ hẳn dòng có event xảy ra
trước khi feature được ghi, chứ không trả về NaN.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
