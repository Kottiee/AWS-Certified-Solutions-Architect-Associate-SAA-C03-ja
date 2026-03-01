# Practice Test 3 - Quick Review

**Date:** March 1, 2026 | **Score:** 52/65 (80.00%) | **Result:** ✅ PASS!

---

## 📊 Quick Stats

- ✅ Correct: 52 questions (+3 from Test 2)
- ❌ Incorrect: 13 questions (-3 from Test 2)
- 📈 Improvement: +4.62%
- ⏱️ Time: 99 minutes (1 hour 39 minutes)
- 🎯 **FIRST PASSING SCORE!**

---

## 📈 Domain Scores

| Domain | Score | Status | Trend |
|--------|-------|--------|-------|
| Design Resilient Architectures | 78% | ⚠️ Good | ↗️ +3% |
| Design High-Performing Architectures | 75% | ⚠️ Good | ↗️ +5% |
| Design Secure Architectures | 82% | ✅ Strong | ↗️ +4% |
| Design Cost-Optimized Architectures | 85% | ✅ Strong | ↗️ +5% |

---

## ❌ Key Mistakes to Review (13 incorrect)

### High-Performing Architectures (5 mistakes)
1. **Q15:** EC2 Instance Metadata endpoint - Wrong IP address
   - **Mistake:** Selected wrong 169.254.x.x IP
   - **Correct:** `http://169.254.169.254/latest/meta-data/`
   - 📚 Module 03 - Instance Metadata Service

2. **Q31:** Auto Scaling Target Tracking
   - **Mistake:** Confused target tracking with step scaling
   - **Correct:** Target tracking automatically maintains target metric value
   - 📚 Module 03 - Auto Scaling policies

3. **Q50:** EC2 Public IP Changes on Stop/Start
   - **Mistake:** Thought Elastic IP was changing
   - **Correct:** Public IPv4 changes on stop/start (not EIP)
   - 📚 Module 03 - EC2 IP addressing

4. **Q51:** S3 Consistency Model
   - **Mistake:** Still thought S3 has eventual consistency
   - **Correct:** Strong read-after-write consistency
   - 📚 Module 04 - S3 consistency (MEMORIZE THIS!)

5. **Q64:** Auto Scaling AZ Balancing
   - **Mistake:** Selected step scaling + predictive
   - **Correct:** Target tracking + AZ capacity rebalancing
   - 📚 Module 03 - Auto Scaling AZ distribution

### Resilient Architectures (4 mistakes)
6. **Q17:** FSx for Lustre with DataSync
   - **Mistake:** Selected wrong integration method
   - **Correct:** DataSync supports FSx for Lustre + native S3 integration
   - 📚 Module 04 - FSx for Lustre

7. **Q24:** CloudFront Custom Error Pages
   - **Mistake:** Tried to create second distribution
   - **Correct:** Use S3 + Custom Error Responses in single distribution
   - 📚 Module 06 - CloudFront custom errors

8. **Q52:** Step Functions for Serverless Workflow
   - **Mistake:** Selected SQS for orchestration
   - **Correct:** Step Functions provides visual workflow + state machine
   - 📚 Module 08 - Step Functions vs SWF vs SQS

9. **Q65:** Multi-Region DR with RDS
   - **Mistake:** Tried to update RDS DNS directly
   - **Correct:** Promote read replica, then update application CNAME
   - 📚 Module 05 - RDS cross-region failover

### Secure Architectures (3 mistakes)
10. **Q32:** Secrets Manager Rotation Monitoring
    - **Mistake:** Selected CloudTrail for compliance check
    - **Correct:** AWS Config rule `secretsmanager-rotation-enabled-check`
    - 📚 Module 07 - Secrets Manager compliance

11. **Q35:** CloudHSM Backup Encryption
    - **Mistake:** Thought it used KMS CMK
    - **Correct:** Uses EBK (Ephemeral) + PBK (Persistent) backup keys
    - 📚 Module 07 - CloudHSM backup mechanism

12. **Q47:** NACL vs Security Group for Deny Rules
    - **Mistake:** Tried to use Security Group explicit deny
    - **Correct:** Only NACLs support explicit deny rules
    - 📚 Module 06 - VPC security layers

### Cost-Optimized (1 mistake)
13. **Q44:** S3 Lifecycle Expiration
    - **Mistake:** Confused expiration with Glacier Deep Archive auto-delete
    - **Correct:** Configure lifecycle rule to expire after 365 days
    - 📚 Module 04 - S3 Lifecycle policies

