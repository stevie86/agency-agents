# Agent Workflow Sequences
# Predefined multi-agent workflows for Sisyphus

## 🔄 Common Sequences

### 1. Feature Development
```
User: "Add user authentication to the app"

Sequence:
1. Backend Architect → Design auth system (JWT, OAuth)
2. Security Engineer → Review for vulnerabilities  
3. Frontend Developer → Build login/signup UI
4. QA Engineer → Write auth tests
5. DevOps Automator → Deploy with secrets management
```

### 2. Performance Issue
```
User: "The app is slow"

Sequence:
1. Backend Architect → Identify bottlenecks
2. Database Optimizer → Tune slow queries
3. Frontend Developer → Optimize bundle/rendering
4. DevOps Automator → Add monitoring/alerts
```

### 3. New Mobile App
```
User: "Build a mobile app for iOS and Android"

Sequence:
1. UI Designer → Create mockups and design system
2. Mobile App Developer → Build with React Native/Flutter
3. Backend Architect → Design API backend
4. QA Engineer → Test on devices
5. DevOps Automator → CI/CD for app stores
```

### 4. Security Audit
```
User: "Check if my app has security issues"

Sequence:
1. Security Engineer → Full vulnerability scan
2. Backend Architect → Review API security
3. Code Reviewer → Check code for anti-patterns
4. DevOps Automator → Harden infrastructure
```

### 5. Data Pipeline
```
User: "Set up data analytics for our product"

Sequence:
1. Data Engineer → Build ETL pipeline
2. Data Scientist → Define metrics and analysis
3. Backend Architect → Design data API
4. Frontend Developer → Build analytics dashboard
```

## 🎯 Routing Logic for Sisyphus

```python
# Intent detection → Agent routing
INTENT_MAP = {
    # Direct requests (single agent)
    "fix bug": "Code Reviewer",
    "review code": "Code Reviewer",
    "deploy": "DevOps Automator",
    "security check": "Security Engineer",
    
    # Multi-step (sequence)
    "new feature": "feature_development_sequence",
    "slow app": "performance_sequence", 
    "mobile app": "mobile_app_sequence",
    "security audit": "security_audit_sequence",
    "analytics": "data_pipeline_sequence",
}

def route(request):
    # Check for multi-step intent first
    for pattern, sequence in SEQUENCE_MAP.items():
        if pattern in request.lower():
            return run_sequence(sequence)
    
    # Single agent routing
    agent = find_agent_by_keywords(request)
    return delegate(agent, request)
```

## ⚡ Quick Reference

| You Say | Sisyphus Routes To |
|---------|-------------------|
| "Fix this bug" | Code Reviewer |
| "Add login" | Feature Development sequence |
| "App is slow" | Performance sequence |
| "Deploy to prod" | DevOps Automator |
| "Security issue" | Security Audit sequence |
| "Build mobile app" | Mobile App sequence |
| "Add analytics" | Data Pipeline sequence |
| "Review this PR" | Code Reviewer |
