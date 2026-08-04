# Day 17 — MFA, Root Account Security Best Practices

## What it is
**MFA (Multi-Factor Authentication)** adds a second proof of identity beyond a password — usually a time-based code from an app on your phone.

## Why it matters
Password alone = "something you know." An attacker who steals your password still can't log in without also having your MFA device. This is one of the highest-impact, lowest-effort security improvements you can make.

## Key concepts

### MFA types AWS supports
- **Virtual MFA device** — authenticator app (Google Authenticator, Authy) generating rotating codes
- **Hardware MFA device** — physical token
- **FIDO Security Key** — USB/NFC key using WebAuthn
- **SMS-based MFA** — supported but discouraged (vulnerable to SIM swapping)

### Root account best practices
- Root user has **unrestricted** access to the entire account — never use it day-to-day
- Enable MFA on root **immediately** after account creation
- Create an IAM user (or use federation) for daily work instead
- Don't create access keys for the root account unless absolutely necessary

### MFA Delete (S3-specific)
A special S3 feature requiring MFA to permanently delete objects or disable versioning — protects against accidental or malicious deletion.

## Hands-on / commands

```bash
# Enable a virtual MFA device for a user (via Console is easier for QR scanning)
aws iam create-virtual-mfa-device --virtual-mfa-device-name my-mfa-device \
  --outfile QRCode.png --bootstrap-method QRCodePNG

# Enable MFA on the device (requires two consecutive codes)
aws iam enable-mfa-device --user-name my-user \
  --serial-number arn:aws:iam::123456789012:mfa/my-mfa-device \
  --authentication-code1 123456 --authentication-code2 789012
```

Example IAM policy condition requiring MFA:
```json
"Condition": {
  "BoolIfExists": {
    "aws:MultiFactorAuthPresent": "true"
  }
}
```

## Common exam gotchas
- MFA Delete on S3 requires the bucket owner's root account credentials + MFA to enable/disable — not something a regular IAM user can toggle
- SMS MFA is technically supported but is the least secure option — avoid recommending it in security-focused answers
- Enforcing MFA via IAM policy condition (`aws:MultiFactorAuthPresent`) is a common real exam scenario for "require MFA before allowing sensitive actions"

## My notes / things that confused me
this maps directly to Azure Conditional Access policies requiring MFA, if you're familiar with that side of identity management
