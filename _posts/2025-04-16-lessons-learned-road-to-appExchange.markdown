---
layout: post
title: "Lessons Learned On the Road to Producing An AppExchange App"
date: 2025-04-30 08:00:00 -0400
author: Tamara Chance
categories: salesforce agentforce AI
image: assets/img/stockImages/road-to-appExchange.svg
---
This post isn’t a technical tutorial or a finished success story. It’s a behind-the-scenes look at the real process of taking an idea, turning it into a product, and preparing to hand it over for Salesforce’s most rigorous review.

While this post won’t discuss the specifics of the app (that’ll come later), this has been such a learning journey that I want to share a few of the lessons I’ve learned so far for anyone else who might be thinking about building a product for the AppExchange.

#### **Where It All Started**

The idea for this app didn’t come from a whiteboard or team brainstorm – it came from the need to fulfill the requirements of our clients. We had seen similar requests from various Salesforce customers and orgs, and the out-of-the-box Salesforce features and similar AppExchange solutions were almost good enough, but just never quite right.

In order to fulfill our client’s requirements, we built something custom given our previous experience with this problem. We realized that this solution was not unique to the client or their business but could be applied to nearly any organization using Service Cloud. 

So, we decided to take this simple, generalizable idea and develop it into a feature-rich solution that anybody could configure for their own org.

#### **Lesson Learned #1:** _When You Don’t Know the Org Your App Will Live In, You Have to Bake in Flexibility, Documentation, and Assumption-Free Logic from the Very Beginning_

The mental shift between building a client solution and a product is bigger than expected. When scoping for the project, I took for granted a lot of the known factors that were a part of building the client solution, such as field names, object access, feature assumptions.

This meant that much of my original scoping requirements had to be reevaluated after the build process began. *Oof.*

**What Scoping Looks Like from a Product Development Perspective:**

- Constant reevaluation of what is MVP vs. what is Nice-to-Have. That line is slippery. MVP is hard to define when you're emotionally invested. Every feature feels important—until it looks like it will delay your launch.
- Rediscovering requirements mid-way through. Some functionality gaps only became obvious as we began to build out features.
- Being intentional about making features and setup intuitive for admins in an org you’ll never see.

#### **Lesson Learned #2:** _Org-Agnostic Development Is a Muscle. At First It’s Painful. Then It Makes You Better_

This is where things got real.

The moment I registered my namespace and saw it prefix everything that I had so far, I felt like I had stepped into a whole new flavor of Salesforce development. I also felt like I had picked a dumb name. Every decision in the beginning of app development has downstream implications. I ended up going back and registering a new namespace that made more sense. Thankfully, before writing 1000 lines of code.

Some things were very familiar, such as the tools that we would be using to build the app:
- Scratch orgs + unlocked packages
- LWC for UI, Apex for backend logic
- Permission-based test paths to mimic different org setups

Some things stood out as needing to be more stringent because of the _unknown_ factor:

- **CRUD/FLS enforcement is not optional.** It’s central to passing review and respecting org integrity.
- **Meaningful test coverage goes beyond 75%.** Tests must prove security guardrails work.
- **Packaging early forces you to build smarter.** It encourages better separation of concerns and discourages hardcoding and reliance on real data.

#### **Lesson Learned #3:** _Security Review Isn’t Just About Code—It’s About Trust. Build Like Someone Else’s Client Data Depends on It. Because It Does_

Even though the app hasn’t been submitted for Security Review yet, it’s been on our mind since day one. And honestly, that mindset has helped shape the app for the better.

In anticipation of Security Review, I added a [GitHub Action to run Salesforce Code Analyzer](https://sfdxdeveloper.com/salesforce/cli/sf/code/analyzer/github/actions/2025/05/07/salesforce-code-analyzer.html) on every PR. It was my first time configuring Actions and working with GitHub workflows like this—but I can already see myself using them more in the future.

How I’m Preparing:

- **Salesforce Code Analyzer in GitHub Actions** to catch things proactively. Plus, the Salesforce Code Analyzer documentation is required as part of the submission.
- **Strict enforcement of CRUD/FLS and documented tests**
- **ApexDoc Documentation annotations** added from the start to ensure the codebase is well documented.
- **Thinking through the demo experience**, which will need to be clean and convincing.

I think this phase will be a bit like preparing for an audit. You know your app works—but then you have to prove that it behaves safely, securely, and predictably under any org configuration.

#### **Final Thoughts – For Anyone Thinking About Their First AppExchange App**

If you’re thinking of building an AppExchange app: *do it*. But go in with your eyes open. You’re not just building an app. You’re building for trust, for scale, and for orgs you’ll never see.

For me, this project has already delivered a career's worth of growth. And it hasn't even been submitted yet. Sometimes it still feels like a moving target, but I’m looking forward to sharing more details about the app after it’s successfully survives Security Review.

For now, I’m taking it one package version, one PR, and one Analyzer scan at a time.
