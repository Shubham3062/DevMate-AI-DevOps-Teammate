┌───────────────────────┐
│        START          │
│                       │
│ input                 │
│ role                  │
│ environment           │
│ requested_action      │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│     POLICY CHECK      │
│                       │
│ ALLOW                 │
│ DENY                  │
│ REQUIRE_APPROVAL      │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│      KNOWLEDGE        │
│      SEARCH/RAG       │
└───────────┬───────────┘
            │
            ▼
┌────────────────────────────────┐
│          DEVMATE AGENT         │
│                                │
│ System Prompt                  │
│ Skills                         │
│ Knowledge                      │
│ MCP Tools                      │
│ Policy Context                 │
│                                │
│ Response Format: JSON Schema   │
└───────────────┬────────────────┘
                │
                ▼
       ┌─────────────────┐
       │ STRUCTURED JSON │
       └────────┬────────┘
                │
                ▼
       ┌─────────────────┐
       │   AUDIT TABLE   │
       └────────┬────────┘
                │
                ▼
               CHAT
