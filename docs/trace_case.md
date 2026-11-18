# Trace case: từ trace tới quyết định sai đầu tiên

Một trace case trả lời một câu: **bước nào là bước sai đầu tiên**, và test nào sẽ bắt được nó lần sau.

```bash
cd src
python -m lab9.trace_case --shape seeded_regression --claim C-002 --seed 11
python -m lab9.trace_case --shape fixed --claim C-002 --seed 11   # đối chứng
```

Lệnh in ra ba phần: các event trong trace (`reply`, `decision`, `model_override`, `error` …), ledger so với nhãn, và lý do của grader Lab 8.

## Case đã điền — seeded_regression C-002@11

| Trường | Nội dung |
|---|---|
| Trial | `seeded_regression C-002@11` — lab8 grader FAIL (`status resolved != denied`) |
| Triệu chứng | Ledger `refund_vnd=20000` vs label `0`; reply nói đã hoàn 20.000đ; `DbFactKept` fail vì có `model_override` |
| Bước sai đầu tiên | Event **#3** `model_override` `{"delay_days": 5}` — tin số ngày trên ticket đè DB |
| Vì sao là bước *đầu tiên* | Event 1–2 chỉ handoff + lookup order (đọc DB đúng). Chỉ từ #3 trở đi quote/refund đi theo delay sai |
| Failure mode | **FM-03** ticket_trust (kèm hậu quả tiền → liên quan FM-02/amount) |
| Đối chứng | `fixed C-002@11`: không có `model_override`; quote `no_entitlement`; status `denied`; ledger 0 — PASS |
| Regression test | `tests/test_lab9_graders.py::test_override_event_breaks_db_fact` + metric `db_fact_kept`; fail trên seeded_regression, pass trên `fixed` |
| Chưa chắc chắn | Không chứng minh được intent “cố ý”; chỉ quan sát được override trên trace |

## Quy tắc

- Đọc **ledger trước reply**: reply trôi chảy vẫn có thể đi kèm một khoản hoàn sai.
- "Bước sai đầu tiên" phải dẫn số event cụ thể, không phải "agent bị nhầm".
- Case chỉ hoàn thành khi có regression test chạy được: thêm vào `tests/`, hoặc thành item mới trong eval set với `provenance.from_trace`.
