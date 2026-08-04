# Day 9 — Creating a Bucket, Uploading Files, Static Website Hosting

## What it is
Hands-on walkthrough of turning an S3 bucket into a live static website — useful for hosting a React/Vite `dist/` build, exactly like the frontend of your task-tracker project.

## Why it matters
Static website hosting on S3 is one of the cheapest, simplest ways to serve a frontend — no servers to manage at all.

## Hands-on / steps

### 1. Create the bucket
```bash
aws s3 mb s3://my-static-site-2026 --region eu-west-1
```

### 2. Disable Block Public Access (needed for a public website)
Console → Bucket → Permissions → Block Public Access → Edit → uncheck all → Save

### 3. Add a bucket policy allowing public read
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-site-2026/*"
    }
  ]
}
```

### 4. Enable Static Website Hosting
Console → Bucket → Properties → Static website hosting → Enable
- Index document: `index.html`
- Error document: `error.html` (optional)

### 5. Upload your files
```bash
aws s3 cp ./dist/ s3://my-static-site-2026/ --recursive
```

### 6. Access your site
Use the endpoint shown under Static Website Hosting settings:
```
http://my-static-site-2026.s3-website-eu-west-1.amazonaws.com
```

## Common exam gotchas
- Block Public Access **overrides** any bucket policy — this is the #1 cause of "I added a policy but still get 403" confusion
- Forgetting to set the Index Document causes errors even when `index.html` exists in the bucket
- S3 website endpoints are HTTP only — for HTTPS, you'd put CloudFront in front of the bucket

## My notes / things that confused me
Block Public Access overriding policy

I added a bucket policy for public access... and still got 403

Confusing at first — I wrote a policy that clearly said "allow public read," but access was still denied. Turns out Block Public Access is a separate setting that overrides the bucket policy entirely, regardless of what the policy says. It's AWS's safety net — even a "correct" policy gets ignored if this toggle is still on. Had to manually turn it off before the policy actually took effect.
