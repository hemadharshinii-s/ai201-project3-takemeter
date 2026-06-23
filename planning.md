# TakeMeter Planning Document

### Community

For this project, I chose the Genshin Impact community, focusing primarily on public posts from the Genshin Impact subreddit and other public Genshin discussion spaces. I selected this community because it contains a broad range of discourse styles rather than only one type of conversation. Players frequently discuss game mechanics, character builds, banner decisions, lore, and personal gameplay experiences, creating a mixture of technical analysis, subjective opinions, and emotional reactions. 

This diversity makes Genshin Impact a strong candidate for a text classification task. The distinctions between reasoning-based posts and emotionally driven posts are meaningful to community members and occur often enough that a classifier should be able to learn them from a relatively small labeled dataset. 

--- 

### Label Taxonomy

#### **Label 1: Analysis**

***Definition:*** A post belongs to **Analysis** if its primary purpose is to explain, reason about, or seek information regarding gameplay mechanics, optimization, strategy, builds, or other evidence-based aspects of Genshin Impact. 

***Example Posts:***

1. > I’ve been out of Genshin since around the start of Fontaine, and coming back now the power creep feels kinda crazy 😭 Can anyone help me put together two decent teams that I can use for now while I catch up and work on getting/building better characters?

   **Why?** Even though it's phrased as a question, the primary purpose is to seek gameplay advice and team-building guidance, making it an analysis-oriented post. 
2. > Every time you get a 5 star artifact these are the possible values each sub stat will have and that they can obtain at every 4 levels. When upgrading artifacts there's RNG choosing which of the 4 sub stats to upgrade, and then another RNG to pick which of those 4 possible values will be added to that sub stat.

   **Why?** This is an explanation of game mechanics based on factual information and reasoning.

***Boundary Rule:*** Questions that ask for gameplay advice or optimization are classified as **Analysis,** since their primary intent is understanding mechanics rather than expressing an opinion. 

#### **Label 2: Hot Take**

***Definition:*** A post belongs to **Hot Take** if its primary purpose is to make an evaluative or judgmental claim about a character, weapon, banner, or aspect of the game, especially when the emphasis is on opinion rather than detailed reasoning. 

***Example Posts:***

1. > Kazuha Kazuha is pretty overrated. idk what it is about him, but everybody praises him and wants him so badly. They completely ignore other good anemo characters like venti, sucrose & basically all other anemo characters.

   **Why?** This is fundamentally an opinion evaluating a character and community perception rather than presenting analysis. 
2. > Genshin has not been as fun as of late the quests are just walk here dialogue with choices that mean nothing walk here do the same thing random small fight walk here done and then when you do get a big fight it with acts like you were struggling or you have to wait for the dialogue to end to actually continue fighting then there's the quests that force you to use other characters that suck at that specific boss not to mention the amount of mini games in levels that only show up once and never again 90%bof their new five stars are female and you rarely get male characters even as four stars and not to mention the custom character situation where you have to pay for good things with real money with no way of getting them any other way anyway...

   **Why?** Although the author gives reasons, the overall goal is to argue that the game has decline and express a judgment about its quality. 

***Boundary Rule:*** If a post contains both an opinion and some supporting evidence, it will still be labeled **Hot Take** when the main focus is persuading readers of the author's judgment rather than explaining mechanics. 

#### **Label 3: Reaction**

***Definition:*** A post belongs to **Reaction** if its primary purpose is to express an emotional response or personal experience related to gameplay, story events, or the gacha system rather than to analyze or debate. 

***Example Posts:***

1. > I just lost the 50/50 with Lauma. This is the 7th 50/50 I've lost out of the last 8. I'm F2P, so it's incredible. Is there anyone who has the same or worse luck in the world with this game, or is there someone who can understand me? I hate the 50/50. I don't know where the 50/50 chance is, but it must be imaginary because I'm at 50/400.

   **Why?** The post is mainly venting frustration and sharing a personal experience.
2. > Hoping to get varka. By end of his half I’ll be able to reach pity once, but it’s a 50/50 :/

   **Why?** This is an emotional expression of anticipation and anxiety rather than analysis or debate.

***Boundary Rule:*** Posts centered on frustration, excitement, celebration, disappointment, or storytelling about personal experiences are classified as **Reaction,** even if they contain minor factual details. 

