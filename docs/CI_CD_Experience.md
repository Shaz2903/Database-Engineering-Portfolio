CI/CD Workflow Diagram
=======================

Requirement (Jira)
        ↓
PL/SQL Development
        ↓
Unit Testing (utPLSQL)
        ↓
Prepare Deployment Scripts
        ↓
Commit & Push (GitKraken)
        ↓
Create Pull Request
        ↓
Peer Code Review
        ↓
Merge PR
        ↓
Create Test Branch
        ↓
Deploy to Test Environment
        ↓
QA / Developer Testing
        ↓
Defects / Enhancements?
      Yes ↺ Back to Development
      No ↓
Ready for Release (Jira)
        ↓
Change Management
        ↓
CAB Approval
        ↓
Production Deployment
        ↓
Post-Deployment Validation
