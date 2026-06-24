# Project 3: Takemeter

### Overview

TakeMeter is a text classification system that identifies the primary discourse style of posts within the Genshin Impact community. Using a manually annotated dataset of public Genshin Impact discussions, the project classifies posts into one of the three categories:

- Analysis
- Hot Take
- Reaction

The goal is not to determine what a post is about, but rather how the author is communicating. Many Genshin Impact discussions blend gameplay advice, opinions, and emotional experiences, making discourse-style classification a challenging natural langauge process task. 

This project compares a fine-tuned DistilBERT classifier against a zero-shot large language model baseline to evaluate how well supervised fine-tuning performs on a relatively small community-specific dataset. 

---

### Community Choice and Reasoning

For this project, I selected the **Genshin Impact community,** primarily using public posts and comments from Genshin Impact discussion spaces such as Reddit.

I chose this community because it contains a wide variety of discourse styles. Players frequently discuss:

- Team building
- Character optimization
- Game mechanics
- Banner decisions
- Story reactions
- Personal gacha experiences
- Opinions about game design

Unlike communities where most posts follow a single pattern, Genshin discussions naturally mix reasoning, judgment, and emotional expression. This made it an ideal environment for a discourse-classification task.

The distinction between gameplay analysis, opinion-driven hot takes, and emotional reactions is meaningful to community members and occurs frequently enough to support supervised machine learning.

---

### Label Taxonomy

#### **Label 1: Analysis**

**Definition:** A post is labeled **Analysis** when its primary purpose is to explain, reason about, or seek information regarding gameplay mechanics, optimization, strategy, character builds, or other evidence-based aspects of Genshin Impact.

**Example 1:**

> I’ve been out of Genshin since around the start of Fontaine, and coming back now the power creep feels kinda crazy 😭 Can anyone help me put together two decent teams that I can use for now while I catch up and work on getting/building better characters?

*Why?* The post is seeking gameplay guidance and strategic advice.

**Example 2:**

> Every time you get a 5 star artifact these are the possible values each sub stat will have and that they can obtain at every 4 levels. When upgrading artifacts there's RNG choosing which of the 4 sub stats to upgrade, and then another RNG to pick which of those 4 possible values will be added to that sub stat.

*Why?* The post explains game mechanics using factual reasoning.

#### **Label 2: Hot Take**

**Definition:** A post is labeled **Hot Take** when its primary purpose is to express an evaluative judgment or opinion about a character, weapon, banner, game system, or community belief.

**Example 1:**

> Kazuha is pretty overrated. idk what it is about him, but everybody praises him and wants him so badly. They completely ignore other good anemo characters like venti, sucrose & basically all other anemo characters.

*Why?* The post primarily evaluates a character and community perception.

**Example 2:**

> Genshin has not been as fun as of late the quests are just walk here dialogue with choices that mean nothing walk here do the same thing random small fight walk here done and then when you do get a big fight it with acts like you were struggling or you have to wait for the dialogue to end to actually continue fighting then there's the quests that force you to use other characters that suck at that specific boss not to mention the amount of mini games in levels that only show up once and never again 90%bof their new five stars are female and you rarely get male characters even as four stars and not to mention the custom character situation where you have to pay for good things with real money with no way of getting them any other way anyway...

*Why?* The author is arguing a judgment about the game's quality.

#### **Label 3: Reaction**

**Definition:** A post is labeled **Reaction** when its primary purpose is to express an emotional response, personal experience, celebration, disappointment, frustration, or excitement.

**Example 1:**

> I just lost the 50/50 with Lauma. This is the 7th 50/50 I've lost out of the last 8. I'm F2P, so it's incredible. Is there anyone who has the same or worse luck in the world with this game, or is there someone who can understand me? I hate the 50/50. I don't know where the 50/50 chance is, but it must be imaginary because I'm at 50/400.

*Why?* The post is mainly venting frustration and sharing a personal experience.

**Example 2:**

> Hoping to get varka. By end of his half I’ll be able to reach pity once, but it’s a 50/50 :/

*Why?* This is an emotional expression of anticipation and anxiety rather than analysis or debate.

--- 

### Dataset

#### **Data Source**

The dataset was collected from public Genshin Impact discussion spaces, primarily Reddit discussions related to Genshin Impact. 

Only publicly available text posts and comments were used. 

#### **Annotation Process**

All examples were manually reviewed and labeled according to the taxonomy described above. When a post contained multiple discourse styles, I assigned the label corresponding to its **primary communicative intent.**

For example:

- Advice-seeking questions were labeled **Analysis**
- Evaluative opinions were labeled **Hot Take**
- Emotional storytelling was labeled **Reaction**

