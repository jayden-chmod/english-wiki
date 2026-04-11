# Session 001 — First Day at Motics
**Date:** 2026-04-04
**Time:** 1:40 PM – 6:32 PM (BST)
**Topic:** Onboarding, development environment, Audit Agent architecture

---

[S-001] **Salinna:** Hey! You joining us for the team lunch today? We're heading to that ramen place around the corner.

[U-001] **User:** Yeah, good to see you here. I'm working on getting used to development environment.

[F-001] **Feedback:**
- "getting used to development environment" → missing article "the"
- Overall: very natural, just one article slip.

[C-001] **Corrected:** "Yeah, good to see you here. I'm working on getting used to **the** development environment."

---

[S-002] **Salinna:** Oh nice, yeah the setup can take a bit. Have you managed to get everything running locally yet? We use Docker for most of it, so hopefully it wasn't too painful.

[U-002] **User:** I've also prefered Docker for my projects, so I'm used to it.

[F-002] **Feedback:**
- "prefered" → "preferred" (spelling: double r)
- "I've also preferred Docker" → slightly awkward; more natural: "I've used Docker in my projects before"

[C-002] **Corrected:** "I've used Docker in my projects before, so I'm used to it."

---

[S-003] **Salinna:** Oh nice, that makes things way easier then! Some people struggle with the local setup the first day. Which part of the stack are you looking to focus on first — frontend or backend?

[U-003] **User:** Well, backend. I think I can introduce ontology validation on the audit agent.

[F-003] **Feedback:**
- "introduce ontology validation on the audit agent" → "on" → "into" or "to" (preposition)
- Overall: solid sentence, one preposition tweak.

[C-003] **Corrected:** "Well, backend. I think I can introduce ontology validation **into** the audit agent."

---

[S-004] **Salinna:** Oh, interesting! What kind of ontology are you thinking — like SNOMED CT or something FHIR-based? The audit agent currently just checks structure, so adding semantic validation would be a big step up.

[U-004] **User:** Do you have any recommandation? I was about to research on which one to focus.

[F-004] **Feedback:**
- "recommandation" → "recommendation" (spelling)
- "research on which one to focus" → "research which one to focus on" / "look into which one to use" ("research on" unnatural; "to focus" needs "on" at end)

[C-004] **Corrected:** "Do you have any recommendations? I was about to look into which one to use."

---

[S-005] **Salinna:** Honestly, I'd start with SNOMED CT — it's the most widely used in UK clinical settings, and our clinic partners are already familiar with it. FHIR is great but it's more about data exchange than validation. That said, worth having a quick look at what NHS Digital recommends too.

[U-005] **User:** Thank you. Okay, I'll try SNOMED CT. I'll show it when prototyping is completed.

[F-005] **Feedback:**
- "show it" → "show you" (who are you showing it to?)
- "prototyping is completed" → "the prototype is ready/done" (more natural in casual speech)

[C-005] **Corrected:** "Thank you. Okay, I'll try SNOMED CT. I'll show you when the prototype is ready."

---

[S-006] **Salinna:** Sounds good! No rush — take your time to explore it first. If you hit any questions about how the audit agent is structured, just ping me on Slack.

[U-006] **User:** I imagined this flow. When the agent extracts and structures medical facts from clinician's records(?). Give it Ontology as context to make sure output consistency. And if there is no exact disease name, then we search on ontology and find the right name. We can also check what action should be done. When it is near the deadline or hasn't done yet, the agent informs that to related stakeholders. Periodically, we can make reports about which part needs improvement to pass the audit.

[F-006] **Feedback:**
- "clinician's records" → "clinical records" (plural possessive / more natural term)
- "Give it Ontology as context" → "We feed it the ontology as context" (needs subject; no caps on ontology)
- "make sure output consistency" → "ensure output consistency"
- "search on ontology" → "search the ontology" ("on" unneeded)
- "hasn't done yet" → "hasn't been done yet" (passive voice needed)
- "informs that to related stakeholders" → "notifies the relevant stakeholders" (unnatural phrasing)
- "make reports" → "generate reports" (more technical/natural)

[C-006] **Corrected:** "When the agent extracts and structures medical facts from clinical records, we feed it the ontology as context to ensure output consistency. If there's no exact disease name, we search the ontology to find the right match. We can also track what actions need to be taken — if a task is approaching its deadline or hasn't been done yet, the agent notifies the relevant stakeholders. And periodically, we can generate reports on which areas need improvement to pass the audit."

---

[S-007] **Salinna:** This is actually a really solid flow. The ontology-grounding step is exactly what the audit agent is missing right now — it flags issues but doesn't always catch when the wrong terminology is used. The deadline-tracking piece is interesting too, that's almost like a task management layer on top. Are you thinking this all stays within the audit agent, or would it need to hook into the other agents like Scribe?

[U-007] **User:** Yes, eventually. we need to integrate the agents. The scribe agent and audit agent can be grouped because it is closely related to medical records and actions. On the other hand, the phone agent and email agent are related to admin works. Umm, I'm not sure where billimg agent should be included. It's litte bit related to medical information but not entirely.

[F-007] **Feedback:**
- "Yes, eventually. we" → "we" should be capitalised after a period
- "it is closely related" → "they are closely related" (plural subject: two agents)
- "admin works" → "admin tasks" / "administrative work" ("works" incorrect here)
- "billimg" → "billing" (typo)
- "where billing agent should be included" → "where the billing agent fits" (article + natural verb)
- "litte bit" → "a little bit" (typo + missing article)

