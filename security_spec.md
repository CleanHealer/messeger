# Security Spec

## 1. Data Invariants
- A `User` profile must have an ID matching the authenticated user's UID. Only the user can update their own profile.
- A `Channel` must have a `createdBy` field matching the user who creates it.
- A `Message` can only be created by an authenticated user, and its `userId` must match the user's UID. Messages cannot be modified or deleted once created, or maybe they can be edited by the author. Let's make them immutable for simplicity.

## 2. The "Dirty Dozen" Payloads
1. Create user profile with mismatched UID (Identity) -> `users/userA` with `auth.uid == userB`.
2. Create channel with `createdBy` set to someone else.
3. Update channel description as a non-creator (should be allowed for public channels? Let's say only creator can update, or anyone can create channels but no one can edit them).
4. Create message with `userId` spoofing another user.
5. Provide a 1MB string for a message text (Resource Exhaustion).
6. Update a message with a new `createdAt` timestamp (Immutability).
7. Delete someone else's message.
8. Read channels without being signed in (Anonymous Read/Write).
9. Create message without a `createdAt` matching `request.time`.
10. Update a user profile injecting a "role" field (State/Schema integrity).
11. Update another user's profile.
12. Fetch all user profiles (List Users) as unauthenticated.

## 3. The Test Runner
A `firestore.rules.test.ts` will verify these using `@firebase/rules-unit-testing`.
