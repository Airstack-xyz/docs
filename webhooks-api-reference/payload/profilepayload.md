# ProfilePayload

## Example

```json
{
  "id": "b9f62fdc493d1e0033b79dee9102fbe9f72e820e8acece4b6b9d21b2a4240b31",
  "eventTimestamp": "2024-05-11T08:43:50.531Z",
  "dappName": "farcaster",
  "dappSlug": "farcaster_v3_optimism",
  "blockchain": "optimism",
  "userId": "3761",
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "isDefault": false,
  "profileName": "tokenstaker.eth",
  "profileTokenId": "3761",
  "profileTokenIdHex": "0x0eb1",
  "profileTokenAddress": "0x00000000fc6c5f01fc30151999387bb99a9f489b",
  "profileCreatedAtBlockNumber": 111894270,
  "profileCreatedAtBlockTimestamp": "2023-11-07T20:01:57Z",
  "profileTokenUri": "",
  "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
  "profileMetadata": null,
  "coverImageURI": "",
  "twitterUserName": "",
  "location": "",
  "website": "",
  "profileImageContentValue": {
    "image": {
      "extraSmall": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/extra_small.jpg",
      "large": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/large.jpg",
      "medium": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/medium.jpg",
      "original": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/original_image.jpg",
      "small": "https://assets.airstack.xyz/v2/image/social/10/IucLiJGfa/N17Nm7Vg9Ds6fIORR8W0IVmtDidK4ZGqvdcoSEMLGd0hzozjsSu2K3fkKSNwU8TPCOWo90zOmFgf4veZz99NWWdNVh2zzIB8OKt9zk1iNhXY/qBGfpFdZra1uD3yg84WLFLK1gKfQ2nQ==/small.jpg"
    }
  },
  "isFarcasterPowerUser": false,
  "followingCount": 117,
  "followerCount": 108,
  "profileBio": "Building Airstack",
  "profileDisplayName": "Sarvesh",
  "profileImage": "https://i.imgur.com/Y1au7ZB.jpg",
  "socialCapital": {
    "socialCapitalScore": 0.00151243059,
    "socialCapitalScoreRaw": "151243059"
  }
}  
```

## Fields

## Outputs

| Name                              | Type                                                             | Description                                                                                                                                                                                                                                      |
| --------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `blockchain`                      | `Blockchain`                                                     | Blockchain associated with the social identity                                                                                                                                                                                                   |
| `dappName`                        | `SocialDappName`                                                 | Either `farcaster` or `lens`                                                                                                                                                                                                                     |
| `dappSlug`                        | `SocialDappSlug`                                                 | Social DApp slug (contract version) with these values: `farcaster_optimism`, `farcaster_goerli`,  `lens_polygon`, `farcaster_v2_optimism`, `lens_v2_polygon`                                                                                     |
| `followerCount`                   | `Int`                                                            | Total number of followers                                                                                                                                                                                                                        |
| `followingCount`                  | `Int`                                                            | Total number of followings                                                                                                                                                                                                                       |
| `id`                              | `ID`                                                             | Airstack unique identifier for the data point                                                                                                                                                                                                    |
| `isDefault`                       | `Boolean`                                                        | Whether a social profile is default/primary or not, only apply to Lens, Farcaster always `false`.                                                                                                                                                |
| `isFarcasterPowerUser`            | `Boolean`                                                        | Whether a social profile is has a power badge or not.                                                                                                                                                                                            |
| `profileCreatedAtBlockNumber`     | `Int`                                                            | Block number when the social profile was created.                                                                                                                                                                                                |
| `profileCreatedAtBlockTimestamp`  | `Time`                                                           | Timestamp when the social profile was created.                                                                                                                                                                                                   |
| `profileBio`                      | `String`                                                         | Social profile bio on Farcaster or Lens.                                                                                                                                                                                                         |
| `profileDisplayName`              | `String`                                                         | Social profile display name on Farcaster or Lens.                                                                                                                                                                                                |
| `profileImage`                    | `String`                                                         | Link to Social profile image.                                                                                                                                                                                                                    |
| `profileName`                     | `String`                                                         | Farcaster profile name (fname primarily used) or Lens profile name (e.g. `lens/@betashop9`)                                                                                                                                                      |
| `profileTokenAddress`             | `Address`                                                        | <p>For Farcaster, it is going to return the IdRegistry contract address on Optimism.<br><br>For Lens, it will return Lens profile NFT contract address on Polygon.</p>                                                                           |
| `profileTokenId`                  | `String`                                                         | <p>For Farcaster, it's the user's FID.<br><br>For Lens, it's the user's profile NFT's token ID.</p>                                                                                                                                              |
| `profileTokenIdHex`               | `String`                                                         | Return the hex value of `profileTokenId`                                                                                                                                                                                                         |
| `profileTokenUri`                 | `String`                                                         | <p>Token URI of Lens Profile NFT.<br><br>For Farcaster, this will return empty string.</p>                                                                                                                                                       |
| `socialCapital`                   | [`SocialCapital`](../../api-references/objects/socialcapital.md) | Farcaster user's [social capital score](../../abstractions/trending-casts/social-capital-value-and-social-capital-scores.md).                                                                                                                    |
| `userAddress`                     | `Address`                                                        | User's custody address.                                                                                                                                                                                                                          |
| `userAssociatedAddresses`         | `[Address!]`                                                     | <p>All addresses associated with the social profiles.<br><br>For Farcaster, it will return both the custody and connected addresses.<br><br>If you are only fetching connected addresses, use <code>connectedAddresses</code> field instead.</p> |
| `userCreatedAtBlockNumber`        | `Int`                                                            | Block number when the social profile was created.                                                                                                                                                                                                |
| `userCreatedAtBlockTimestamp`     | `Number`                                                         | Timestamp when the social profile was created.                                                                                                                                                                                                   |
| `userHomeURL`                     | `String`                                                         | Social Profile Home URL.                                                                                                                                                                                                                         |
| `userId`                          | `String`                                                         | <p>For Farcaster, it's the user's FID.<br><br>For Lens, it's the user's profile NFT's token ID.</p>                                                                                                                                              |
| `userLastUpdatedAtBlockNumber`    | `Int`                                                            | Block number when the social profile was last updated.                                                                                                                                                                                           |
| `userLastUpdatedAtBlockTimestamp` | `Number`                                                         | Timestamp when the social profile was last updated.                                                                                                                                                                                              |
| `userRecoveryAddress`             | `Address`                                                        | Farcaster user's recovery address. For Lens, it will return empty string.                                                                                                                                                                        |
| `profileMetadata`                 | `Map`                                                            | This will return `null` for Farcaster profiles.                                                                                                                                                                                                  |
| `coverImageUri`                   | `String`                                                         | URI to the cover image of a social profile.                                                                                                                                                                                                      |
| `twitterUserName`                 | `String`                                                         | Social profile's twitter (from Lens).                                                                                                                                                                                                            |
| `website`                         | `String`                                                         | Social profile's website.                                                                                                                                                                                                                        |
| `location`                        | `String`                                                         | Social profile's location.                                                                                                                                                                                                                       |
| `profileImageContentValue`        | `Media`                                                          | **Nested Query** – resized profile images.                                                                                                                                                                                                       |
