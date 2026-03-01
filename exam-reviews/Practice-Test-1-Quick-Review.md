# Practice Test 1 - Quick Review

**Date:** March 2, 2026 | **Score:** 42/65 (64.62%) | **Result:** ❌ FAIL

---

## 📊 Quick Stats

- ✅ Correct: 42 questions
- ❌ Incorrect: 23 questions  
- 🚩 Flagged: High uncertainty
- ⏱️ Time: 130 minutes (2 min/question - too slow)

---

## 📈 Domain Scores

| Domain | Score | Status |
|--------|-------|--------|
| Design Resilient Architectures | 71.43% | ⚠️ Borderline |
| Design High-Performing Architectures | 43.48% | ❌ CRITICAL |
| Design Secure Architectures | 77.78% | ⚠️ Good |
| Design Cost-Optimized Architectures | 100% | ✅ Perfect |

---

## ❌ Key Mistakes to Review

### 1. CloudWatch Agent for Custom Metrics (Q2)
**Mistake:** Thought detailed monitoring provides custom metrics  
**Reality:** EC2 memory requires CloudWatch agent with `aggregation_dimensions`  
📚 Study: Module 09 - CloudWatch Agent configuration

### 2. Auto Scaling Cooldown Period (Q21)  
**Mistake:** Didn't know cooldown prevents scaling actions  
**Reality:** ASG won't scale during cooldown even if alarm fires  
📚 Study: Module 03 - Auto Scaling cooldown mechanics

### 3. ECS on Outposts vs Local Zones (Q8)
**Mistake:** Selected Fargate on Outposts  
**Reality:** ECS EC2 launch type on Outposts for lowest latency  
📚 Study: Module 03 - ECS deployment options

### 4. S3 Performance Optimization (Q19)
**Mistake:** Didn't know all optimization strategies  
**Reality:** Need multipart upload + parallelization + catalog index  
📚 Study: Module 04 - S3 performance best practices

### 5. ElastiCache for Database Scaling (Q32, Q52)
**Mistake:** Selected wrong caching strategy  
**Reality:** ElastiCache offloads DB reads, not Multi-AZ replicas  
📚 Study: Module 04 - ElastiCache use cases

### 6. Network ACL vs Security Group (Q47)
**Mistake:** Tried to use Security Group for explicit deny  
**Reality:** Only NACLs support explicit deny rules  
📚 Study: Module 06 - VPC Security (NACL vs SG)

### 7. CloudWatch for Monitoring (Not Trusted Advisor) (Q53)
**Mistake:** Selected Config for underutilized resources  
**Reality:** CloudWatch metrics + Trusted Advisor cost checks  
📚 Study: Module 09 - CloudWatch vs Trusted Advisor

### 8. File Gateway for Hybrid Storage (Q41)
**Mistake:** Selected Tape Gateway  
**Reality:** File Gateway stores files as S3 objects (admin can access via S3 console)  
📚 Study: Module 04 - Storage Gateway types

### 9. EBS Snapshots for Cross-AZ Migration (Q62)
**Mistake:** Thought could directly attach volume to different AZ  
**Reality:** Must create snapshot, then create new volume in target AZ  
📚 Study: Module 04 - EBS cross-AZ operations

### 10. S3 Strong Consistency (Q63)
**Mistake:** Thought S3 has eventual consistency  
**Reality:** S3 has strong read-after-write consistency (since Dec 2020)  
📚 Study: Module 04 - S3 consistency model

---

## 🎯 Priority Study Topics

### 🔴 CRITICAL (Study First - 40 hours)
1. **CloudWatch Agent & Custom Metrics** - 8 hours
   - Installing and configuring agent
   - Publishing custom metrics
   - `aggregation_dimensions` for ASG metrics
   - Memory and disk metrics

2. **ECS Deployment & Networking** - 8 hours
   - awsvpc vs bridge vs host modes
   - Dynamic port mapping with ALB/NLB
   - EC2 vs Fargate launch types
   - Outposts vs Local Zones vs Regions

3. **S3 Performance Optimization** - 8 hours
   - Multipart upload (when and how)
   - Parallel operations and prefix design
   - Transfer Acceleration
   - LIST optimization strategies

4. **ElastiCache Strategies** - 8 hours
   - Redis vs Memcached
   - Cache-aside vs Write-through patterns
   - Integration with RDS
   - Session management

5. **Auto Scaling** - 8 hours
   - Cooldown periods
   - Target tracking vs step scaling
   - AZ rebalancing
   - Health checks (ELB vs ASG)

### 🟡 IMPORTANT (Study Second - 20 hours)
6. **FSx File Systems** - 5 hours
7. **VPC Security (NACL vs SG)** - 5 hours
8. **Storage Gateway Types** - 5 hours
9. **EBS Operations** - 5 hours

### 🟢 REVIEW (Study Last - 10 hours)
10. **Cost Optimization** - Already strong (100%)
11. **IAM & Security** - Already good (77.78%)

---

## ✅ Action Items

- [ ] Re-watch Module 03: Compute (focus on ECS, Auto Scaling)
- [ ] Re-watch Module 09: Monitoring (focus on CloudWatch agent)
- [ ] Hands-on Lab: Install CloudWatch agent and publish metrics
- [ ] Hands-on Lab: Deploy ECS with awsvpc and dynamic ports
- [ ] Hands-on Lab: S3 multipart upload with parallelization
- [ ] Review all 23 incorrect questions with detailed explanations
- [ ] Create flashcards for weak topics
- [ ] Take Practice Test 2 after 2-3 days of focused study

---

## 📝 Quick Tips

💡 **CloudWatch:** Memory is NOT a default metric  
💡 **ECS:** awsvpc mode = ENI per task = Security Groups per task  
💡 **S3:** Strong consistency for ALL operations  
💡 **Auto Scaling:** Cooldown prevents thrashing  
💡 **NACL:** Stateless, supports explicit deny  
💡 **ElastiCache:** Offload reads from database  

---

**Next Review:** After completing priority study topics  
**Target:** Understand ALL incorrect questions before Practice Test 2

