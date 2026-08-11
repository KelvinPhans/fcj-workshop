---
title: "Event 2 - Agentic AI Build Week"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Event 2 - FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!

## Event Overview

**FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!** was a solution showcase and reporting session for the Hackathon organized by the **AWS First Cloud AI Journey (FCAJ)** community in collaboration with **JI Fund**.

The event brought together students, software engineers, cloud professionals, and AI practitioners. Participating Hackathon teams demonstrated their live products, presented cloud architecture diagrams, shared practical implementation strategies, and discussed engineering challenges in building AI-powered applications.

My objective in attending the event was to learn how teams developed AI solutions within a constrained timeframe (~48 hours) and to study how a Proof of Concept (PoC) evolves into a production-ready system hosted on AWS.

---

## Event Objectives

- Learn fundamental Agentic AI concepts and practical software applications.
- Understand the evolution process from a PoC prototype to a scalable production solution.
- Gain real-world engineering insights from participating Hackathon teams.
- Understand production readiness criteria for enterprise AI applications.
- Observe how various AWS cloud services are combined into end-to-end AI system architectures.

---

## Featured Speakers

- **Mr. Nguyen Gia Hung** – Head of Solution Architect, AWS Vietnam | Founder of AWS First Cloud AI Journey (FCAJ).
- **Mr. Joseph Marazota** – Head of Technology, Amazon ASEAN.
- Participating Hackathon Teams.

---

## Main Event Content

The event focused heavily on **Agentic AI**. Unlike conventional Generative AI applications that primarily respond to direct user prompts, an Agentic AI system can:
- Plan multi-step tasks (Task planning).
- Dynamically invoke external tools and APIs (Tool calling).
- Execute complex multi-step workflows.
- Evaluate intermediate outputs and self-correct actions.
- Interact autonomously with other software components.

Beyond AI capabilities, Hackathon solutions were evaluated against production-grade criteria:
- **Guardrails**: Controlling and restricting AI model behaviors and output boundaries.
- **Role-Based Access Control (RBAC)**: Enforcing strict data access permissions.
- **Human-in-the-loop**: Incorporating human verification at critical decision checkpoints.
- **API Cost Optimization**: Managing AI model token budgets and operational costs.
- **Scalability, Security & Reliability**: Ensuring system robustness, data protection, and high availability.

Participating teams developed their solutions within an intensive **48-hour** development window, presenting their problem statements, solution concepts, AWS architectures, implementation challenges, and live product demonstrations.

---

## Selected Team Presentations

### A. Agentic AI for Online Ordering Systems

- **Problem Statement:** Traditional online ordering platforms require users to navigate multi-step manual processes: account creation, payment detail entry, browsing multiple menus, and manual cart assembly.
- **PoC Solution:** Built an AI Agent supporting conversational food ordering via natural language.
- **Reported Capabilities:**
  - Automated restaurant menu ingestion from official websites.
  - Storing structured menu data on AWS cloud infrastructure.
  - Maintaining order history and user preference memory.
  - Automatically generating orders and adding items to the cart through conversational workflows.
- **Key Takeaway:** Demonstrated how an AI Agent can execute user tasks directly rather than returning text responses.

![Figure 1. AWS architecture presented by the Agentic AI online ordering team.](/images/events/agentic-ai-online-ordering-architecture.jpg)
*Figure 1. AWS architecture presented by the Agentic AI online ordering team.*

---

### B. Agentic AI for Data Analysis

- **Problem Statement:** Data Analysts spend significant time generating repetitive reports and conducting manual data manipulation.
- **PoC Solution:**
  - Receiving analytical requests via natural language inputs.
  - Automatically generating initial data analysis reports.
  - Incorporating an **Agent Loop** to refine outputs based on analyst feedback.
  - Applying **Guardrails** to validate data accuracy and output formatting.
- **Key Takeaway:** Demonstrated effective human-in-the-loop collaboration between analysts and AI Agents.

![Figure 2. AWS architecture presented by the Agentic AI data analysis team.](/images/events/agentic-ai-data-analysis-architecture.jpg)
*Figure 2. AWS architecture presented by the Agentic AI data analysis team.*

