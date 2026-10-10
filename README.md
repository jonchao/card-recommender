# Card Picker

A passcode-protected credit card recommender. Visitors mark perks as **Use / Maybe / Wouldn't use**. The site then ranks 9 cards and suggests same-ecosystem combos:

- Sapphire Reserve and Sapphire Preferred
- Amex Platinum and Gold
- Venture X and Venture
- Freedom Unlimited and Freedom Flex
- Savor

An **Our links** tab lists our own referral links and each card's current sign-up bonus, grouped by bank.

Everything lives in `index.html` (plus bank logos in `logos/`), with no build step. It's hosted on GitHub Pages; Firestore only checks the passcode.

## One-time setup

1. **Create a Firebase project** at https://console.firebase.google.com (Analytics not needed).
2. **Create a Firestore database:** Build → Firestore Database → Create database (production mode).
3. **Register a web app:** Project settings → Your apps → Web (`</>`). Copy the `firebaseConfig` object into `index.html`, replacing the `REPLACE_ME` block.
4. **Set the passcode:** in the Firestore data tab, create collection `config` → document `access` → field `code` (string) = your passcode.
5. **Publish the rules:** paste `firestore.rules` into Firestore → Rules and click Publish.
6. **Host it:** push this folder to a GitHub repo, then Settings → Pages → Deploy from branch `main` / root.

## How the passcode works

- On unlock, the page reads `sites/{passcode}/referrals`. The rules only open the path whose name equals `config/access.code`, so that read succeeds only with the right passcode.
- A wrong passcode reads nothing.
- To change the passcode, update `config/access.code`. Everyone has to enter the new one.
- Everything shown on the site, including our referral links, is in the page code, so the passcode is a front-end gate and the public GitHub repo can be read by anyone.

## Updating card data

Card fees and perks are in the `CARDS` and `PERKS` arrays near the top of the script in `index.html`. Each perk's `offers` map is `{ cardId: [estimated $ per year if used, "short detail"] }`. Terms were last checked **Oct 2026**.

## Our referral links & sign-up bonuses

`OUR_LINKS` in `index.html` holds our own referral link for each card (leave `""` to show "Link coming soon"). `OFFERS` holds each card's public sign-up bonus, copied from the issuer's card page, and `OFFERS_AS_OF` is the date shown under them. Issuer sites block in-browser fetching, so bonuses don't update by themselves: check the Chase and Amex pages (linked from each card) and edit `OFFERS` + `OFFERS_AS_OF` when they change. Amex offers vary by applicant, so they're shown as "Up to".

Bank logos (`logos/chase.svg`, `logos/amex.svg`) come from Wikimedia Commons and are trademarks of their owners, used only to identify each card's issuer.
