---
layout: post
title: "Why Contacts Should Not Have Record Types in Salesforce NPSP and Nonprofit Cloud"
date: 2026-06-10 08:00:00 -0400
author: Jon Duelfer
authorTitle: Founder & Salesforce Consultant
authorImage: /assets/img/jon-profile.jpg
categories: salesforce nonprofit npsp
image: /assets/img/postImages/contacts-no-record-types.png
snippet: /assets/img/postImages/contacts-no-record-types-snippet.png
tag: Salesforce Nonprofit
---
If you've spent time configuring Salesforce for a nonprofit, you've likely encountered the temptation to add record types to the Contact object. It seems intuitive at first: you have donors, volunteers, program participants, board members — shouldn't each of these have their own record type?

The short answer is no. And once you understand why, you'll see that record types on Contacts are one of the most common — and most limiting — configuration mistakes nonprofits make in NPSP and Nonprofit Cloud.

#### **The Problem with Categorizing People**

Record types are powerful tools. They let you present different page layouts, control picklist values, and segment your data. For many objects in Salesforce, they make perfect sense.

But record types carry an implicit assumption: that a record belongs to one category. A case is either an open service request or a closed one. An opportunity is either a grant or a major gift. Those are meaningful, stable distinctions.

People are not that simple.

#### **Your Contacts Wear Many Hats**

Think about the constituents in your database. How many of them fit neatly into a single box?

- Maria volunteers every weekend **and** donates at the end of the year.
- James attended your job training program three years ago, found employment, and now sits on your advisory board.
- Dr. Chen is a major gift donor, a corporate sponsor through her company, **and** a program mentor.

If you've assigned each of these contacts a record type — *Donor*, *Volunteer*, *Program Participant* — you've already lost the plot. The moment Maria writes her first check, what do you do? Change her record type from *Volunteer* to *Donor* and lose the volunteer context? Create a hybrid *Volunteer-Donor* type? Neither answer is good.

#### **Record Types Lock You Into a Single Identity**

The fundamental issue is that **record types are mutually exclusive**. A Contact can only have one at a time. That works when the thing being categorized has a single, stable identity. It doesn't work when you're tracking the evolving, multidimensional relationship between a person and your organization.

Forcing a Contact into one record type means:

- **You lose relationship history.** Changing a record type doesn't record why it changed or what the person was before.
- **You create gaps in reporting.** Segmenting by record type gives you a false picture — your "donors" report excludes volunteers who also give.
- **You introduce data friction.** Staff debate which type a new contact should be, or avoid updating it because they don't want to overwrite someone's categorization.
- **You constrain automation.** Flows and automations built around record types break down as contacts' roles evolve.

#### **What to Do Instead**

NPSP and Nonprofit Cloud are built around the idea that a Contact is a person first — and that person's relationships, roles, and engagements are tracked through related records, not through a label stamped on the Contact itself.

Here's how to model constituent roles without record types:

**Affiliations and Relationships** capture how a contact is connected to organizations and other people over time. NPSP's Affiliation object and Nonprofit Cloud's Party Relationship model are purpose-built for this.

**Program Enrollment and Engagement** in Nonprofit Cloud lets you track exactly which programs a contact participates in, at what level, and over what timeframe — without reducing them to a single category.

**Opportunity and Campaign history** tells the donor story. Whether someone gave once or a hundred times is answered by their related Opportunities and Campaign Member records, not a field on the Contact.

**Custom fields and checkboxes** — like `Is_Volunteer__c` or `Is_Board_Member__c` — are a lighter-weight approach when you truly need to flag a Contact's roles for page layout logic or list views. Unlike record types, they compose: a contact can be a volunteer *and* a board member *and* a donor simultaneously.

#### **When Record Types on Contacts Might Make Sense**

There are narrow exceptions. Some organizations use Contact record types to distinguish fundamentally different *types of people* in their system — for example, separating individual constituents from staff users who are also in the Contact object for internal relationship tracking. If your use case genuinely requires different field sets or picklist values for categorically different populations that will never overlap, record types can be justified.

But "this person is a donor" is not a categorically different population. It's a role. And roles change.

#### **The Guiding Principle**

When you're tempted to add a Contact record type, ask yourself: **am I describing what kind of person this is, or what this person does with us?**

If it's the latter — and it almost always is — reach for the relationship and engagement data model that NPSP and Nonprofit Cloud were designed to support. Your constituents are more than a single label, and your CRM should reflect that.

---

*Have questions about data modeling in Salesforce NPSP or Nonprofit Cloud? [Reach out to us](/contact) — we'd love to help you build a system that grows with your organization.*
