# Judge audit

Chạy: `python -m lab9.judge --csv <file> --audit 5`

| Tiêu chí | n | κ | Precision | Recall | Đạt FLOORS (κ≥0.6, P≥0.8, R≥0.7)? | Quyết định |
|---|---|---|---|---|---|---|
| tone_ok | 100 | 0.757 | 0.857 | 0.800 | có (PASS floors) | dùng (calibrated) |
| helpful | 100 | 0.375 | 1.000 | 0.300 | không (FAIL floors) | loại khỏi gate |

## ≥5 case audit thủ công
| # | Case | Judge nói | Người nói | Ai đúng? | Ghi chú |
|---|---|---|---|---|---|
| 1 | H-008 | PASS | FAIL | Người | Tone suồng sã (“anh check giúp em”) — judge bỏ sót FAIL |
| 2 | H-017 | PASS | FAIL | Người | Xưng “bạn ơi” + số tiền — judge bỏ sót tone fail |
| 3 | H-029 | PASS | FAIL | Người | Câu lạnh/đẩy khách — judge miss |
| 4 | H-042 | FAIL | PASS | Người | Reply lịch sự đúng số; judge false positive |
| 5 | H-051 | FAIL | PASS | Người | Tone ổn; judge quá nhạy với từ “refund” |
| 6 | HL-015 | PASS | FAIL | Người | Helpful judge bỏ sót nội dung không hữu ích (R thấp) |
| 7 | HL-016 | PASS | FAIL | Người | Xin lỗi dài nhưng không giải quyết — FN |

## Kết luận
- **helpful bị loại**: κ=0.375 < 0.6 và recall=0.3 — bỏ sót ~70% FAIL thật (fn=21). Dùng làm gate sẽ để lọt regression “không hữu ích”. Precision=1.0 chỉ nói khi đánh FAIL thì đúng, nhưng hầu như không đánh FAIL.
- **tone_ok** đạt FLOORS (κ/P/R) nên có thể dùng cho theo dõi; tiền/approval vẫn chỉ chấm bằng code.
- Rủi ro còn lại: judge chỉ được kiểm trên dữ liệu replay đã công khai; cần nhãn người mới để có held-out thật.
