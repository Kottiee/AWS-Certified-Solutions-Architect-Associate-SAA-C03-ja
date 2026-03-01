# Practice Test 2 - Quick Review

**Date:** March 2, 2026 | **Score:** 49/65 (75.38%) | **Result:** ⚠️ BORDERLINE PASS

---

## 📊 Quick Stats

- ✅ Correct: 49 questions (+7 from Test 1)
- ❌ Incorrect: 16 questions (-7 from Test 1)
- 📈 Improvement: +10.76%
- ⏱️ Time: Not recorded

---

## 📈 Domain Scores

| Domain | Score | Status | Change from Test 1 |
|--------|-------|--------|-------------------|
| Design Resilient Architectures | ~75% | ⚠️ Good | +3.57% |
| Design High-Performing Architectures | ~70% | ⚠️ Borderline | +26.52% ✅ |
| Design Secure Architectures | ~78% | ⚠️ Good | +0.22% |
| Design Cost-Optimized Architectures | ~80% | ⚠️ Good | -20% |

---

## ❌ Key Mistakes to Review (16 incorrect)

### High-Performing Architectures (Still Weak)
1. **CloudWatch Monitoring** - Still confusion about custom metrics
2. **ECS Networking** - Dynamic port mapping requirements
3. **S3 Optimization** - Parallel operations and prefix strategies
4. **Caching Strategies** - When to use ElastiCache vs CloudFront

### Resilient Architectures
5. **FSx for Lustre** - DataSync integration and S3 data repository
6. **Disaster Recovery** - RDS read replica promotion process
7. **CloudFront Configuration** - Custom error pages and invalidation

### Secure Architectures  
8. **VPC Endpoints** - Gateway vs Interface endpoints
9. **KMS Key Policies** - Key users vs key administrators
10. **CloudHSM Backup** - EBK/PBK encryption mechanism

### Cost-Optimized Architectures
11. **Redshift Costs** - Manual snapshot retention
12. **S3 Glacier Retrieval** - Expedited vs Standard vs Bulk
13. **Reserved Instances** - RI types and flexibility

---

## 🎯 Progress Analysis

### ✅ Improvements from Test 1
- **High-Performing Architectures:** +26.52% (massive improvement!)
- **Better time management** (assumed, based on score improvement)
- **CloudWatch concepts** starting to stick
- **ECS fundamentals** getting clearer

### ⚠️ Still Need Work
- **Caching strategies** - Still confusing ElastiCache use cases
- **FSx file systems** - Lustre vs Windows vs NetApp
- **Cost optimization** - Actually dropped 20% (need review!)
- **CloudFront** - Custom error pages, invalidation timing

---

## 🎯 Priority Study Topics for Test 3

### 🔴 CRITICAL (Must Fix Before Test 3)
1. **Cost Optimization - URGENT** (Dropped 20%)
   - Redshift snapshot lifecycle (manual vs automated)
   - S3 Glacier retrieval tiers and costs
   - Reserved Instance types and convertibility
   - Storage tiering strategies

2. **CloudFront & CDN** (Repeated mistakes)
   - Custom error pages configuration
   - Invalidation vs TTL expiry
   - Origin failover setup
   - Signed URLs vs Signed Cookies

3. **FSx File Systems** (New weak area)
   - FSx for Lustre: HPC, ML, S3 integration
   - FSx for Windows: SMB, AD, Multi-AZ
   - DataSync integration
   - When to use each type

### 🟡 IMPORTANT (Reinforce)
4. **ElastiCache Deep Dive**
   - Redis vs Memcached detailed comparison
   - Cache patterns (cache-aside, write-through, write-behind)
   - Integration patterns with RDS
   - Cluster vs replication group

5. **VPC Endpoints**
   - Gateway endpoints (S3, DynamoDB)
   - Interface endpoints (PrivateLink)
   - Endpoint policies
   - Cost comparison

---

## ✅ Action Items

- [ ] **URGENT:** Re-study Module 13: Cost Optimization
  - Focus on Redshift costs, S3 Glacier, RIs
- [ ] Re-watch CloudFront section in Module 06
- [ ] Hands-on Lab: Configure CloudFront custom error pages
- [ ] Re-watch FSx section in Module 04
- [ ] Hands-on Lab: Deploy FSx for Lustre with S3 integration
- [ ] Review all 16 incorrect questions in detail
- [ ] Take Practice Test 3 after 2 days of focused cost optimization study

---

## 📝 Key Learnings

### ✅ What's Working
- **CloudWatch fundamentals** improving
- **ECS networking** much clearer
- **Security concepts** remain strong
- **Study approach** is effective (10.76% improvement)

### ❌ What Needs Attention
- **Cost optimization** - Regression alert!
- **CloudFront configuration** - Repeated mistakes
- **FSx knowledge** - New gap identified
- **Caching strategy selection** - Decision framework needed

---

## 💡 Quick Tips for Test 3

**Cost Optimization:**
- 💰 Manual Redshift snapshots = Cost until deleted
- 💰 S3 Glacier Standard = 3-5 hours = Cost-effective
- 💰 Expedited = 1-5 minutes = Expensive
- 💰 RI Convertible = Change instance family (more flexible, slightly higher cost)

**CloudFront:**
- 🌐 Custom error pages = S3 origin + response page path
- 🌐 Invalidation = Immediate but costs money
- 🌐 Use versioning instead of invalidation when possible

**FSx:**
- 📁 Lustre = HPC, ML, POSIX, S3 integration
- 📁 Windows = SMB, AD, Multi-AZ, Windows workloads
- 📁 DataSync = Automated transfer to/from FSx

---

**Target for Test 3:** 80%+ (52/65 questions)  
**Focus:** Cost optimization (prevent regression) + CloudFront + FSx

