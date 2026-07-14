# Swingers Support Site Deployment

Upload every file in this directory to a public HTTPS host before App Store submission.

Required App Store Connect URLs:

- Support URL: `https://<host>/support.html`
- Privacy Policy URL: `https://<host>/privacy.html`

Optional App Store Connect URL:

- Marketing URL: `https://<host>/`

Before hosting, replace the generic support-page contact copy with a real public
support email or contact method. Apple expects the Support URL itself to lead to
real contact information.

Recommended setup:

```bash
npm run app-store:support-contact -- --email <real-support-email>
npm run app-store:site
npm run ios:sync:clean
```

After hosting:

1. Open all three URLs in Safari.
2. Confirm `privacy.html` shows "Swingers Privacy Policy".
3. Confirm `support.html` shows "Swingers Support".
4. Confirm `support.html` includes the real support contact.
5. Use `app-store/metadata/app_store_preflight.env.example` as the template for `.env.app-store` or `app-store/metadata/app_store_preflight.env`. The App Store scripts load either file automatically while preserving any values already exported in your shell.
6. Verify the hosted pages from the repo:

   ```bash
   APP_STORE_PRIVACY_URL=https://<host>/privacy.html \
   APP_STORE_SUPPORT_URL=https://<host>/support.html \
   APP_STORE_MARKETING_URL=https://<host>/ \
   APP_STORE_CONNECT_APP_ID=<app-store-connect-id> \
   APP_STORE_LEGAL_SELLER="<legal seller/copyright owner>" \
   APP_STORE_REVIEW_CONTACT_FIRST_NAME=<review-contact-first-name> \
   APP_STORE_REVIEW_CONTACT_LAST_NAME=<review-contact-last-name> \
   APP_STORE_REVIEW_CONTACT_PHONE=<review-contact-phone> \
   APP_STORE_REVIEW_CONTACT_EMAIL=<review-contact-email> \
   APP_STORE_PRIVACY_ANSWERS_CONFIRMED=1 \
   APP_STORE_AGE_RATING_CONFIRMED=1 \
   APP_STORE_EXPORT_COMPLIANCE_CONFIRMED=1 \
   APP_STORE_DSA_TRADER_STATUS=<trader|non-trader|not-distributed-in-eu> \
   APP_STORE_DEVICE_QA_CONFIRMED=1 \
   APP_STORE_DEVICE_QA_DEVICES="<tested iPhone models and iOS versions>" \
   npm run app-store:preflight:full
   ```

7. Enter the required URLs in App Store Connect. Use the optional Marketing URL field if you want the listing to link to the hosted landing page.
8. Enter the App Store Connect app ID, legal seller/copyright owner, App Review contact first name, last name, phone, email, and EU DSA trader-status value into your local shell environment or untracked env file for final preflight.
9. Add the same App Review contact details in App Store Connect under App Review information before submitting for review.
10. Complete App Privacy in App Store Connect using `app-store/metadata/app_privacy_answers.json` and set `APP_STORE_PRIVACY_ANSWERS_CONFIRMED=1`.
11. Complete age rating in App Store Connect using `app-store/metadata/age_rating_draft.json` and set `APP_STORE_AGE_RATING_CONFIRMED=1`.
12. Complete export compliance in App Store Connect using `app-store/metadata/export_compliance.json`; answer `No` for non-exempt encryption and set `APP_STORE_EXPORT_COMPLIANCE_CONFIRMED=1`.
13. Complete the EU Digital Services Act trader-status disclosure in App Store Connect. If you declare trader status, verify the required public DSA contact details with Apple and set `APP_STORE_DSA_TRADER_CONTACTS_VERIFIED=1` after verification.
14. Install the current Release build on physical iPhone hardware with `npm run ios:device:release`, complete `app-store/metadata/device_qa_checklist.md`, then set `APP_STORE_DEVICE_QA_CONFIRMED=1` and `APP_STORE_DEVICE_QA_DEVICES="<tested iPhone models and iOS versions>"`.
15. For command-line upload, add App Store Connect API credentials to your shell or local untracked env file:

   ```bash
   APP_STORE_CONNECT_API_KEY_ID=<api-key-id>
   APP_STORE_CONNECT_API_ISSUER_ID=<issuer-id>
   APP_STORE_CONNECT_API_KEY_PATH=<path-to-AuthKey_api-key-id.p8>
   # Or use API_PRIVATE_KEYS_DIR=<directory-containing-AuthKey_api-key-id.p8>
   ```

   Keep the `.p8` private key outside git. If your account has multiple providers, also set `APP_STORE_PROVIDER_PUBLIC_ID`.

16. Run `npm run ios:submit:appstore` to rebuild, preflight, export, and upload the `.ipa`.
17. Update `app-store/metadata/external_submission_blockers.md` if these blockers are cleared.

Do not use a localhost, private repository preview, unauthenticated file URL, or non-HTTPS URL for App Store Connect.
