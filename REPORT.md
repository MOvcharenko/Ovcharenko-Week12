I had to correctly confgigure the variable flow from one Make module to another.
Needed to add a "Parse JSON" between the AI Agent and the GMail to produce structured output.
As well as a "Set multiple values" between Reddit output and the AI Agent, so that the LLM could have access to proper context.

Initial testing revealed the AI misclassified a toxic comment 
containing profanity and personal attacks as SAFE. This highlighted 
the importance of explicit classification criteria and examples in 
AI prompts. After refining the prompt with specific toxicity rules 
and hostile language patterns, classification accuracy improved 
significantly. This demonstrates that AI agents require careful 
prompt engineering and testing with edge cases to perform reliably 
in moderation contexts."