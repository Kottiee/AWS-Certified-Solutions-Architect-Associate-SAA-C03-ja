# Practice Test 4 - Quick Review

**Date:** March 1, 2026 | **Score:** 49/65 (75.38%) | **Result:** ⚠️ BORDERLINE PASS

---

## 📊 Quick Stats

- ✅ Correct: 49 questions (-3 from Test 3) 
- ❌ Incorrect: 16 questions (+3 from Test 3)
- 📉 **Regression:** -4.62% from Test 3
- ⏱️ Time: 98 minutes 56 seconds
- ⚠️ **CONCERN:** Performance drop after reaching 80%

---

## 📈 Domain Scores

| Domain | Score | Status | Change from Test 3 |
|--------|-------|--------|-------------------|
| Design Resilient Architectures | 73.68% | ⚠️ Borderline | ↘️ -4.32% |
| Design High-Performing Architectures | 77.27% | ⚠️ Good | ↗️ +2.27% |
| Design Secure Architectures | 84.62% | ✅ Strong | ↗️ +2.62% |
| Design Cost-Optimized Architectures | **60.00%** | ❌ **WEAK** | ↘️ **-25%** 🚨 |

---

## 🚨 CRITICAL ALERT: Cost Optimization Crashed!

**Cost-Optimized went from 85% → 60%** (-25 points!)

This is the **#1 priority** to fix before Test 5.

---

## ❌ Key Mistakes to Review (16 incorrect)

### Cost-Optimized Architectures (4 mistakes) 🚨 CRITICAL
1. **Q13:** Redshift Snapshot Costs
   - **Mistake:** Increase automated retention to 35 days
   - **Correct:** Delete unneeded manual snapshots (they cost $ until deleted!)
   - 💰 **Key:** Manual snapshots persist indefinitely, automated expire
   - 📚 Module 05 - Redshift backup lifecycle

2. **Q17:** S3 Glacier Retrieval Options
   - **Mistake:** Selected Expedited (1-5 min, expensive)
   - **Correct:** Standard (3-5 hours, cost-effective)
   - 💰 **Key:** Expedited = fast but $$, Standard = balanced, Bulk = cheap but slow
   - 📚 Module 04 - S3 Glacier retrieval tiers

3. **Q26:** Shared Low-Latency Storage for Data Science
   - **Mistake:** Selected EBS Multi-Attach
   - **Correct:** S3 Standard (scalable, shared, cost-effective)
   - 💰 **Key:** EBS Multi-Attach is same-AZ only, limited use case
   - 📚 Module 04 - Storage selection for shared workloads

4. **Q30:** Database Read Scaling
   - **Mistake:** Configure Multi-AZ replicas for reads
   - **Correct:** ElastiCache to offload reads
   - 💰 **Key:** Multi-AZ = HA failover only, not for read scaling
   - 📚 Module 04 - ElastiCache for cost-effective scaling

### Resilient Architectures (5 mistakes)
5. **Q7:** Auto Scaling with Custom Metrics
   - **Mistake:** Selected CPU/Network (predefined metrics)
   - **Correct:** Memory requires CloudWatch agent (NOT predefined)
   - 📚 Module 09 - CloudWatch custom metrics

6. **Q22:** Load Balancer for Host-Based Routing
   - **Mistake:** Selected NLB
   - **Correct:** ALB (Layer 7 for HTTP host headers)
   - 📚 Module 06 - ALB vs NLB routing capabilities

7. **Q35:** CloudFront Cache Refresh for Updated Config
   - **Mistake:** Wait 24 hours or set TTL=0
   - **Correct:** Invalidate the specific object immediately
   - 📚 Module 06 - CloudFront invalidation

8. **Q37:** Auto Scaling for High Availability
   - **Mistake:** Upsize to largest instance
   - **Correct:** ELB health checks + Auto Scaling across AZs
   - 📚 Module 03 - HA architecture patterns

9. **Q53:** SNS for Message Routing
   - **Mistake:** Confused with other messaging services
   - **Correct:** SNS for pub/sub to multiple SQS queues
   - 📚 Module 08 - SNS fan-out pattern

### High-Performing Architectures (5 mistakes)
10. **Q5:** VPN Access for External Users
    - **Mistake:** Selected Site-to-Site VPN
    - **Correct:** AWS Client VPN (user-level authentication)
    - 📚 Module 06 - Client VPN vs Site-to-Site VPN

