# 🎯 Mock Interview 03 — Git, Jenkins & CI/CD Pipelines
## Every Wednesday 8:00 PM IST | CloudDevOpsHub | Mentored by Vikas Ratnawat

> 🎓 **Join the WhatsApp Group** → [CloudDevOpsHub Community](http://www.clouddevopshub.com)
> 📺 **Join the Live Session with Vikas from CloudDevOpsHub** → [www.clouddevopshub.com](http://www.clouddevopshub.com)
> ⏰ **Live:** 8:00 AM – 9:15 AM IST | **Daily Q&A:** 9:15 AM – 10:00 AM IST | **Mock Interview:** Every Wednesday 8:00 PM IST

---


## 📋 Mock Interview Overview

| | |
|-|-|
| 🎯 **Focus** | Git, Jenkins & CI/CD Pipelines |
| 📚 **Topics** | Git branching, Jenkins pipeline, Jenkinsfile, Maven builds |
| ⏰ **Schedule** | Every Wednesday 8:00 PM IST |
| ⏱️ **Duration** | 60–90 minutes |
| 👥 **Format** | Live peer + mentor feedback |

---

## ⏱️ Interview Timeline

```
00–05 min  →  "Tell me about yourself" — structured intro
05–20 min  →  Technical Q&A: Git, Jenkins & CI/CD Pipelines
20–35 min  →  Live scenario / troubleshooting problem
35–50 min  →  Architecture design or code walkthrough
50–60 min  →  Your questions for the interviewer (CRITICAL!)
```

---

## ❓ 20 Questions — Git, Jenkins & CI/CD Pipelines

### 🟢 Basic (Q1–Q5)
🟢 Basic

1. Walk me through your experience with Git branching.
At Citi I maintained branching and release workflows across 40+ repositories. The model was simple: main is always releasable, work happens on short-lived feature branches, and merges go through pull requests with reviews. Releases get their own release branch and tag, and urgent fixes go on hotfix branches that merge back to both. I also protected main so nobody could push directly. The main lesson was to keep branches short-lived, because long-lived ones cause painful merge conflicts.

2. What are Git, Jenkins and CI/CD pipelines, and why do they matter?
Git is version control: it tracks every change and lets teams work in parallel. Jenkins is an automation server that reacts to those changes. A CI/CD pipeline is the automated path from commit to production: build, test, package and deploy. Together they replace manual, error-prone releases with fast, repeatable ones. In my experience that moved a release cycle from 2 weeks to 3 days across 25+ applications.

3. Explain the main components in simple terms.

Git: a repository, commits (snapshots), branches (parallel lines of work) and a remote like GitLab or GitHub.
Jenkins: a controller that schedules and coordinates, agents that do the work, jobs or pipelines that define the steps, a Jenkinsfile that holds the pipeline as code, plugins, and a credentials store.
CI/CD pipeline: stages (source, build, test, scan, package, deploy) plus triggers like a webhook on every push.

4. What’s the first thing you check if it fails?
I read the console output to see which stage failed and what the first real error is. Then I ask what changed: a recent commit, a plugin update, or a credential that expired. If the log doesn’t explain it, I check the environment: disk space on the agent, network access to Git or the registry, and tool versions. Most failures are one of those.

5. How do you stay current?
I follow Jenkins security advisories and release notes, read the GitLab and Kubernetes docs, and learn by building. For example, I recently containerised a website on NGINX to practise Docker. I also use GitHub Copilot to speed up my IaC work, and I try new things in a sandbox before I use them in a real pipeline.

🔵 Intermediate

6. Describe a real project.
On the Jio Mart e-commerce platform for Common Grounds Coffee Co., I built Jenkins pipelines for five microservices: Frontend, Product, Order, Payment and Inventory. Each pipeline did build, test, Docker image push to Docker Hub and deploy. That cut release cycle time by 30% and raised pipeline reliability to about 95%. I also documented the workflow and trained the QA team to run the jobs themselves, so routine releases stopped waiting on DevOps.

7. How do you handle security and access control?
I use least privilege everywhere:

Role-based access in Jenkins, with SSO or LDAP where possible.
Secrets in the Jenkins credentials store or HashiCorp Vault, never in code or Jenkinsfiles, and masked in logs.
Protected branches and required reviews in Git.
Regular patching of Jenkins core and plugins.
Vulnerability scanning in the pipeline. At Oracle, that kind of scanning plus Vault helped cut critical pre-production findings by 60%.

8. Walk through setting it up from scratch on a new server.

Install a supported JDK and the Jenkins package, and start it as a service.
Open the port, put NGINX in front with TLS, and unlock Jenkins with the initial admin password.
Install the plugins I need and create admin accounts with role-based access.
Add credentials, connect Git, and set up agents.
Create a multibranch pipeline that reads a Jenkinsfile from the repo, and add a webhook so pushes trigger builds.
Set up backups of the Jenkins home directory, and manage the config as code where I can.

9. What’s your structured approach to troubleshooting in production?
First, contain the impact and roll back if there’s a recent change. Then I identify what changed, check logs and metrics (ELK, Prometheus, Grafana), form a hypothesis, test it, and fix it. Afterwards I write a blameless post-mortem and add a check or alert so it can’t fail silently again. At Citi I used Prometheus and Grafana to watch post-release health.

10. How does it integrate with the broader CI/CD pipeline?
A commit triggers a webhook, and Jenkins runs build and unit tests. A scan step follows, then Docker image build and push to a registry, and a deploy to Kubernetes. Monitoring then feeds back into the next release. Git is the single source of truth, Jenkins is the orchestrator, and Terraform and Ansible handle the infrastructure.

🟡 Scenario-based

11. Production is down and logs point to the pipeline. Your plan?

Stabilise first: if a recent deployment caused it, I roll back to the last good version.
Communicate: I tell stakeholders what’s happening and when to expect updates.
Diagnose: I compare the failing release with the last good one, and check config, credentials and dependencies.
Fix and learn: I apply the fix through the normal pipeline, then run a post-mortem and add a guardrail.

12. A junior engineer misconfigured it and traffic is impacted. What do you do?
I fix the impact first by reverting to the last known good config, which is easy when config lives in Git. I keep it blameless and focus on how the system allowed the mistake. Then I add guardrails: required reviews on config changes, a staging check, and tighter permissions. I also sit with the engineer to walk through what happened.

13. Management wants a 30% cost cut. Your approach?
I’d measure first to find where the money goes. Then:

Use ephemeral agents that scale to zero when idle, and spot instances where the builds can tolerate it.
Cache dependencies and Docker layers to shorten builds.
Skip unnecessary builds with path filters, and clean up old artifacts and logs.
Right-size the controller and agents.
I’d report savings against the baseline so the 30% is provable.

14. Migrate to a new architecture with zero downtime?
I’d run old and new side by side. Everything moves to code first (Jenkinsfiles and config as code), then I mirror pipelines on the new setup and run both in parallel to compare results. I cut over job by job or team by team, and keep the old system read-only for fast rollback until the new one is proven.

15. A security vulnerability is found in your pipeline. Remediation?
I assess severity and exposure, then contain it: restrict access and take the affected component offline if needed. I patch Jenkins or the plugin, rotate any credentials that might have been exposed, and check audit logs for misuse. Then I re-run scans, verify the fix, and document it. To prevent a repeat, I add scanning and patch-management checks to the process.

🔴 Senior / Architect

16. Design for multi-region HA targeting 99.99%.
99.99% means roughly 52 minutes of downtime a year, so there’s no room for single points of failure. Open-source Jenkins isn’t natively active-active, so I’d design around it:

Run the controller on Kubernetes with persistent storage, and keep all config as code so it can be rebuilt quickly in another region.
Use stateless, autoscaling agents in each region.
Replicate source and artifacts across regions, with a multi-region registry.
Deploy applications progressively (canary or blue-green) behind a global load balancer with health checks.
Build everything with Terraform, and test the failover regularly.

17. What monitoring and alerting do you implement?
I track queue wait time, build duration, failure rate, agent availability and disk usage in Prometheus and Grafana, with logs in ELK or Datadog. Alerts go out for a controller down, a growing queue, disk above a threshold and a spike in failures. I also watch delivery metrics like deployment frequency and lead time, to show whether the pipeline is actually improving things.

18. Describe your disaster recovery experience.
My approach is to make the setup rebuildable rather than precious. The controller config is in code, infrastructure is in Terraform and Ansible, and the Jenkins home and secrets are backed up on a schedule to another location. I define recovery time and recovery point targets, and I test restores, because an untested backup isn’t a backup. Adapt this to a real incident of yours if you have one.

19. How do you mentor junior engineers?
I pair with them on real tasks, explain the why behind each step, and give them safe sandboxes to break things. I write short runbooks and documentation, review their pipelines constructively, and hold blameless post-mortems. At Jio Mart I trained the QA team to run Jenkins jobs themselves, and that’s the kind of independence I aim for.

20. Top 3 mistakes teams make and how I prevent them.

Manual, undocumented setups: the server becomes impossible to rebuild. I fix this with pipelines and config as code, with everything in Git.
Secrets in code or logs: I use a secrets manager, credential masking and scanning.
Slow, flaky pipelines and long-lived branches: developers stop trusting them. I keep branches short, parallelise tests, fix flaky tests quickly and treat pipeline speed as a metric.

---

## 📝 "Tell Me About Yourself" Template

```
"I am [Name], a [X-year / fresher] engineer specializing in Multi-Cloud
and DevOps. I am completing the 55-session intensive program at
CloudDevOpsHub, mentored by Vikas Ratnawat — Microsoft MVP and
former engineer at PwC and Amazon.

I have completed Tasks Task-001 through Task-042 covering:
Git branching, Jenkins pipeline, Jenkinsfile, Maven builds

My most impactful project was [Project Name] where I [specific achievement
with measurable metric — e.g., reduced deployment time by 70%].

I am targeting [target role] where I can contribute to [team goal]
and grow towards an architect-level position within 18 months."
```

---

## 🤔 Questions You MUST Ask the Interviewer

```
1. "What does a typical on-call rotation look like for this team?"
2. "What is the biggest technical challenge your DevOps team is solving this quarter?"
3. "How does the team measure success of DevOps initiatives — what KPIs matter?"
4. "What does the tech stack look like today and where is it heading in the next year?"
5. "What growth path do you see for someone in this role over the next 2 to 3 years?"
```

---

## 📊 Self-Evaluation Scorecard

| Area | Score (1–5) | Notes |
|------|------------|-------|
| Technical Accuracy | | |
| Communication Clarity | | |
| Problem-Solving Structure | | |
| Real-World Examples Used | | |
| Unknown Question Handling | | |
| Questions Asked to Interviewer | | |
| **Total** | **/30** | |

```
25–30 : Interview-ready — apply aggressively now
18–24 : Review weak areas — 2 more practice sessions needed
< 18  : Deep practice needed — revisit relevant sessions
```

---

## 📣 LinkedIn Post — After Mock Interview

```
🎯 Mock Interview #03 complete — 55-Session CloudDevOpsHub Challenge!

Focus: Git, Jenkins & CI/CD Pipelines

My honest self-scores today:
Technical: __/5 | Communication: __/5 | Scenarios: __/5 | Total: __/30

3 things I learned from today's live mock:
1. [Your own takeaway]
2. [Your own takeaway]
3. [Your own takeaway]

1 thing I need to practice more:
→ [Your honest gap — be specific]

8 Wednesdays. 8 mock interviews. Real questions. Live pressure. Honest feedback.
This is what separates CloudDevOpsHub from every other program.

Thank you Vikas Ratnawat and CloudDevOpsHub Community! 🙏

#MultiCloudDevOpswithAI #CloudDevOpsHub #55SessionsChallenge
#MockInterview03 #InterviewPrep #HighPackageJourney #DevOps
```

> 🏷️ **Tag:** Vikas Ratnawat | CloudDevOpsHub Community

---

## ⏭️ Navigation

| | |
|-|-|
| [🏠 Main Roadmap](../README.md) | [➡️ Mock-04: Docker & Kubernetes + GKE](./Mock-04-Docker-Kubernetes-GKE.md) |


---
<div align="center">

**CloudDevOpsHub | Multi Cloud + DevOps with AI | India's Largest Cloud & DevOps Community**

*✅ Mentored by Vikas Ratnawat (PwC | Ex Microsoft MVP) | 58,000+ Members | 10,000+ Students Trained*

🌐 [www.clouddevopshub.com](http://www.clouddevopshub.com) | ⏰ Live: **8:00 AM – 9:15 AM IST** | Q&A: **9:15 – 10:00 AM IST**

</div>

