---
id: post-comment
title: Post/Comment
slug: /core-concepts/post-comment
sidebar_label: Post/Comment（Content📝）
sidebar_position: 6
description: Post
---

# Post/Comment vs. Essence

Post and comment are different forms of content supported by the protocol, but unlike an EssenceNFT the post and comment do not require any on-chain transaction and are not represented by NFTs. Instead, they are implemented through off-chain proofs that are synced to Arweave. Developers can use Post/Comment API to manage the user’s social data such as comments, moments, etc. This is great for applications with lightweight social content needs, that do not want to incur gas costs.

## Idempotent Proof of Content

Social platforms like Twitter have millions of new content generated by users every single day. Therefore, the data standard has to be efficient and good to work with at the same time to support scaling.

We use idempotent proof to describe the most up-to-date state for content published by an address. The benefit of using idempotency is for easy conflict resolution without an ACID guarantee. We describe the social content using the final desired state at that timestamp.

The proof content includes the following details of the desired state between an originator and published content:

```ts
type Proof = {
  content: string;
  digest: string;
  signature: string;
  signingKey: string;
  signingKeyAuth: SigningKeyAuth;
  arweaveTxHash: string;
};

type SigningKey = {
  publicKey: string;
  format: "SubjectPublicKeyInfo";
  algorithm: "ES256";
};

type SigningKeyAuth = {
  address: string;
  message: string;
  signature: string;
};
```

We only describe these data standards in the raw object format. However, the final message would be encoded with both a digest of the message and a signature signed by the owner.
