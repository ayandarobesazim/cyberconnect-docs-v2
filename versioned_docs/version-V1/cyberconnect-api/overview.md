---
id: overview
title: Overview
slug: /cyberconnect-api/overview/
sidebar_label: Overview
sidebar_position: 1
description: In this section, you’ll find all the information you need to query data from the CyberConnect Protocol.
---

In this section, you’ll find all the information you need to query data from the CyberConnect Protocol.

## GraphQL API

You can access a variety of data about users, their identities, and their connections by using the GraphQL API. From NFT ownership to customizable recommendations for new connections, you can extract data with a simple query and start building meaningful social experiences for your users from here.

| Method                                                                | Description                                                                                                                                                            |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`identity`](/V1/cyberconnect-api/graphql-api/identity)               | This method returns a user’s identity information including their followers, followings, friends, Twitter handle, avatar, etc.                                         |
| [`connections`](/V1/cyberconnect-api/graphql-api/connections)         | This method returns a list of [connection](/V1/concepts/connection/) details between a given address and a list of other addresses.                                    |
| [`recommendations`](/V1/cyberconnect-api/graphql-api/recommendations) | This method returns a list of a user’s potential connections based on a variety of recommendation algorithms weighted by factors such as their connections and assets. |
| [`nftOwners`](/V1/cyberconnect-api/graphql-api/nftOwners)             | This method takes in an NFT contract address and token ID as input and returns its owner’s identity information.                                                       |
| [`rankings`](/V1/cyberconnect-api/graphql-api/rankings)               | This method returns a list of ranked users based on their follower count.                                                                                              |

## REST API

You can access a variety of data about user profiles and avatars on Ethereum by using the REST API.

| Method                                              | Description                                                                                                 |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| [`profile`](/V1/cyberconnect-api/rest-api/profile/) | This method takes in an Ethereum address or ENS name as input and returns its profile metadata information. |
| [`avatar`](/V1/cyberconnect-api/rest-api/avatar/)   | This method takes in an Ethereum address or ENS name as input and returns its avatar.                       |

## How does it work?

The indexer layer of the CyberConnect Protocol collects on-chain data from blockchain networks (e.g. Ethereum, Solana) as well as off-chain data from IPFS and web2 social platforms. The indexer then transforms and stores the raw data into graph databases. Developers can access the data and customize their queries using the GraphQL API. Read more about the [CyberConnect Indexer](/V1/protocol/cyberconnect-indexer/) and its technical architecture.
