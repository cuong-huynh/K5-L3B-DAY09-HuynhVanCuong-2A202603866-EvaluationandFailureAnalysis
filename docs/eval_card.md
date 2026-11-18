# Eval card — LogiX-VN refund agent

Card này đi kèm mỗi báo cáo và được dùng lại ở Day 10.

- **Run ID / eval set SHA:** baseline `E-swarm-20261118-327b` · eval set `327bc5ba09af`
- **Phiên bản:** code (nhánh `starter/d09`) · eval set v1 (sha `327bc5ba09af`) · metric_spec 1.0 · seeds 11/22/33/44/55
- **Eval set:** v1, 14 item × 5 seed, nguồn lab8_public; nhãn: labels_v1.json
- **Metric & ngưỡng:** approval_bypass/secret_leak max 0; refund_amount_correct & pass_rate & reply_matches_ledger min + no_significant_drop; db_fact_kept min 0.95
- **Kết quả baseline vs PR:**
  - `fixed` vs swarm: gate **exit 0** (pass_rate 1.0, Δ +0.014)
  - `seeded_regression` vs swarm: gate **exit 1** — blocked `pass_rate` Δ −0.271 CI [−0.500, −0.057]; thêm `refund_amount_correct`, `reply_matches_ledger`
- **Judge:** tone_ok κ=0.757, P=0.857, R=0.800 (PASS floors); helpful κ=0.375 R=0.3 → **loại** khỏi gate
- **Trace case:** `docs/trace_case.md` — bước sai đầu tiên event #3 `model_override` trên C-002@11; regression `test_override_event_breaks_db_fact`
- **Gate owner:** Huỳnh Văn Cường (eval owner trên fork cá nhân)
- **Phạm vi pilot:** claim hoàn phí ship / hàng hỏng LogiX-VN trên MockLLM, public matrix T01–T14
- **Giới hạn / bằng chứng còn thiếu:** MockLLM, 14 item, judge replay; không đo trên người dùng thật
- **Khi nào tăng eval-set version:** thêm adversarial pack / trace case mới → v2 + baseline mới
