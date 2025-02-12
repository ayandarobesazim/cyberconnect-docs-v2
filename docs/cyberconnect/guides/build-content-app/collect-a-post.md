---
id: collect-a-post
title: Collect a post
slug: /how-to/build-content-app/collect-a-post
sidebar_label: Collect a post
sidebar_position: 6
description: How to build content app - collect a post
---

In this section you'll learn how to implement the [Collect Essence](/api/content/essence/collect-essence) feature. Previously you've learned that creating a post means registering an essence, but the process of minting and transferring the NFT is actually executed when a user collects the post.

## GraphQL mutations

To collect an essence, meaning to collect a post, is a two step process and requires two GraphQL mutations: `CreateCollectEssenceTypedData` and `Relay`.

1. `CreateCollectEssenceTypedData` is used to present data to the user in a readable format:

```tsx title="graphql/CreateCollectEssenceTypedData.ts"
import { gql } from '@apollo/client'

export const CREATE_COLLECT_ESSENCE_TYPED_DATA = gql`
  mutation CreateCollectEssenceTypedData($input: CreateCollectEssenceTypedDataInput!) {
    createCollectEssenceTypedData(input: $input) {
      typedData {
        id
        sender
        data
        nonce
      }
    }
  }
`
```

2. `Relay` is responsible for broadcasting the transaction, minting and transferring the NFT:

```tsx title="graphql/Relay.ts"
import { gql } from '@apollo/client'

export const RELAY = gql`
  mutation Relay($input: RelayInput!) {
    relay(input: $input) {
      relayActionId
    }
  }
`
```

3. `RelayActionStatus` is used to get the relaying status:

```tsx title="graphql/RelayActionStatus.ts"
import { gql } from '@apollo/client'

export const RELAY_ACTION_STATUS = gql`
  query RelayActionStatus($relayActionId: ID!) {
    relayActionStatus(relayActionId: $relayActionId) {
      ... on RelayActionStatusResult {
        txHash
        txStatus
      }
      ... on RelayActionError {
        reason
        lastKnownTxHash
      }
      ... on RelayActionQueued {
        queuedAt
      }
    }
  }
`
```

## Collect a Post

Now that you set up the APIs required, you can implement the Collect feature. The approach is almost exactly the same as it was for [Subscribe to Profile](/how-to/build-content-app/subscribe-to-profile):

1. Get data in a readable format and the `typedDataID` for it;
2. Get the user to sign the message data and get its `signature`;
3. Call the `relay` and pass it the `typedDataID` and `signature`;

```tsx title="components/CollectBtn.tsx"
/* Create typed data in a readable format */
const typedDataResult = await createCollectEssenceTypedData({
  variables: {
    input: {
      collector: account,
      profileID: profileID,
      essenceID: essenceID,
    },
  },
})

const typedData = typedDataResult.data?.createCollectEssenceTypedData?.typedData
const message = typedData.data
const typedDataID = typedData.id

/* Get the signature for the message signed with the wallet */
const params = [account, message]
const method = 'eth_signTypedData_v4'
const signature = await signer.provider.send(method, params)

/* Call the relay to broadcast the transaction */
const relayResult = await relay({
  variables: {
    input: {
      typedDataID: typedDataID,
      signature: signature,
    },
  },
})
const txHash = relayResult.data?.relay?.relayTransaction?.txHash
```

If the collect process was successful, you can verify the transaction hash on [goerli.etherscan.io](https://goerli.etherscan.io/).

![transaction hash](/img/v2/build-content-app-collect-a-post-tx.png)

You can also view the NFT when a user collects a post on [testnets.opensea.io](https://testnets.opensea.io). You'll notice that the image for the NFT and all other details about it correspond to the details passed to the [Metadata Schema](/how-to/build-content-app/create-a-post#metadata-schema) fields (e.g. `image_data`, `name`, `description`, etc).

![nft essence](/img/v2/build-content-app-collect-a-post-nft.png)

Next up you will dive deep into middlewares and learn how to set them. Let's start with [Middleware for Subscribe](/how-to/build-content-app/middleware-for-subscribe).
