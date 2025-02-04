---
title: "HackMidwest 2024: How We Built a USDA Chatbot in 24 Hours (and Won!)"
date: 2024-11-04T13:42:32-06:00
draft: false
categories: ["hackathon"]
tags: ["tech", "LLM", "Python", "AWS", "Bedrock", "RAG", "Prompting"]
---

When my friend and I signed up for **HackMidwest 2024**, we had no idea what we were going to build. Zero. Zilch. Nada. We showed up with our laptops, a vague sense of excitement, and a whole lot of caffeine. The hackathon kicked off with some awesome workshops—Red Hat OpenShift, AWS, Pinata, and more. We attended as many as we could, soaking up knowledge and trying to figure out what we could possibly create in 24 hours.

Then came the moment of truth: the challenge announcements. We scanned the list, and one immediately caught our eye—the **AskUSDA Challenge**. It was intriguing, doable, and had a clear problem statement: help USDA provide better information to the public using AI. We looked at each other and said, “Let’s do this.”

---

## The Plan (or Lack Thereof)

We didn’t have a detailed roadmap, but we knew we wanted to build something around **Large Language Models (LLMs)** and **Retrieval-Augmented Generation (RAG)**. The idea was simple: create a chatbot that could answer questions using USDA’s data. But here’s the twist—we wanted to make it flexible enough to work with *any* dataset, not just USDA-specific information.

We started by exploring **Red Hat OpenShift** for deployment, but mid-development, we hit a snag. Deploying a model there was more complex than we anticipated. So, we pivoted to **AWS**. Specifically, we used **AWS Bedrock** and its **Knowledge Base** feature for RAG. It was surprisingly straightforward, and within hours, we had a working prototype.

![Architecture](/images/dchat_arch.png)  
*Architecture of our solution*

---

## The Build

Here’s the breakdown of what we built in 24 hours:

**A Scraper to Rule Them All**: We created a scraper tool called **[No Game No Scrape](https://github.com/rahulmysore23/No-Game-No-Scrape)** (yes, the name is a nerdy anime reference). It can scrape data from any website and format it for our chatbot.

**The Chatbot**: We built **[D-Chat](https://github.com/rahulmysore23/D-Chat)**, a chatbot powered by AWS Bedrock and RAG. It’s designed to ingest scraped data, sync it with the knowledge base, and provide accurate, context-aware answers. The best part? It’s not USDA-specific. You can use it for any dataset.

**The Prompt**: We nailed down a simple yet effective prompt that guided the chatbot to provide concise, accurate answers. It was a game-changer.

---

## The USDA Team and the Presentation

Midway through the hackathon, we hit a few roadblocks. Thankfully, the USDA team was there to help. They answered our questions, clarified the requirements, and even gave us some insights into how their data is structured. It was a huge help.

When it came time to present, every team got just **2 minutes** to showcase their solution. We kept it simple: we explained the problem, demonstrated the chatbot, and highlighted its flexibility. The judges were impressed, and by the end of the hackathon, we found out we’d **won the USDA Challenge**!

![Demo](/images/dchat_demo.jpeg)  
*Demo of our chatbot in action*

---

## Lessons Learned

1. **Pivot When Necessary**: We started with OpenShift but switched to AWS when things got too complicated. Flexibility is key in hackathons.
2. **Keep It Simple**: Our scraper and chatbot were built with simplicity in mind. No over-engineering, just a focus on solving the problem.
3. **Ask for Help**: The USDA team was incredibly supportive. Don’t hesitate to reach out to mentors or challenge sponsors.

---

## Final Thoughts

HackMidwest 2024 was an incredible experience. We went in with no plan, built something we’re proud of, and even won a challenge. If you’re curious about our project, check out the links below:

- **[No-Game-No-Scrape](https://github.com/rahulmysore23/No-Game-No-Scrape)**
- **[D-Chat](https://github.com/rahulmysore23/D-Chat)**
- **[My LinkedIn Post](https://www.linkedin.com/posts/rahulmysore23_hackmidwest2024-usdachallenge-agriculturaltech-activity-7248075478459113472-f4G7/?utm_source=share&utm_medium=member_desktop)**

And if you’re thinking about participating in a hackathon, just go for it. You never know what you’ll end up building—or winning. 😊
