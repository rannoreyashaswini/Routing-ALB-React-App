1. App Setup (Minimal)
Launch EC2 instance (Amazon Linux/Ubuntu).
Install Node.js.
Run React Vite app on port 3000.

npm install
npm run build
npm run preview -- --port 3000

2. Security Groups
ALB SG: Allow inbound traffic on port 80 (HTTP) from internet.
EC2 SG: Allow inbound traffic on port 3000 only from ALB SG.

3. Create Target Group
Target type: Instances.
Protocol: HTTP.
Port: 3000.
Register EC2 instance.
Health check path: /.

4. Application Load Balancer
Create ALB in public subnets.
Listener: Port 80 (HTTP).
Action: Forward requests to target group (port 3000).

5. Routing Verification
Get ALB DNS name.

Open in browser → React app loads via port 80.
Confirm health checks are passing.

6. Troubleshooting Notes
Health check fails → check SG rules, app running on 3000, correct path.
App not loading → verify listener forwards to correct target group.
Timeouts → ensure EC2 SG allows traffic from ALB SG.

Design:
Start
  |
  v
Is ALB DNS reachable?
  |-- No --> Check ALB SG (port 80 open to internet)
  |
  v
Are health checks passing?
  |-- No --> Verify:
              - App running on port 3000
              - Correct health check path (/)
              - EC2 SG allows traffic from ALB SG
  |
  v
Does app load via ALB DNS?
  |-- No --> Confirm listener forwards to target group (port 3000)
              Check target group registration
  |
  v
Success: React app accessible via port 80