--- 

### Hard Edge Cases

The most difficult cases are posts that combine multiple discourse styles. 

For example:

> "Is Hu Tao really worth using without Staff of Homa? Everyone says she's amazing, but mine feels weak."

The post could reasonably be interpreted as: 

- **Analysis,** because it asks about gameplay performance and optimization. 
- **Hot Take,** because it implicitly questions the character's strength and community perception.

**Final Decision:** I would label this example as **Analysis** because the primary purpose is to seek gameplay guidance about Hu Tao's performance and equipment rather than to persuade readers that she is good or bad. Although it contains a personal opinion, the central intent is obtaining information and advice. 

To ensure consistency during annotation, I will classify posts according to their **primary communicative intent:**

- If the author is mainly requesting or providing gameplay reasoning, I will label the post **Analysis.**
- If the author is mainly evaluating or judging whether something is good, bad, overrated, or worth using, I will label it **Hot Take.**
- If the author is primarily expressing emotion or describing a personal experience, I will label it **Reaction.**

Questions will **not** receive their own label. Instead, they too will be assigned based on intent using the above rules. 

---

### Data Collection Plan

I will collect at least **200 public text posts or comments** from the Genshin Impact community, primarily from the main subreddit (r/Genshin_Impact) and, if necessary, additional public Genshin-related communities to improve balance. 

My target distribution is approximately:

- Analysis: 65-70 examples
- Hot Take: 65-70 examples
- Reaction: 60-70 examples

If one category becomes underrepresented after collecting 200 examples, I will intentionally search for additional posts matching that category using relevant keywords and continue collecting until the distribution is more balanced. I will also verify that no single label represents more than 70% of the dataset. 

The final dataset will be stored as a CSV containing at least: 

- `text`
- `label`

--- 

### Evaluation Metrics

I will evaluate the classifier using several complementary metrics. 

- **Overall accuracy** measures the percentage of correctly classified examples and provides a high-level summary of performance.
- **Per-class precision, recall, and F1-score** measure how well the model performs on each individual label. These metrics are important because some discourse styles may be easier to identify than others. 
- **Confusion matrix** analysis will show which categories the model most frequently confuses with one another, helping identify weaknesses in the label boundaries or training data. 

Accuracy alone is insufficient because a model could achieve deceptively high performance by overpredicting one common category. Examining per-class metrics and confusion patterns provides a more complete evaluation.

--- 


### Definition of Success

I would consider this classifier successful if it satisfies the following conditions:

- Overall test accuracy of **at least 75%.**
- **F1-score of at least 0.70 for each label,** demonstrating that performance is reasonably balanced across categories.
- No category is consistently ignored or overwhelmingly overpredicted. 
- Most remaining errors occur in genuinely ambiguous edge cases rather than in clearly defined examples. 

At that level of performance, I believe the classifier would be useful as a lightweight tool for automatically characterizing the style of discussion occurring within the Genshin Impact community.

--- 

### AI Tool Plan

#### **Label Stress-Testing**

Before beginning large-scale annotation, I will use an AI assistant (likely either ChatGPT or Claude) to generate several borderline examples that sit between my labels (particularly **Analysis** vs. **Hot Take**). If those examples are difficult to classify consistently, I will revise my definitions or decision rules before collecting the full dataset. 

#### **Annotation Assistance**

I currently plan to perform the labeling manually. However, I may experiment with using an LLM to propose preliminary labels for a small batch of posts as a speed aid. If I did so, every proposed label will be manually reviewed and corrected (as needed) before inclusion in the dataset, and this assistance will be disclosed in the project's AI usage section.

#### **Failure Analysis**

After evaluating the trained model, I plan to use an AI assistant to review misclassified examples and suggest possible error patterns, such as confusion between Analysis and Hot Take or difficulties with emotionally phrased questions. Any suggested patterns will be verified by manually examining the underlying examples before being included in the final evaluation report.

--- 

### Expected Challenges

The biggest challenge will likely be distinguishing between posts that contain both gameplay reasoning and strong opinions. Many Genshin discussions blend factual analysis with subjective evaluation, making the boundary between **Analysis** and **Hot Take** intentionally difficult. Establishing and consistently applying the decision rules above should reduce annotation inconsistency and produce a clearer training signal for the model. 