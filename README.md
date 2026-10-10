# Card Picker

A passcode-protected credit card recommender. Visitors mark perks as **Use / Maybe / Wouldn't use**. The site then ranks 9 cards and suggests same-ecosystem combos:

- Sapphire Reserve and Sapphire Preferred
- Amex Platinum and Gold
- Venture X and Venture
- Freedom Unlimited and Freedom Flex
- Savor

A **Referrals** tab lets anyone with the passcode share their referral link along with a short recommendation.

Everything lives in a single file, `index.html`, with no build step. It's hosted on GitHub Pages, and referrals are stored in Firestore.

## One-time setup

1. **Create a Firebase project** at https://console.firebase.google.com (Analytics not needed).
2. **Create a Firestore database:** Build → Firestore Database → Create database (production mode).
3. **Register a web app:** Project settings → Your apps → Web (`</>`). Copy the `firebaseConfig` object into `index.html`, replacing the `REPLACE_ME` block.
4. **Set the passcode:** in the Firestore data tab, create collection `config` → document `access` → field `code` (string) = your passcode.
5. **Publish the rules:** paste `firestore.rules` into Firestore → Rules and click Publish.
6. **Host it:** push this folder to a GitHub repo, then Settings → Pages → Deploy from branch `main` / root.

## How the passcode works

- Referrals are stored at `sites/{passcode}/referrals`. The rules only open the path whose name equals `config/access.code`.
- A wrong passcode reads nothing.
- To change the passcode, update `config/access.code`. Existing referrals stay under the old path, so either move them in the console or let people re-add theirs.
- The quiz itself is public card info bundled in the page, so its lock is a front-end gate. The referral links are what's actually protected.
- To remove a spam or outdated referral, delete it in the Firebase console. The site never allows edits or deletes.

## Updating card data

Card fees and perks are in the `CARDS` and `PERKS` arrays near the top of the script in `index.html`. Each perk's `offers` map is `{ cardId: [estimated $ per year if used, "short detail"] }`. Terms were last checked **Oct 2026**.

## Our referral links & sign-up bonuses

`OUR_LINKS` in `index.html` holds our own referral link for each card (leave `""` to show "Link coming soon"). `OFFERS` holds each card's public sign-up bonus, copied from the issuer's card page, and `OFFERS_AS_OF` is the date shown under them. Issuer sites block in-browser fetching, so bonuses don't update by themselves: check the Chase and Amex pages (linked from each card) and edit `OFFERS` + `OFFERS_AS_OF` when they change. Amex offers vary by applicant, so they're shown as "Up to".