The annotation process focused on the author's dominant purpose rather than isolated words or phrases.

#### **Dataset Size:** 232 total examples

#### **Label Distribution**

| Label | Count | 
| --- | ---: | 
| Analysis | 73 |
| Hot Take | 81 | 
| Reaction | 78 | 
| Total | 232

No label accounts for more than 70% of the dataset.

#### **Difficult Annotation Cases**

**Example 1: Analysis vs. Hot Take**

> Is Hu Tao really worth using without Staff of Homa? Everyone says she's amazing, but mine feels weak.

Decision: **Analysis**

Although the post contains an opinion about Hu Tao's strength, its primary purpose is seeking gameplay advice rather than persuading readers that Hu Tao is overrated.

**Example 2: Personal Story With Advice Request**

> I've posted about this before. For the full context. I've been playing Genshin since 2021 on an iPad. That iPad is gone. I had the email linked to my Genshin account on there so I didn't think to save it on my phone once I started mainly playing on it. I have an iPhone XS max that will soon no longer be supported by Apple. I've submitted countless account forms to Hoyo support. Every single one has been declined even though the information is correct. I am now no longer allowed to submit any more forms due to how many I was doing. I've stopped playing on the account recently because it dawned on me that one day, it will be the last day I log in to the account. I’ve mainly been on my alt that I started in 2023 and have not logged in until recently. I no longer have the energy to get that account up to what my main is(AR 58). My alt is at AR 28. I don't know what to do anymore. Thanks for reading this. Any advice would be deeply appreciated.

Decision: **Analysis**

The post contains emotional content, but the ultimate goal is obtaining information and guidance.

#### **Example 3: Reaction vs. Hot Take**

