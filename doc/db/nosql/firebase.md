---
title: "Firebase — Google's BaaS (Firestore, Auth, Hosting, Functions)"
main_link: https://firebase.google.com/
keywords: [firebase, firestore, baas, google, realtime-database, auth, vendor-lockin]
status: reviewed
---

# Firebase — Google's BaaS (Firestore, Auth, Hosting, Functions)

**Main link:** <https://firebase.google.com/>

Console (per-project): <https://console.firebase.google.com/> · Firestore docs: <https://firebase.google.com/docs/firestore>

## Summary

Firebase is Google's Backend-as-a-Service (BaaS) platform. Originally just a realtime database, it now bundles Firestore (the modern document database), Authentication, Hosting, Cloud Functions, Cloud Messaging (FCM), Remote Config, Crashlytics, App Check, A/B testing, and a growing pile of ML / Genkit features. From a developer's perspective: you get a scalable backend with auth and storage by writing only client-side code.

## Insight

The actual question with Firebase is never "is it good?" — it usually is — but "how much vendor lock-in are you willing to take in exchange for skipping six months of backend work?"

Where Firebase is the right call:

- **Mobile apps with realtime sync** (chat, collaborative editing, presence, leaderboards). Firestore's real-time listeners are very good and were the original reason the platform existed.
- **MVPs and prototypes.** You can ship a working app with auth, storage, push, and a database in a weekend.
- **Side projects and games.** Spark plan (free tier) is generous; Blaze (pay-as-you-go) scales to a respectable load before you need to think.
- **You're already deeply in GCP.** Firebase integrates cleanly with the broader Google Cloud surface.

Where it bites:

- **Lock-in is real and asymmetric.** Migrating off Firestore means rewriting your data model, your security rules, your client SDK calls, and probably your auth integration. Plan an exit before you commit.
- **Pricing on the Blaze plan can spike** in unexpected ways: per-document-read billing punishes denormalised models and chatty clients. Monitor it.
- **Firestore is a document store**, not relational. Joins, transactions across many documents, and aggregation queries are limited or expensive. If your data is fundamentally relational, you'll fight the model.
- **Security rules** are a DSL you must learn properly, or your "private" data will be world-readable. Default rules in new projects are still a common foot-gun.
- **Vendor risk** — Google has a long history of sunsetting products. Firebase itself looks safe, but specific features (Firebase Hosting redirects, Dynamic Links, etc.) have been deprecated.

Honest alternatives, by motivation:

- **"I want Firebase but on Postgres / open source"** → [Supabase](https://supabase.com/). Postgres + auth + realtime + storage + functions, self-hostable.
- **"I want a single Go binary that does this"** → [PocketBase](https://pocketbase.io/).
- **"I want a self-hosted multi-tenant platform"** → [Appwrite](https://appwrite.io/).
- **"I just want Firestore-shaped storage on AWS"** → DynamoDB + Cognito + AppSync. Different tradeoffs, similar lock-in.

## References / raw notes

The original captured link in this article was a per-project console URL of the form:

```
https://console.firebase.google.com/project/<your-project-id>/settings/integrations/analytics?pli=1
```

That's not a public page — it's the Analytics integration settings inside the Firebase console for a specific project. Useful pages instead:

- Console root: <https://console.firebase.google.com/>
- Pricing: <https://firebase.google.com/pricing>
- Status: <https://status.firebase.google.com/>

## Similar / related topics

- [Supabase](https://supabase.com/) — open-source Firebase-shaped BaaS on top of [[postgresql]].
- [PocketBase](https://pocketbase.io/) — single-binary BaaS in Go with SQLite under the hood.
- [Appwrite](https://appwrite.io/) — self-hosted BaaS, MariaDB-backed.
- [[fauna]] — was the closest "managed serverless DB" alternative; now sunset.
- DynamoDB + AWS Amplify — the AWS-flavoured equivalent.

## Internal links

<!-- reviewed -->

- [[redis]]
- [[postgresql]]
- [[fauna]]

## Keywords

`#firebase` `#firestore` `#baas` `#google-cloud` `#vendor-lockin` `#realtime-db` `#auth` `#db`
