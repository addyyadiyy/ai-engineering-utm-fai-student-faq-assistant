# **Week 1 Observation Sheet**

## **System name: YouTube Recommendations**


### **Group members:**

- Laila Khadija

- Nur Adina

- Nur Asyura
  
- Nurin Afiqah

  
### **1. What problem does this system solve? Who benefits?**

Problem: There is far more video content on YouTube than any user can browse, so people struggle to find videos they want to watch. The recommendation system filters this into a personalised feed.

Beneficiaries: Viewers, who find relevant content faster (for example, our Korean talk and reality shows). Creators also benefit, because recommendations help new audiences find their videos. YouTube benefits too, since recommendations keep users watching.


### **2. Which AI stack layer is most visible to users?**

Layer: Application (Application / Model / Infrastructure)

Explanation: Users only see the homepage feed, the "Because you watched..." label, and the three-dot menu with "Not interested" and "Don't recommend channel". The ranking and recommendation models, and the infrastructure that collects and processes watch history and serves the feed, are not visible to users.


### **3. Quality dimensions (at least 3, with measurement method)**

- Dimension 1: Fairness Measure: Compare the first top 10 homepage items across group members, counting how many different creators, genres and languages appear and whether each person gets similarly varied, relevant recommendations.

- Dimension 2: Personalisation Measure: Count how many of the first top 10 homepage items match the user's usual interests, and how many items from a new topic appear after an unusual search (before and after comparison).

- Dimension 3: Transparency Measure: Score how well the system explains each recommendation: 0 = no explanation, 1 = vague explanation, 2 = specific explanation naming the cause.


### **4. What did the system do WELL? Give a specific example.**

The homepage personalisation matched the user's viewing habits. It mainly recommended Korean talk shows and reality shows, which are the content the user watches most. It also responded to a new interest. After a search for Japanese cooking videos, one or two cooking videos appeared on the homepage, so the system picked up the new search signal.


### **5. What did it do POORLY? Give a specific example.**

The system was not transparent. Most homepage videos had no explanation of why they were recommended. The one "Because you watched..." label we found did not say which video or action caused it. The three-dot menu lets users give feedback, but it does not explain why a video was shown. We rated transparency 1 out of 2.


### **6. Responsible AI risks (at least 2). Who is harmed and how?**

Risk 1: Lack of transparency and accountability (observed). All users are affected. Because they cannot see why a video was recommended, they cannot check whether the system is fair or correct it when it makes mistakes.

Risk 2: Privacy (hypothesis). The system relies on watch history, search history and interactions to build a profile of each user. Users may not realise how much of their behaviour is used to shape what they see.


### **7. If you could fix ONE thing, what and why?**

Add a specific explanation to each recommended video, for example "Recommended because you watched [video title]", with a direct option to remove that video from the signals used. This directly fixes our weakest finding (transparency).


System map: see docs/week1_system_map.png.
