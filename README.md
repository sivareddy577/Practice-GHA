                     ┌────────────────────────────┐
                     │        GitHub Push         │
                     │        (main branch)       │
                     └────────────┬──────────────┘
                                  │ triggers
                                  ▼
               ┌──────────────────────────────┐
               │        Build Job             │
               │ (Node + Tests + Docker Build)│
               │ - Checkout code             │
               │ - Setup Node                │
               │ - Restore & Cache deps      │
               │ - Run Tests                 │
               │ - Build Docker Image        │
               └────────────┬────────────────┘
                            │ outputs image tag
                            ▼
               ┌──────────────────────────────┐
               │   Push Docker to Artifactory │
               │ - Login with secrets/ OIDC   │
               │ - Tag & Push image            │
               └────────────┬────────────────┘
                            │ triggers next job
                            ▼
               ┌──────────────────────────────┐
               │     Deploy to Staging        │
               │ - SSH / OIDC access          │
               │ - Pull image from Artifactory│
               │ - Stop old container         │
               │ - Run new container          │
               └────────────┬────────────────┘
                            │ manual approval (env: staging)
                            ▼
               ┌──────────────────────────────┐
               │  Deploy to Production         │
               │ - Requires manual approval   │
               │ - SSH / OIDC access          │
               │ - Pull image from Artifactory│
               │ - Stop old container         │
               │ - Run new container          │
               └────────────┬────────────────┘
                            │ optional notifications
                            ▼
               ┌──────────────────────────────┐
               │ Slack / Teams / Email Alert  │
               └──────────────────────────────┘
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/56939d20-a849-4533-80ca-fb56fb0739c7" />