---

## 🎯 Progress Analysis

### ✅ Major Improvements
- **First Passing Score:** 80% achieved! 🎉
- **Cost Optimization:** Recovered to 85% (was 80% in Test 2)
- **Secure Architectures:** Now 82% (consistent improvement)
- **Time Management:** 99 minutes = good pace
- **Confidence:** Higher, reflected in fewer flagged questions

### ⚠️ Still Need Work
- **S3 Consistency Model:** STILL making mistakes (Q51)
- **Auto Scaling Details:** Target tracking vs step scaling confusion
- **CloudFront Configuration:** Custom error pages still unclear
- **EC2 Networking Basics:** IP addressing, metadata endpoints

---

## 🎯 Study Plan Before Test 4

### 🔴 CRITICAL (Must Master)
1. **S3 Consistency Model** - 2 hours
   - **MEMORIZE:** Strong read-after-write for ALL operations
   - **OLD MODEL (wrong):** Eventual consistency for overwrites
   - **NEW MODEL (correct):** Strong consistency everywhere
   - Re-read: [S3 Strong Consistency](https://aws.amazon.com/s3/consistency/)

2. **EC2 IP Addressing & Metadata** - 2 hours
   - Public IPv4 vs Elastic IP behavior
   - Instance Metadata Service: `169.254.169.254/latest/meta-data/`
   - IMDSv2 vs IMDSv1
   - Practice: Access metadata from running instance

3. **Auto Scaling Deep Dive** - 3 hours
   - Target tracking (maintains target value automatically)
   - Step scaling (manual steps based on alarm breach size)
   - AZ rebalancing (distributes capacity evenly)
   - Practice: Create target tracking policy in console

### 🟡 IMPORTANT (Reinforce)
4. **CloudFront Configuration** - 2 hours
   - Custom error responses with S3 origin
   - Invalidation costs and when to use
   - Origin failover configuration
   - Practice: Configure custom 404 page

5. **AWS Config for Compliance** - 2 hours
   - Config rules vs CloudTrail vs Trusted Advisor
   - Managed rules (especially Secrets Manager)
   - Remediation actions
   - Practice: Enable Config rule

6. **RDS Multi-Region DR** - 2 hours
   - Read replica promotion process
   - CNAME vs DNS updates
   - Failover testing
   - Practice: Promote read replica in lab

---

## ✅ Action Items

- [ ] **CRITICAL:** Re-read S3 consistency documentation
- [ ] Practice accessing EC2 instance metadata
- [ ] Create target tracking Auto Scaling policy in lab
- [ ] Configure CloudFront custom error page in lab
- [ ] Review all 13 incorrect questions with detailed notes
- [ ] Create flashcards for:
  - S3 consistency model
  - Auto Scaling policy types
  - Instance metadata endpoint
  - NACL vs Security Group
- [ ] Take Practice Test 4 after 1-2 days of focused review

---

## 💡 Key Learnings & Tips

### ✅ What's Working
- **Cost optimization** fully recovered
- **Security concepts** strong and improving
- **Study rhythm** is effective (consistent improvement)
- **Lab practice** helping with hands-on understanding

### 📝 Must Remember

**S3 Consistency:**
```
✅ CORRECT: Strong read-after-write for ALL operations
❌ WRONG: Eventual consistency (this was old model before Dec 2020)
```

**Auto Scaling Policies:**
```
Target Tracking = Set target value, ASG maintains it automatically
Step Scaling = Manual steps based on alarm breach size
Simple Scaling = Single step, uses cooldown (legacy)
```

**EC2 IP Addressing:**
```
Public IPv4 = Changes on stop/start
Elastic IP = Static, persists across stop/start
Private IP = Never changes
```

**NACL vs Security Group:**
```
NACL = Stateless, subnet-level, explicit deny supported
Security Group = Stateful, instance-level, allow-only
```

**CloudFront Custom Errors:**
```
1. Put error pages in S3
2. Configure Custom Error Responses in distribution
3. Set response page path (e.g., /errors/404.html)
```

---

## 🎯 Goals for Test 4

**Target Score:** 85%+ (55/65 questions)
**Key Focus:** 
1. Don't regress on cost optimization
2. Master S3 consistency model
3. Nail Auto Scaling policy selection
4. Get CloudFront configuration right

**Confidence Level:** 🟢 GOOD - Ready for Test 4 after targeted review

---

**Next Review:** After Practice Test 4  
**Exam Ready:** Not yet - need consistent 85%+ scores across 2-3 tests

