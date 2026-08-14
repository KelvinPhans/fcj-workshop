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

As a student responsible for the **React frontend** and **Hugo workshop documentation** of the Startups Blogs project, I attended the event to observe how teams transformed ideas into working AI products within approximately 48 hours. I also focused on how they communicated user flows, presented AWS architectures, and identified the additional requirements needed to move a Proof of Concept (PoC) toward production.

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

{{< blog-image src="images/events/agentic-ai-online-ordering-architecture.jpg" alt="AWS architecture presented by the Agentic AI online ordering team" caption="Figure 1. AWS architecture presented by the Agentic AI online ordering team." >}}

---

### B. Agentic AI for Data Analysis

- **Problem Statement:** Data Analysts spend significant time generating repetitive reports and conducting manual data manipulation.
- **PoC Solution:**
  - Receiving analytical requests via natural language inputs.
  - Automatically generating initial data analysis reports.
  - Incorporating an **Agent Loop** to refine outputs based on analyst feedback.
  - Applying **Guardrails** to validate data accuracy and output formatting.
- **Key Takeaway:** Demonstrated effective human-in-the-loop collaboration between analysts and AI Agents.

{{< blog-image src="images/events/agentic-ai-data-analysis-architecture.jpg" alt="AWS architecture presented by the Agentic AI data analysis team" caption="Figure 2. AWS architecture presented by the Agentic AI data analysis team." >}}

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

{{< blog-image src="images/events/agentic-ai-traffic-tracking-architecture.jpg" alt="AWS architecture presented by the passenger traffic tracking team" caption="Figure 3. AWS architecture presented by the passenger traffic tracking team." >}}

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

This was my first opportunity to closely observe a large-scale Hackathon centered on **Agentic AI**. From a frontend perspective, I paid particular attention to how each team translated a complex agent workflow into an interface that users could understand and operate within a short demonstration.

The event also changed how I approach technical documentation. A convincing presentation must connect the user problem, application flow, AWS architecture, security controls, operational risks, and measurable outcome rather than showing a diagram in isolation. I plan to apply this lesson when refining the Startups Blogs interface and writing the step-by-step Hugo workshop.

---

## Lessons Learned

- Start with the user's actual workflow before selecting an AI model or AWS service.
- Design frontend states that make long-running agent actions, errors, confirmations, and human approval points understandable.
- Document the relationship between each interface action, backend API, and AWS service instead of presenting disconnected implementation steps.
- Treat RBAC, data protection, observability, and cost controls as production requirements from the beginning.
- Clearly distinguish implemented Startups Blogs features from future Agentic AI research ideas.

<!-- Event 2 personal photos will be added later. -->

---

## Conclusion

The **FCAJ x Agentic AI Build Week** delivered modern technical perspectives on Agentic AI and AWS cloud architecture design. The key takeaways regarding PoC versus production standards, Guardrails implementation, RBAC integration, and cost optimization provide a solid knowledge foundation for my future software engineering and cloud development endeavors.
