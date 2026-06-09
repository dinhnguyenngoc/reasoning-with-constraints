# Reasoning with Constraints — CSP/COP Solvers & Benchmarks

> Đồ án môn **Hệ Cơ sở Tri thức (Knowledge Based Systems)** — Chương trình Thạc sĩ Công
> nghệ Thông tin, Trường Đại học Công nghệ TP.HCM (HUTECH), 08/2025.

Tìm hiểu **lập luận theo ràng buộc (Reasoning with Constraints)** — biểu diễn tri thức dưới
dạng ràng buộc và tìm lời giải thỏa mãn — kèm **demo giải nhiều bài toán CSP/COP thực tế**
bằng `python-constraint` và **OptaPlanner**, có so sánh hiệu năng với brute-force.

## Cơ sở lý thuyết

**CSP — Constraint Satisfaction Problem** biểu diễn bài toán bằng bộ `(X, D, C)`:
- `X` — tập biến · `D` — miền giá trị · `C` — tập ràng buộc → tìm phép gán thỏa **toàn bộ** ràng buộc.

| Khái niệm | Nội dung |
|-----------|----------|
| **Phân loại mục tiêu** | Satisfaction (tìm nghiệm hợp lệ) · Optimization (tìm nghiệm tối ưu) |
| **Loại ràng buộc** | Hard constraints (bắt buộc) · Soft constraints (nên thỏa, tối ưu hóa) |
| **Bậc ràng buộc** | Unary · Binary · k-ary |
| **Không gian phép gán** | `dⁿ` (n biến, mỗi biến d giá trị) → lý do cần cắt tỉa |
| **COP** | Constraint Optimization — tối ưu hàm mục tiêu trên tập nghiệm khả thi |

## Phương pháp giải

1. **Search-based** — generate-and-test (brute-force) → DFS/backtracking với kiểm tra ràng buộc sớm (early pruning).
2. **Consistency Algorithms** — constraint propagation, loại bỏ giá trị bất khả thi khỏi miền *trước* khi gán.
3. **Domain Splitting** — tách miền giá trị của biến thành các bài toán con.
4. **Local Search** — tìm nghiệm nhanh cho không gian lớn/vô hạn (không đảm bảo đầy đủ).
5. **COP solving** — inference-based (propagation) + metaheuristic/hybrid (Genetic, Tabu Search).

## Bài toán thực nghiệm

| # | Bài toán | Loại | Ràng buộc |
|---|----------|------|-----------|
| 1 | **Crossword Puzzle** | Satisfaction | Các từ khác nhau; ký tự giao nhau khớp |
| 2 | **N-Queens** | Satisfaction | Không 2 hậu cùng hàng/cột/đường chéo |
| 3 | **School Timetabling** | Mixed (hard + soft) | Phòng/giáo viên/lớp không trùng giờ; ưu tiên mềm |
| 4 | **Call Center Scheduling** | Mixed | Gán cuộc gọi đúng kỹ năng; tối thiểu thời gian chờ |
| 5 | **Vehicle Routing** | Optimization | Không vượt sức chứa xe; tối thiểu quãng đường |

## Công cụ

- **`python-constraint`** — mô hình hóa & giải CSP.
- **OptaPlanner** (9.44) — giải bài toán tối ưu có ràng buộc (timetabling, routing, scheduling).
- **Brute-force** — baseline so sánh.

## Benchmark — N-Queens

Chạy trên MacBook Pro 2GHz Intel Core i5, 16GB RAM · Python 3.10 · OptaPlanner 9.44.

| N | Số trạng thái (N!) | Brute-force | python-constraint (all) | python-constraint (1 nghiệm) | OptaPlanner (1 nghiệm) |
|---|--------------------|-------------|--------------------------|------------------------------|------------------------|
| 8 | 40,320 | 9.68 ms | 36.2 ms | 2.14 ms | 5 s |
| 12 | 479,001,600 | 9 s | 14 s | 5.87 ms | 5 s |
| 14 | 87,178,291,200 | 5m37s | 31.7 s | 7.18 ms | 5 s |
| 16 | 2.09 × 10¹³ | — | — | 3.70 ms | 5 s |
| 32 | ~2.63 × 10³⁵ | — | — | 16.11 ms | 5 s |

→ CSP với pruning vượt trội brute-force khi N lớn; tìm **1 nghiệm** rất nhanh kể cả N=32.

## Cài đặt & chạy

```bash
pip install python-constraint
# OptaPlanner: chạy trên JVM (Java) — xem thư mục optaplanner/ trong repo
python nqueens_csp.py --n 8
```

> Cập nhật tên file/script cho khớp repo.

## Kết luận

- CSP cho phép biểu diễn bài toán dạng *biến – miền – ràng buộc*, giảm mạnh không gian
  tìm kiếm so với duyệt toàn bộ trạng thái.
- Áp dụng linh hoạt: crossword, N-Queens, timetabling, routing, scheduling.
- Công cụ (`python-constraint`, OptaPlanner) giúp củng cố lý thuyết bằng thực nghiệm.

### Hướng phát triển
- Bài toán lớn hơn (chuỗi cung ứng, lập lịch đa mục tiêu).
- Ứng dụng web cho phép nhập CSP (biến/miền/ràng buộc) và xem lời giải tự động.
- So sánh sâu hơn backtracking vs consistency vs local search.

## Tài liệu tham khảo

- Poole & Mackworth, *Artificial Intelligence: Foundations of Computational Agents* — Ch. 4 *Reasoning with Constraints* (Cambridge, 2010).
- D. Mitra, *CSE 5692: Constraint Reasoning* (Florida Institute of Technology).
- OptaPlanner Use Cases.

## Nhóm thực hiện

Nguyễn Ngọc Đỉnh · Hà Anh Dũng — GVHD: TS. Nguyễn Hùng Sơn