---

### C. Agentic AI for Passenger Traffic Tracking

- **AWS Services Utilized in Team Presentation:**
  Amazon Kinesis Video Streams, Amazon ECS, Amazon ECR, Amazon SageMaker Endpoints, Amazon S3, Amazon DynamoDB, Amazon CloudFront, Amazon API Gateway, AWS Lambda, AgentCore Runtime, Amazon Bedrock, Amazon Cognito, AWS IAM, AWS Secrets Manager, AWS CloudTrail, Amazon CloudWatch.
- **Solution Overview:**
  - Video and image streams ingested via Amazon Kinesis Video Streams.
  - Frame processing components (ECS/SageMaker) analyzing frames to calculate passenger metrics.
  - Metrics and analytical results securely persisted on S3 and DynamoDB.
  - API Gateway and AWS Lambda serving analytical endpoints to Frontend apps via CloudFront CDN.
  - Integrated security (Cognito, IAM, Secrets Manager) and observability (CloudWatch, CloudTrail).
  - Agentic AI components (Bedrock / AgentCore) enabling intelligent queries over processed traffic metrics.

*(Note: The above AWS architecture belongs to the presentation delivered by a Hackathon team and does not represent the Startups Blogs architecture).*

![Figure 3. AWS architecture presented by the passenger traffic tracking team.](/images/events/agentic-ai-traffic-tracking-architecture.jpg)
*Figure 3. AWS architecture presented by the passenger traffic tracking team.*

---

## Knowledge Gained

- **Core Nature of Agentic AI:** Effective AI Agents require planning, execution, tool integration, and output evaluation rather than relying solely on single prompts.
- **Importance of Guardrails:** Establishing guardrails is essential for constraining model behavior and maintaining output quality.
- **PoC vs Production Gap:** PoC prototypes and production systems differ significantly in security standards, scalability, reliability, observability, and operational cost.
- **Solving Real User Problems:** AI technologies deliver genuine value only when simplifying real-world user workflows.
- **AWS Cloud Integration:** Practical patterns for combining AWS cloud services into end-to-end AI solution architectures.

---

## Potential Future Applications to Startups Blogs (Future Possibilities)

*(Note: The following represent potential future research directions and architectural possibilities learned from the event. The current Startups Blogs platform does NOT implement Agentic AI, Amazon Bedrock, or SageMaker).*

- **Intelligent Search:** Exploring natural language search assistance for investors looking across public business profiles and funding opportunities.
- **Content Description Assistance:** Conceptualizing AI-assisted drafting tools to help business owners structure funding campaign details.
- **Public Data Summarization:** Helping investors summarize public business information and funding opportunity highlights.
- **Moderation Support:** Researching automated content moderation assistance tools for platform administrators.
- **Enforcing Guardrails & RBAC:** Ensuring future AI capabilities adhere to strict role-based authorization so AI features cannot access unauthorized private data.

---

## Personal Experience

This event provided my first opportunity to closely observe a large-scale Hackathon centered on **Agentic AI**. I was impressed not only by the creative product demonstrations but also by the rigorous discussions surrounding production readiness.

The experience highlighted that building AI applications involves much more than selecting a foundational LLM model; security, authorization, reliability, observability, and cost management are the critical factors determining real-world success. Interacting with students, software engineers, and AWS specialists provided invaluable technical perspectives and career insights.

---

## Lessons Learned

- Always start with real user problems before selecting technologies.
- Balance functional features, output accuracy, security, and operational costs.
- Design authorization (RBAC) and data protection from the ground up when AI accesses application data.
- Clearly differentiate design standards between PoC prototypes and production architectures.
- Active participation in technical communities and Hackathons serves as an effective learning channel.

<!-- Event 2 personal photos will be added later. -->

---

## Conclusion

The **FCAJ x Agentic AI Build Week** delivered modern technical perspectives on Agentic AI and AWS cloud architecture design. The key takeaways regarding PoC versus production standards, Guardrails implementation, RBAC integration, and cost optimization provide a solid knowledge foundation for my future software engineering and cloud development endeavors.