11. **Q16:** ECS Task-Level Security
    - **Mistake:** Default networking provides task-level SGs
    - **Correct:** Must use awsvpc networking mode for per-task ENIs/SGs
    - 📚 Module 03 - ECS networking modes

12. **Q32:** Secrets Manager Rotation Compliance
    - **Mistake:** Global switch to enable rotation
    - **Correct:** AWS Config rule `secretsmanager-rotation-enabled-check` + SNS
    - 📚 Module 07 - Secrets Manager compliance monitoring

13. **Q38:** Private Access to S3/DynamoDB
    - **Mistake:** Selected interface endpoint for S3
    - **Correct:** Gateway endpoints for both S3 and DynamoDB
    - 📚 Module 06 - VPC Gateway vs Interface endpoints

14. **Q46:** Shared Storage Across Regions
    - **Mistake:** Selected wrong FSx or gateway type
    - **Correct:** Specific storage service based on requirements
    - 📚 Module 04 - Storage Gateway and FSx options

### Secure Architectures (2 mistakes)
15. **Q29:** Secrets Manager Rotation for API Keys
    - **Mistake:** Use Parameter Store or multiple secrets
    - **Correct:** Customize rotation Lambda in Secrets Manager
    - 📚 Module 07 - Secrets Manager custom rotation

16. **Q36:** S3 Bucket Access Control
    - **Mistake:** Explicit deny or NotPrincipal
    - **Correct:** Bucket policy allowing specific role + users assume role
    - 📚 Module 07 - S3 least-privilege access patterns

---

## 🎯 Critical Issues Analysis

### 🚨 #1 CRITICAL: Cost Optimization Collapse
**What Happened:**
- Score dropped from 85% to 60% (-25 points!)
- Made basic mistakes on Redshift, Glacier, storage selection
- Confused cost-effective solutions with expensive alternatives

**Why It Happened:**
- Didn't review cost optimization after Test 3
- Focused too much on performance/resilience
- Forgot basic cost principles (manual snapshot costs, Glacier tiers)

**Fix Required:**
- ⚠️ **URGENT:** Dedicated 4-hour cost optimization review
- Review ALL cost-related questions from Tests 1-4
- Create cost comparison cheat sheet
- Practice cost calculator scenarios

### ⚠️ #2 Persistent CloudWatch Issues
- **Still making mistakes on custom metrics** (Q7, Q32)
- CloudWatch agent concepts not fully absorbed
- Need hands-on practice, not just theory

### ⚠️ #3 ECS Networking Confusion
- **awsvpc mode still unclear** (Q16)
- Difference between networking modes not memorized
- Need to actually deploy ECS with different modes

---

## 🎯 Urgent Study Plan for Test 5

### 🔴 CRITICAL (Must Complete - 8 hours)

#### 1. Cost Optimization Deep Dive (4 hours) 🚨
**Morning Session (2 hours):**
- Re-watch Module 13: Cost Optimization entirely
- Create cost comparison table:
  - Redshift: Manual vs automated snapshots
  - S3 Glacier: Expedited vs Standard vs Bulk (time + cost)
  - Storage: S3 Standard vs EBS vs EFS (when to use what)
  - Compute: On-Demand vs Reserved vs Spot
  
**Afternoon Session (2 hours):**
- Review all cost-related mistakes from Tests 1-4
- AWS Pricing Calculator: Practice 5 real scenarios
- Create flashcards:
  - Manual snapshots = cost until deleted
  - Glacier Standard = 3-5hr = cost-effective
  - Multi-AZ RDS = HA not read scaling
  - ElastiCache = offload reads = save DB cost

#### 2. CloudWatch Agent Hands-On (2 hours)
- **Lab:** Install CloudWatch agent on EC2
- **Lab:** Publish custom memory metric
- **Lab:** Create alarm on custom metric
- **Lab:** Configure `aggregation_dimensions` for ASG

#### 3. ECS Networking Hands-On (2 hours)
- **Lab:** Deploy ECS task with bridge mode
- **Lab:** Deploy ECS task with awsvpc mode
- **Lab:** Configure task-level security groups
- **Lab:** Test dynamic port mapping with ALB

### 🟡 IMPORTANT (Review - 4 hours)