[C-007] **Corrected:** "Yes, eventually we need to integrate the agents. The Scribe and Audit agents could be grouped together since they're closely related to medical records and documentation. On the other hand, the Phone and Email agents are more about admin tasks. Hmm, I'm not sure where the Billing agent fits — it's a little bit related to medical information, but not entirely."

---

[S-008] **Salinna:** That's a fair point about Billing. The way I think about it — Billing needs the clinical data to generate accurate claims, like diagnosis codes and procedure codes. But the output is financial, so it's kind of a bridge between the two. Maybe it makes sense as its own layer, or just loosely coupled to both? What would you call those groupings architecturally?

[U-008] **User:** I think we should separate core medical records and information and make an independatnt layer, so that every agent can access to them.

[F-008] **Feedback:**
- "independatnt" → "independent" (typo)
- "access to them" → "access them" ("access" as verb doesn't need "to")
- "core medical records and information" → slightly redundant; "core clinical data" is cleaner

[C-008] **Corrected:** "I think we should separate the core clinical data into an independent layer so that every agent can access them."

---

[S-009] **Salinna:** Yes, exactly — that's essentially a shared data layer, like a single source of truth. We actually already integrate with EHR systems, so the data's coming in from there. The question is how we structure it internally. Are you thinking something like a dedicated service, or more of a shared database schema that all agents query?

[U-009] **User:** Well, I think we should develop a separated service and every agent access it via endpoints managed by it. If every agent has a direct access and modify them freely, i may cause a problem.

[F-009] **Feedback:**
- "a separated service" → "a separate service" ("separated" implies was once joined; "separate" is the adjective)
- "every agent access it" → "every agent accesses it" (3rd person singular)
- "has a direct access" → "has direct access" (no article before "direct access")
- "i may cause a problem" → "it could cause problems" (typo "i"→"it"; plural more natural)

[C-009] **Corrected:** "Well, I think we should develop a separate service and have every agent access it through its own endpoints. If every agent has direct access and can freely modify the data, it could cause problems."

---

[S-010] **Salinna:** Agreed — that's basically a microservices pattern with a dedicated clinical data service. It gives us proper access control and a single place to enforce data validation rules. One thing to think about though: if every agent is hitting that service, latency becomes a concern, especially for the Phone Agent which needs to respond in real time. How would you handle that?

[U-010] **User:** I don't think the phone agent need access to it. They're addressing things like reservation, canceling, etc.

[F-010] **Feedback:**
- "the phone agent need" → "the phone agent needs" (3rd person singular)
- "They're addressing" → "It handles" (pronoun consistency; "handle" more natural than "address")
- "reservation" → "reservations" (plural)
- "etc." → "and so on" (more natural in spoken English)

[C-010] **Corrected:** "I don't think the phone agent needs access to it. It handles things like reservations, cancellations, and so on."

---

[S-011] **Salinna:** That's a fair point for the core clinical data — it definitely doesn't need medical records. But it might still need some light patient data, like whether someone already has a booking or their basic contact info. So maybe it has read-only access to a small subset, not the full layer. Does that change your design at all?

[U-011] **User:** That's possible solution. We can make an another account that only has read-only acessibility.

[F-011] **Feedback:**
- "That's possible solution" → "That's a possible solution" (article "a" missing)
- "an another account" → "another account" ("another" = "an + other"; "an" is redundant)
- "read-only acessibility" → "read-only access" ("accessibility" = disability UX term; "access" is correct; also typo)

[C-011] **Corrected:** "That's a possible solution. We can create another account that only has read-only access."

---

[S-012] **Salinna:** Yeah, or more precisely a service account with a restricted role — like an IAM role on GCP with only read permissions on specific collections. That way it's enforced at the infrastructure level, not just by convention. Have you worked with GCP IAM before?

[U-012] **User:** Just personal use. But I think I can develop it by reading docs.

[F-012] **Feedback:**
- "Just personal use." → sentence fragment; "Just for personal projects." is complete
- "develop it by reading docs" → "pick it up by reading the docs" ("develop" unnatural here; "pick it up" = learn a skill; "the docs" with article)

[C-012] **Corrected:** "Just for personal projects. But I think I can pick it up by reading the docs."

---

[S-013] **Salinna:** That's fine — GCP docs are actually pretty good. And honestly for IAM it's not too complex once you grasp the roles and permissions model. I can share some internal examples too, so you don't have to start from scratch. Welcome to the team, by the way — this has been a great first conversation!

[U-013] **User:** Thanks. Me, too. Are you guys having lunch together?

[F-013] **Feedback:**
- "Me, too." — natural and casual, no issue.
- "Are you guys having lunch together?" — perfectly natural.
- No corrections needed.

[C-013] **Corrected:** No correction needed.

---

[S-014] **Salinna:** Yeah! We usually grab something nearby on Fridays. A few of us are heading to the Thai place down the road — want to join? It's a good chance to meet the rest of the team.

[U-014] **User:** That would be nice? When do you leave?

[F-014] **Feedback:**
- "That would be nice?" → "?" makes it sound uncertain/odd; use "!" for acceptance
- "That would be nice" → "That sounds great" is more natural

[C-014] **Corrected:** "That sounds great! When do you leave?"

---

[S-015] **Salinna:** In about 20 minutes — around 12:30. I'll swing by your desk when we're heading out. Do you have any dietary restrictions I should give the team a heads up about?

[U-015] **User:** No, I don't have any~ Then, see you soon!

[F-015] **Feedback:**
- "any~" → drop the "~" (Korean texting habit; not used in English)
- "Then, see you soon!" — natural and friendly, no issue.

[C-015] **Corrected:** "No, I don't have any. See you soon!"

---

[S-016] **Salinna:** Perfect! See you in a bit.