> Since joining the game six months ago, I have wished for 10 characters. And behold, I have lost 50/50 on 9 OF THEM. I have triggered capturing radiance twice (on Mavuika C1 and Flins C2, and I'm counting them as a lose too since you have to lose the 50/50 to trigger it). My only win was on Ineffa's first banner. Same with the weapon banner, lost every 50/50. So I'm either very, very unlucky or the gacha in this game is undeniably predatory. If there is a 50% chance to lose, where is the other 50 ???

Decision: **Reaction**

Although the author makes a judgment about the gacha system, the majority of the post focuses on personal frustration and experience.

---

### Fine-Tuning Approach

#### **Base Model**

The fine-tuned classifier uses **distilbert-base-uncased** from Hugging Face Transformers.

#### **Training Platform**

Training was performed using: 

- Google Colab
- Tesla T4 GPU
- Hugging Face Transformers
- PyTorch

#### **Data Split**

The dataset was split using stratified sampling:

| Split | Size | 
| --- | ---: |
| Train | 162 | 
| Validation | 35 | 
| Test | 35 | 

This preserved approximately equal label distributions across all splits.

#### **Hyperparameters**

| Parameter | Value | 
| --- | ---: |
| Epochs | 5 | 
| Learning Rate | 2e - 5 | 
| Batch Size | 16 | 
| Weight Decay | 0.01 | 
| Warmup Steps | 50 | 
| Max Length | 256 | 

**Hyperparameter Decisions**

I increased training from the notebook's default recommendation of 3 epochs to **5 epochs.** Because the dataset contained only 232 examples, I wanted the model to receive additional exposure to the training examples while monitoring validation accuracy. The model did not show catastrophic overfitting, so I retained the 5-epoch configuration.

---

### Baseline Classifier

#### **Approach**

The baseline classifier used **Llama 3.3 70B Versatile (Groq API)** in a zero-shot classification setting. 

The model was provided: 

- Community description 
- Label definitions
- One example for each label
- Instructions to output only a label 

The specific prompt given was: 

```
"""
You are classifying posts from the Genshin Impact community.
Assign each post to exactly one category.

analysis: The primary purpose is to explain, reason about, or seek information regarding gameplay mechanics, optimization, strategy, builds, or evidence-based aspects of the game.
Example: "Is Hu Tao worth using without Staff of Homa? Everyone says she's amazing, but mine feels weak."

hot take: The primary purpose is to express an evaluative opinion or judgment about a character, weapon, banner, or aspect of the game, emphasizing personal opinion over factual analysis.
Example: "Kazuha is pretty overrated. Everyone ignores other good Anemo characters."

reaction: The primary purpose is to express an emotional response or personal experience related to gameplay, story events, or the gacha system.
Example: "I just lost my seventh 50/50 in a row and I'm so frustrated."

If a post mixes multiple styles, classify it based on its primary communicative intent.

Respond with ONLY one of these exact labels:
analysis
hot take
reaction
Do not include any explanation or punctuation.
"""
```

No fine-tuning was performed.

Each test example was sent individually to the model and evaluated on the exact same test set used by the fine-tuned classifier.

---

### Evaluation Report

#### **Overall Accuracy**

| Model | Accuracy | 
| --- | ---: | 
| Groq Zero-Shot Baseline | 0.800 | 
| Fine-Tuned DistilBERT | 0.771 | 

The fine-tuned model achieved strong performance, but the zero-shot baseline slightly outperformed it by approximately 2.9 percentage points.

#### **Fine-Tuned Model Metrics**

| Label    | Precision | Recall |   F1 |
| -------- | --------: | -----: | ---: |
| Analysis |      0.80 |   0.73 | 0.76 |
| Hot Take |      0.62 |   0.83 | 0.71 |
| Reaction |      1.00 |   0.75 | 0.86 |

**Macro Average**

- Precision: 0.81
- Recall: 0.77
- F1: 0.78

#### **Baseline Metrics**

| Label    | Precision | Recall |   F1 |
| -------- | --------: | -----: | ---: |
| Analysis |      1.00 |   0.73 | 0.84 |
| Hot Take |      0.80 |   0.67 | 0.73 |
| Reaction |      0.71 |   1.00 | 0.83 |

**Macro Average**

- Precision: 0.84
- Recall: 0.80
- F1: 0.80

#### **Fine-Tuned Model Confusion Matrix**

| Actual \ Predicted | Analysis | Hot Take | Reaction |
| ------------------ | -------: | -------: | -------: |
| Analysis           |        8 |        3 |        0 |
| Hot Take           |        2 |       10 |        0 |
| Reaction           |        0 |        3 |        9 |

**Confusion Matrix Interpretation**

The most common failure pattern was **Analysis → Hot Take.** Three analysis posts were classified as hot takes.

A second recurring pattern was **Reaction → Hot Take.** Three reaction posts were classified as hot takes.

Notably, the model never confused Analysis with Reaction directly. Instead, Hot Take acted as a middle category that absorbed many borderline examples. This suggests the model learned that strong language, criticism, or emotionally charged wording often signals Hot Take, even when the author's actual intent is seeking information or sharing a personal experience.

#### **Error Analysis**

Before writing this section, I used ChatGPT to review the misclassified examples and identify recurring patterns. The suggested patterns included:

- Questions being mistaken for opinions
- Emotional language being mistaken for evaluative judgment
- Borderline posts containing both analysis and criticism

After manually reviewing the examples, I agreed with these patterns.

**Error Example 1**

> I’ve been out of Genshin since around the start of Fontaine, and coming back now the power creep feels kinda crazy 😭 Can anyone help me put together two decent teams that I can use for now while I catch up and work on getting/building better characters?

- True Label: **Analysis**
- Predicted Label: **Hot Take**
- Confidence: **0.36**

**Why It Failed?** The phrase "the power creep feels kinda crazy" looks like an evaluate judgment. However, the actual purpose of the post is requesting gameplay help. The model appears to overweight opinionated language and underweight the advice-seeking structure of the post.

**How To Improve?** Add more training examples where questions contain evaluative wording but are ultimately seeking gameplay guidance.

**Error Example 2**

> Is there a way of knowing if the wish RNG is truly random?

- True Label: **Analysis**
- Predicted Label: **Hot Take**
- Confidence: **0.47**

**Why It Failed?** This is an extremely short post with little context. Humans can infer that the author is asking a factual question. The model likely lacks enough information to confidently identify the post as information-seeking.

**How To Improve?** Collect more short analysis-oriented questions during annotation.

**Error Example 3**

> If I’m not guaranteed a character and I’m unable to get the character then I will have felt like I just wasted my time getting all those materials and now they would have to sit in my inventory until the character comes back, which is also bad because I might accidentally use them up.

- True Label: **Reaction**
- Predicted Label: **Hot Take**
- Confidence: **0.41**

**Why It Failed?** The author is expressing frustration about a personal experience However, the statement also resembles criticism of the game's progression systems. The model appears to associate negative sentiment with Hot Take, even when the post is primarily emotional rather than evaluative.

**How To Improve?** Include additional reaction examples involving frustration, disappointment, and emotional venting without explicit evaluation.

#### **Sample Classifications**

*Examples from the fine-tuned DistilBERT model. All of these examples were labeled correctly by the model.*

| Example                                                                               | Predicted Label | Confidence |
| ------------------------------------------------------------------------------------- | --------------- | ---------: |
| "You just have to build around reactions now. Think about how each reaction works and then consider the characters and their roles in it. Like are they on or off field, how do they apply their element and how often, whether they will be the aura or the trigger. Good things to focus on are trying to get an on-field DPS of each element and then supports who can fit on reaction teams for them. Look around the wiki for more info about the mechanics I mentioned and character kit details." | Analysis        |       0.66 |
| "Yesterday I lost my 13th 50/50 wishing for Citlali on her current rerun banner. I had 282 wishes at 52 pity so I knew I could guarantee her, but it sucks knowing I had to spend 100 wishes instead of 30 (which is when C2 Dehya appeared)... Especially right before Nodkrai which is filled with characters I want. T ^ T"                               | Reaction        |       0.42 |
| "I enjoyed Inazuma puzzles the most. The combination of frustration and achievement when you finally get it. Mainland puzzles almost feel insulting to cognitive skills. Especially considering what Sumeru is supposed to stand for."                                               | Hot Take        |       0.44 |
| "Sheer cold was a fun way to spice up exploration and not putting a heat meter in the desert was the single lamest thing they've done in this game"                                 | Hot Take        |       0.48 |
| "focus on building one good team for 4 characters! ur not given a lot of resources so it’s important to make sure u have a solid team before trying to build a bunch of other characters. don’t use ur fragile resin, save it for when ur late game + u can use it to farm for 5 star artifacts !"                              | Analysis        |       0.59 |

**Example of a Correct Prediction** 

> focus on building one good team for 4 characters! ur not given a lot of resources so it’s important to make sure u have a solid team before trying to build a bunch of other characters. don’t use ur fragile resin, save it for when ur late game + u can use it to farm for 5 star artifacts !

Predicted: **Analysis (0.59)**

This prediction is reasonable because the entire post consists of gameplay advice and strategic recommendations. The author is explaining resource management and team-building decisions rather than expressing an opinion or emotional reaction.

---

### Reflection: What the Model Learned vs. What I Intended

My intended distinction was based on communicative intent:

- Analysis = reasoning or information seeking
- Hot Take = evaluation or judgment
- Reaction = emotional experience

The model learned some of these distinctions, but it also relied heavily on surface-level language cues. In particular:

- Negative language frequently triggered Hot Take predictions.
- Emotional frustration was sometimes mistaken for evaluative judgment.
- Advice-seeking questions containing criticism were often classified as Hot Take.

This suggests the model learned a decision boundary closer to **opinionated sounding language** rather than **the author's underlying communicative goal.** The largest challenge remains separating Analysis from Hot Take when posts contain both reasoning and criticism.

---

### Spec Reflection

#### **How The Spec Helped**

The specification's emphasis on defining difficult edge cases early was extremely valuable. Writing explicit boundary rules forced me to think carefully about how to distinguish:

- Advice-seeking vs. opinion-sharing
- Emotional venting vs. evaluative criticism

This made the annotation process much more consistent.

#### **How My Implementation Diverged**

In my planning document, I defined success as:

- At least 75% accuracy
- At least 0.70 F1 for every label

The accuracy goal was achieved, but the project revealed something unexpected: **The zero-shot LLM baseline slightly outperformed the fine-tuned classifier.**

As a result, the most interesting outcome became understanding why fine-tuning underperformed rather than simply trying to maximize accuracy. This shifted the focus of the project toward error analysis and label boundaries.

--- 

### AI Usage

#### **AI Use #1: Label Taxonomy Stress Testing**

I used ChatGPT during the planning phase to generate borderline examples between Analysis and Hot Take. I reviewed these examples manually and used them to refine my label definitions and decision rules. Several generated examples revealed that questions containing opinions were particularly difficult to classify consistently, leading me to explicitly document how those cases should be handled.

#### **AI Use #2: Failure Pattern Analysis**

After obtaining evaluation results, I provided misclassified examples to ChatGPT and asked it to identify common failure patterns. The model suggested several recurring themes:

- Advice-seeking questions mistaken for opinions
- Emotional frustration mistaken for Hot Take
- Short posts lacking sufficient context

I manually reviewed the examples and verified these patterns before incorporating them into the evaluation report.

#### **AI Use #3: Preliminary Annnotation Assistance**

During dataset creation, I also experimented with using ChatGPT to propose preliminary labels for some Genshin Impact posts before manual annotation. My prompts asked the model to classify posts according to my three-label taxonomy (Analysis, Hot Take, and Reaction) using the definitions I had established in `planning.md`. I did **not** accept these labels automatically. Instead, I manually reviewed every suggested label and compared it against my own decision rules. When the AI's suggestion conflicted with my interpretation—particularly for mixed cases involving emotional questions seeking gameplay advice or opinionated strategic recommendations—I overrode the proposed label to maintain consistency with my taxonomy. Using AI in this way accelerated the annotation process but did not replace human judgment. The final dataset consists only of labels that I personally reviewed and approved, and any AI-generated suggestions were treated as drafts rather than ground truth.