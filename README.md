# cloudfront-demo
deploy static pages using s3,cloudfront
Got it 👍
Let’s **reset mentally** and do this in a **very strict, step-by-step way**, assuming **you already made the S3 bucket public** and just want CloudFront to work.
Follow **exactly in this order**. Don’t skip any step.

I’ll keep it **simple, demo-style (public S3)**.

---

# GOAL

Open this successfully:

```
https://d2tbiaxp64rb63.cloudfront.net
```

---

# STEP 1: Fix S3 first (MOST IMPORTANT)

### 1️⃣ S3 → Your Bucket → Permissions

#### A. Block public access

* Click **Edit**
* ❌ Uncheck **Block all public access**
* ✔ Save

---

#### B. Bucket policy (COPY–PASTE EXACTLY)

Replace **BUCKET_NAME** with your real bucket name.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::BUCKET_NAME/*"
    }
  ]
}
```

Click **Save**
✅ No red error should appear.

---

#### C. ACL (IMPORTANT)

* Permissions → **Object Ownership**
* Select: **ACLs enabled**
* Object ownership: **Bucket owner preferred**
* Save

---

### 2️⃣ Upload index.html

Upload this file:

```html
<!DOCTYPE html>
<html>
<body>
<h1>CloudFront Working 🚀</h1>
</body>
</html>
```

---

### 3️⃣ Test S3 DIRECTLY (MUST WORK)

Click on `index.html` → **Object URL**

It should open like:

```
https://BUCKET_NAME.s3.ap-south-1.amazonaws.com/index.html
```

❗ **If this does NOT open → STOP**
CloudFront will NEVER work until this works.

👉 Tell me if this step fails.

---

# STEP 2: CloudFront (DO THIS ONLY AFTER STEP 1 WORKS)

### 4️⃣ CloudFront → Create Distribution

#### Origin settings

* Origin domain:

  ```
  BUCKET_NAME.s3.ap-south-1.amazonaws.com
  ```

  ⚠️ **DO NOT use s3-website URL**

* Origin access: **Public**

* Origin path: leave empty

---

### 5️⃣ Default cache behavior

* Viewer protocol policy: **Redirect HTTP to HTTPS**
* Allowed methods: **GET, HEAD**
* Cache policy: **CachingOptimized**

---

### 6️⃣ Settings (VERY IMPORTANT)

* Default root object:

  ```
  index.html
  ```
* Price class: Any
* Create distribution

---

# STEP 3: WAIT ⏳

* Wait **10 minutes**
* Status must be: **Deployed**

---

# STEP 4: Test CloudFront

Open:

```
https://d2tbiaxp64rb63.cloudfront.net
```

You should see:

```
CloudFront Working 🚀
```

---

# IF YOU STILL GET AccessDenied

### 99% reason:

You used **WRONG origin**

❌ WRONG:

```
s3-website-ap-south-1.amazonaws.com
```

✅ CORRECT:

```
BUCKET_NAME.s3.ap-south-1.amazonaws.com
```

---



I’ll fix it immediately based on your answers.
