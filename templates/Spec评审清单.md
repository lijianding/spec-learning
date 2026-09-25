# Spec Review Checklist

## A. 业务

- [ ] Business Goal 明确
- [ ] Scope 明确
- [ ] Out of Scope 明确
- [ ] Stakeholder 完整
- [ ] Business Rules 有来源
- [ ] 没有把 Assumption 当 Fact

## B. Requirement

- [ ] 每条有唯一 ID
- [ ] 单一
- [ ] 明确
- [ ] 无冲突
- [ ] 可实现
- [ ] 可测试
- [ ] 可追踪
- [ ] Priority 明确
- [ ] Source 明确

## C. Flow / State

- [ ] Main Flow
- [ ] Alternative Flow
- [ ] Exception
- [ ] 状态完整
- [ ] 禁止转换明确
- [ ] Guard 明确

## D. Data

- [ ] Entity 定义
- [ ] Field type
- [ ] Required/Nullable
- [ ] Enum
- [ ] Default
- [ ] Relation
- [ ] Ownership
- [ ] Lifecycle
- [ ] Sensitive Data

## E. API

- [ ] Method/Path
- [ ] Request Schema
- [ ] Success
- [ ] Error
- [ ] Authentication
- [ ] Object Authorization
- [ ] Pagination
- [ ] Idempotency
- [ ] Concurrency
- [ ] Versioning
- [ ] Audit

## F. NFR

- [ ] Performance
- [ ] Capacity
- [ ] Availability
- [ ] Reliability
- [ ] Security
- [ ] Privacy
- [ ] Observability
- [ ] Backup/Restore
- [ ] Compatibility

## G. Failure

- [ ] Invalid input
- [ ] Not found
- [ ] No permission
- [ ] Invalid state
- [ ] Duplicate
- [ ] Concurrency
- [ ] External timeout
- [ ] External unavailable
- [ ] Partial failure

## H. Testing

- [ ] Acceptance Criteria
- [ ] Positive
- [ ] Negative
- [ ] Boundary
- [ ] State transition
- [ ] Authorization
- [ ] Concurrency
- [ ] NFR verification

## I. Traceability

- [ ] Business Goal → Requirement
- [ ] Requirement → API/Data/Design
- [ ] Requirement → Test
- [ ] Change impact identifiable

## J. Review Result

- Reviewer:
- Date:
- Status: Approved / Changes Required
- Blocking Issues:
- Open Questions:
