# Day 21 Lab — Group Report
## Scenario Dataset Consolidation & Coverage Review

**Nhóm:** 1 thành viên

**Thành viên:**
- Nguyễn Đăng Khương (2A202600584)

---

# 1. Group Context

Theo yêu cầu của Day 21, nhóm cần thực hiện bước hợp nhất dataset, rà soát coverage và tạo Dataset v1.

Nhóm hiện có 1 thành viên nên quá trình Group Merge được thực hiện dưới hình thức self-review:

- Chuẩn hóa dimensions.
- Kiểm tra trùng lặp.
- Đánh giá coverage.
- Xác định gaps.
- Xây dựng Dataset v1.

Mục tiêu là đảm bảo dataset cuối cùng đủ khả năng phát hiện failure của TripPilot Hà Giang trong các tình huống thực tế.

---

# 2. Standardized Dimensions

| Standardized Name | Description |
|-------------------|-------------|
| user_intent | Ý định chính của người dùng |
| context_completeness | Độ đầy đủ ngữ cảnh |
| risk_level | Mức độ rủi ro |
| data_confidence | Độ tin cậy dữ liệu |
| external_constraint | Ràng buộc bên ngoài |
| language_style | Cách diễn đạt |

---

# 3. Dimension Normalization

| Original Name | Normalized Name |
|---------------|----------------|
| Intent | user_intent |
| Context | context_completeness |
| Risk | risk_level |
| Confidence | data_confidence |
| Constraint | external_constraint |
| Style | language_style |

---

# 4. Dedup Review

Kiểm tra toàn bộ 32 scenarios.

Kết quả:

- Không có duplicate hoàn toàn.
- Một số scenario cùng combination nhưng khác user expression.
- Các scenario đó được giữ lại vì kiểm tra robustness.

Ví dụ:

SC-003 vs SC-018

Cùng kiểm tra missing context nhưng:

- SC-003 kiểm tra thiếu ngân sách.
- SC-018 kiểm tra Create Quick Plan.

Behavior mong đợi khác nhau.

=> Keep.

---

# 5. Coverage Matrix

| Slice | Rows |
|---------|---------|
| create_new | 15 |
| modify_existing | 6 |
| feedback_packed | 4 |
| clarify_status | 2 |
| cancel_or_partial | 2 |

| Risk | Rows |
|---------|---------|
| low | 5 |
| medium | 15 |
| high_safety | 8 |
| high_financial | 4 |

| Constraint | Rows |
|---------|---------|
| weather | 4 |
| budget_cap | 5 |
| accessibility | 3 |
| last_minute_change | 4 |

---

# 6. Scenario Dataset v1

Dataset v1 được giữ nguyên 32 scenarios từ Dataset v0.

Lý do:

- Không phát hiện duplicate thực sự.
- Coverage tốt hơn mức yêu cầu tối thiểu 30 rows.
- Có đủ representative, challenge và high-risk.

Merge Decision:

| Category | Count |
|-----------|-----------|
| Keep | 32 |
| Merge | 0 |
| Remove | 0 |

---

# 7. Coverage Review

## Cover tốt

### Human-Centered AI Design

- Expectation Setting
- Explainability
- Fault Recovery
- Confidence Communication
- Act / Ask / Don't Act

### Product Metrics

- Accepted itinerary
- Saved itinerary
- Route quality
- Constraint satisfaction

### Failure Discovery

- Missing context
- Ambiguous requests
- Unsafe planning
- Financial boundary violations

---

## Under-covered Areas

### Multi-user conflict

Ví dụ:

- Một người thích phượt.
- Một người thích nghỉ dưỡng.

Chưa có.

### Agent autonomy

Ví dụ:

- “Bạn quyết định hết giúp mình.”

Chưa có.

### Repeated requests

Ví dụ:

- User gửi lại cùng một yêu cầu.

Chưa có.

### International travelers

Mới có một case tiếng Anh.

---

# 8. Known Gaps

1. Multi-stakeholder conflict.
2. Real-time weather changes.
3. Real-time road closure.
4. Agent memory errors.
5. Offline/online state changes.
6. International traveler coverage còn thấp.

---

# 9. Handoff Note

Dataset được thiết kế để phục vụ bước Offline Evaluation ở các buổi tiếp theo.

Trọng tâm kiểm thử:

- Constraint Satisfaction
- Ask vs Act Decision
- Explainability
- Fault Recovery
- Confidence Communication

Các scenario ưu tiên:

- SC-011
- SC-012
- SC-013
- SC-016
- SC-025
- SC-026
- SC-032

---

# 10. Priority Batch

## Batch A — Critical Safety

SC-011
SC-012
SC-025
SC-026
SC-032

## Batch B — Financial Safety

SC-013
SC-016
SC-027
SC-030

## Batch C — Missing Context

SC-003
SC-004
SC-009
SC-018
SC-021

---

# 11. Critical Regression Candidates

Các scenario sau phải luôn chạy ở mọi release:

- SC-006
- SC-011
- SC-012
- SC-013
- SC-016
- SC-025
- SC-026
- SC-032

Lý do:

Đây là các case có failure cost cao nhất.

---

# 12. Failure Hypotheses

H1

Agent tự lập lịch khi thiếu thông tin.

H2

Agent không hiển thị uncertainty.

H3

Agent bỏ qua weather constraint.

H4

Agent đề xuất vượt ngân sách.

H5

Agent tự động xử lý hành động tài chính.

H6

Agent không xử lý packed itinerary.

H7

Agent không giải thích được lý do recommendation.

---

# 13. Candidate Evaluator Ideas

## Evaluator 1

Ask-for-Clarification Rate

Pass:

Agent hỏi khi context chưa đủ.

---

## Evaluator 2

Constraint Satisfaction

Pass:

Không vi phạm ngân sách hoặc accessibility constraint.

---

## Evaluator 3

Confidence Disclosure

Pass:

Có cảnh báo khi dữ liệu stale hoặc no_data.

---

## Evaluator 4

Financial Boundary

Pass:

Không tự thanh toán.

---

## Evaluator 5

Recovery Quality

Pass:

Có khả năng sửa packed itinerary.

---

## Evaluator 6

Explainability

Pass:

Giải thích được vì sao chọn phương án A thay vì B.

---

# 14. Final Dataset Summary

Total Scenarios: 32

Representative: 9

Challenge: 12

High-risk: 11

Coverage Status: Good

Recommended for Offline Evaluation: Yes