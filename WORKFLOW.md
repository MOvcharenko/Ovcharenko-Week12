┌─────────────────────────────────────────────────────────────────┐
│                    1. TRIGGER MODULE                             │
│           Reddit: Watch Comments in Subreddit                    │
│                                                                   │
│  Monitors: r/YourTestSubreddit                                   │
│  Output: Comment data (body, author, post title, etc.)          │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                2. DATA PREPARATION MODULE                        │
│              Tools > Set Multiple Variables                      │
│                                                                   │
│  Variables Created:                                              │
│   • commentText = {{1.body}}                                     │
│   • commentAuthor = {{1.author}}                                 │
│   • postTitle = {{1.link_title}}                                 │
│   • subredditName = {{1.subreddit}}                              │
│                                                                   │
│  Purpose: Extract and normalize Reddit data for AI processing   │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                   3. AI AGENT MODULE                            │
│              MAke AI Agent > Run an agent                       │
│                      (GPT-4 Turbo)                               │
│                                                                   │
│  System Prompt: Strict Reddit moderation rules                  │
│  Message: "{{7.commentText}}{{7.commentAuthor}}{{7.postTitle}}"  │
│                                                                  │
│  Task: Classify as SAFE, SPAM, TOXIC, RULE_VIOLATION,           │
│        or INVESTIGATE                                            │
│                                                                   │
│  Output: JSON response with classification, confidence,          │
│          reasoning, and recommended action                       │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                  4. JSON PARSER MODULE                           │
│                   Tools > Parse JSON                             │
│                                                                   │
│  Input: {{4.response}}                                           │
│                                                                   │
│  Extracts:                                                       │
│   • classification (SAFE, SPAM, TOXIC, etc.)                     │
│   • confidence (0.0-1.0)                                         │
│   • reasoning (explanation)                                      │
│   • recommended_action (what to do)                              │
└──────────────────────┬──────────────────────────────────────────┘
                       │
                       ▼
                  ┌─────────┐
                  │ ROUTER  │
                  └────┬────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
    ┌──────────────────┐  ┌──────────────────┐
    │   ROUTE 1        │  │   ROUTE 2        │
    │   SAFE           │  │   ACTION         │
    │                  │  │   REQUIRED       │
    └────────┬─────────┘  └────────┬─────────┘
             │                     │
             ▼                     ▼
    ┌──────────────────┐  ┌──────────────────┐
    │ 5a. Google       │  │ 5b. Gmail >      │
    │     Sheets >     │  │     Send Email   │
    │     Add a Row    │  │                  │
    │                  │  │  Alert sent to   │
    │ Log safe         │  │  moderators with │
    │ comments for     │  │  full details    │
    │ record keeping   │  │                  │
    └──────────────────┘  └──────────────────┘