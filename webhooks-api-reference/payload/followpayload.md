# FollowPayload

## Example

```json
{
  "id": "000000020c162a7ffcaf089ae12f3e53372302cde3c95cf6bab7b3244df5daa6",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "followerProfileId": "3761"
  "followingProfileId": "326089",
  "followerSince": "2024-02-09T07:37:59Z"
}
```

## Fields

| Name                 | Type              | Description                                                                                                    |
| -------------------- | ----------------- | -------------------------------------------------------------------------------------------------------------- |
| `blockchain`         | `EveryBlockchain` | Blockchain associated with the social identity.                                                                |
| `blockNumber`        | `Int`             | Blocknumber when the follows occured onchain.                                                                  |
| `dappName`           | `String`          | Social DApp name – lens, farcaster                                                                             |
| `dappSlug`           | `String`          | Social DApp slug (contract version) – lens\_polygon, lens\_v2\_polygon, farcaster\_optimism, farcaster\_goerli |
| `followerProfileId`  | `String`          | Follower FID for Farcaster or follower's token ID for Lens.                                                    |
| `followingProfileId` | `String`          | Following FID for Farcaster or following's token ID for Lens.                                                  |
| `id`                 | `ID`              | Airstack Internal ID.                                                                                          |
