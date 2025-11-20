┌─────────────────────────────────────────────────────────────────┐
│                         TRIGGER                                 │
│  Reddit: Watch Comments in Subreddit (polls every 15 min)       │
│  Output: New comment data (author, text, post, upvotes, etc.)   │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                    AI AGENT MODULE                              │
│  Role: Reddit Community Moderator                               │
│  Input: {{comment.text}}, {{author.username}}, {{post.title}}   │
│                                                                 │
│  Task: Analyze comment and classify as:                         │
│   • SAFE - Normal discussion                                    │
│   • SPAM - Promotional/bot content                              │
│   • TOXIC - Harassment/hate speech                              │
│   • RULE_VIOLATION - Breaks subreddit rules                     │
│   • INVESTIGATE - Check user history                            │
│                                                                 │
│  Output: {classification, confidence, reasoning, action}        │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                       ROUTER                                    │
│  Branch based on AI Agent's classification                      │
└───┬──────────┬──────────┬──────────────┬─────────────────────┬──┘
    │          │          │              │                     │
    ▼          ▼          ▼              ▼                     ▼
┌─────┐  ┌─────────┐ ┌─────────┐  ┌──────────┐      ┌──────────────┐
│SAFE │  │  SPAM   │ │ TOXIC   │  │   RULE   │      │ INVESTIGATE  │
│     │  │         │ │         │  │VIOLATION │      │              │
│ No  │  │ Remove  │ │ Remove  │  │  Remove  │      │  Check User  │
│Action│ │ Comment │ │ Comment │  │  Comment │      │   History    │
└─────┘  │         │ │ + Ban   │  │          │      │              │
         │ + Send  │ │ User    │  │ + Send   │      │  Reddit: Get │
         │ Warning │ │         │  │ Warning  │      │  Comments by │
         │  DM     │ │ + Gmail │  │   DM     │      │  User        │
         │         │ │  Alert  │  │          │      │              │
         └─────────┘ │  to     │  └──────────┘      │  AI Agent:   │
                     │  Mods   │                    │  Analyze     │
                     │         │                    │  Pattern     │
                     └─────────┘                    │              │
                                                    │  If suspicious│
                                                    │  → Gmail     │
                                                    │    Alert     │
                                                    └──────────────┘