# Project RUNE - The Digital Mystery Archives

**A Cloud Security Learning Project exploring AWS troubleshooting through hands-on investigations.**

**Status:** Case 001: Phase 1 Completed | IAM Investigation Planned

## Overview

Project RUNE is my personal AWS learning project, created as I work towards a career in cloud support and security.

For Case 001, I set up a private Amazon S3 bucket and investigated why opening a file's URL in a browser returned an AccessDenied error. I wanted to understand whether something was wrong with the configuration or whether AWS was working as intended.

I'm documenting what I test, what I find and what I still need to learn. The longer-term idea is to turn these investigations into interactive mystery cases.

## Case 001 - The Locked Archive

**Scenario:** A user attempts to retrieve a file stored in Amazon S3 but receives an AccessDenied error.

**Investigation question:** Why is access denied, and is this an actual configuration problem or expected security behaviour?

## Learning Objectives:

- Understand Amazon S3 buckets and object storage.
- Distinguish anonymous access from authenticated access.
- Examine S3 Block Public Access and bucket policies.
- Develop a structured approach to investigating access errors.
- Explore AWS IAM and the principle of least privilege in future experiments.

## Lab Environment

- Cloud platform: Amazon Web Services (AWS)
- Service: Amazon S3
- Region: Europe (Stockholm), eu-north-1
- Test bucket: rune-case001-archive-2026-edz
- Test object: rune-case001.txt
- Documentation: Markdown and GitHub

All experiments use an authorised AWS account and non-sensitive sample data.

## Investigation and Findings

### Step 1 - Create the Archive

1. Created a private Amazon S3 bucket and uploaded a sample text file.

2. **Result:** The file was successfully stored in S3.

![S3 test object uploaded](case001-s3-object-uploaded.png)

### Step 2 - Reproduce AccessDenied

1. Opened the object's HTTPS URL directly in a browser without an authenticated S3 request.

2. **Observed result:** Amazon S3 returned an XML response containing AccessDenied.

![Anonymous S3 AccessDenied](case001-anonymous-acess-denied.png)

### Step 3 - Verify Authenticated Access

1. Returned to the AWS Management Console and downloaded the same object through the authenticated session.

2. **Observed result:** The file downloaded successfully.

### Step 4 - Examine Bucket Permissions

1. Reviewed the bucket's Permissions tab.

2. **Observed configuration:**

   - Block all public access: **On**
   - Bucket policy: **No policy configured**

![S3 public access blocked](case001-bucket-public-access-blocked.png)

### Step 5 - Interpret the Evidence

1. The file returned AccessDenied when I opened its URL directly in a browser, but I could download it through the AWS Management Console.

2. I checked the bucket permissions and found that Block Public Access was enabled and no bucket policy was configured.

3. **Conclusion:** The error was expected. The bucket was private, so an unauthenticated request could not retrieve the file.

4. This test did not reproduce an IAM permission error involving an authenticated user or role. That will be part of Phase 2.

## Investigation Limitation

The AWS IAM Policy Simulator was unavailable under the current account configuration.

No account upgrade was performed, and no public access settings were disabled to bypass the restriction.

A controlled IAM permissions investigation remains future work.

![IAM Policy Simulator unavailable](case001-policy-simulator-unavailable.png)

## Security Lessons Learned

- An AccessDenied response does not automatically mean a system is broken.
- Authentication and authorisation are different concepts.
- A direct S3 object URL does not automatically grant access to a private object.
- Access-control settings should be investigated before attempting changes.
- Troubleshooting should preserve secure defaults rather than introduce unnecessary permissions.

## Project Roadmap

### Phase 1 - S3 Access Investigation (Completed)

- [x] Create the GitHub repository.
- [x] Create a private S3 bucket and upload a test object.
- [x] Reproduce and record an anonymous AccessDenied response.
- [x] Verify access through the authenticated AWS Console.
- [x] Examine bucket public-access settings and bucket policy.
- [x] Document the initial investigation and findings.
- [x] Publish reviewed and redacted evidence screenshots.

### Phase 2 - IAM Permissions Investigation (Planned)

- [ ] Set up a suitable authorised IAM testing environment.
- [ ] Reproduce an access denial involving an authenticated IAM identity.
- [ ] Investigate identity-based permissions and identify the cause.
- [ ] Test a least-privilege solution and document the results.

### Phase 3 - Interactive Mystery Experience (Future Development)

- [ ] Develop an interactive mystery investigation interface.
- [ ] Explore additional cloud security and technical support cases.

## Learning Approach

I'm building Project RUNE alongside my Higher Diploma in Science in Computing at the National College of Ireland.

This is my first AWS investigation. My aim is to build practical experience, learn how to troubleshoot problems and become more confident explaining technical findings.