#### 4. CloudFront & CDN (1 hour)
- Review custom error responses
- Practice invalidation in console
- Understand TTL vs invalidation trade-offs

#### 5. VPN & Connectivity (1 hour)
- Client VPN vs Site-to-Site VPN
- When to use each type
- Authentication methods

#### 6. Gateway vs Interface Endpoints (1 hour)
- S3 and DynamoDB = Gateway endpoints (free!)
- Other services = Interface endpoints ($$)
- Cost implications

#### 7. Secrets Manager (1 hour)
- Custom rotation Lambda
- AWS Config compliance checking
- When to use vs Parameter Store

---

## ✅ Action Items Before Test 5

### Must Complete (Priority 1)
- [ ] 🚨 **4-hour cost optimization intensive review**
- [ ] Create cost comparison cheat sheet
- [ ] Complete CloudWatch agent hands-on lab
- [ ] Complete ECS networking hands-on lab
- [ ] Review all 16 incorrect questions with detailed notes

### Should Complete (Priority 2)
- [ ] CloudFront custom error pages lab
- [ ] VPC endpoints comparison lab
- [ ] Secrets Manager rotation lab
- [ ] Create flashcards for all weak topics

### Nice to Have (Priority 3)
- [ ] Watch cost optimization AWS re:Invent talks
- [ ] Read cost optimization best practices whitepaper

---

## 💡 Key Learnings

### ❌ What Went Wrong
1. **Didn't maintain focus on cost optimization** - Assumed it was "done"
2. **Rushed through some questions** - Made avoidable mistakes
3. **Theoretical knowledge without hands-on** - CloudWatch, ECS still unclear
4. **Forgot basic principles** - Manual snapshots cost money!

### ✅ What to Do Differently for Test 5
1. **Review ALL domains before each test** - Even "strong" ones
2. **Slow down on cost questions** - Calculate if needed
3. **Hands-on labs mandatory** - Not optional for weak topics
4. **Create comparison tables** - Visual references help

---

## 📝 Must Remember Cheat Sheet

**Cost Optimization:**
```
Manual Snapshots = Cost $ until you delete them
Automated Snapshots = Expire based on retention period
S3 Glacier Tiers:
  - Expedited: 1-5 min, $$$
  - Standard: 3-5 hours, $ (most cost-effective)
  - Bulk: 5-12 hours, ¢ (cheapest but slowest)
Multi-AZ RDS = HA failover ONLY (not for reads)
ElastiCache = Offload DB reads = Lower DB cost
```

**CloudWatch:**
```
Default Metrics: CPU, Network, Disk I/O (NOT memory!)
Custom Metrics: Memory, disk space, custom app metrics
Agent Required: For memory, detailed disk, custom metrics
aggregation_dimensions = ASG-level metric aggregation
```

**ECS Networking:**
```
bridge = Default, Docker network, port mapping
host = Share host network, no port mapping
awsvpc = ENI per task, Security Groups per task ✅
```

**Load Balancers:**
```
ALB = Layer 7, HTTP/HTTPS, host/path routing
NLB = Layer 4, TCP/UDP, ultra-low latency
CLB = Legacy, basic load balancing
```

**VPN:**
```
Client VPN = Individual users, user authentication
Site-to-Site VPN = Network to network, customer gateway
```

**VPC Endpoints:**
```
Gateway = S3, DynamoDB (FREE routing, no data charge)
Interface = Other AWS services ($ per hour + data)
```

---

## 🎯 Goals for Test 5

**Target Score:** 85%+ (55/65 questions)
**Must Achieve:**
- Cost Optimization: 90%+ (critical recovery)
- High-Performing: 85%+ (maintain improvement)
- Resilient: 80%+ (improve from 73.68%)
- Secure: 85%+ (maintain strength)

**Confidence Level:** 🟡 MODERATE - Need focused cost review first

**Do NOT take Test 5 until:**
- ✅ Completed 4-hour cost optimization review
- ✅ Completed CloudWatch agent lab
- ✅ Completed ECS networking lab
- ✅ Reviewed all 16 incorrect questions
- ✅ Can explain cost trade-offs for common scenarios

---

**Next Steps:** Complete urgent study plan, then take Test 5 in 2-3 days  
**Exam Ready:** Not yet - need 85%+ consistency across multiple tests

