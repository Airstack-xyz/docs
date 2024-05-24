# ProfileFilter

## Example

```json
{
  "userId": "3761",
  "userAddress": "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
  "userAssociatedAddresses": [
    "0xa3a736379bfb4a9748c3e4daf5f9af0edfcc6c1a",
    "Cry3MBYMS34p67Tr6pmart85JuTpdULhgDtaV8rxadng",
    "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879"
  ],
  "profileName": "tokenstaker.eth",
  "userRecoveryAddress": "0x00000000fcb080a4d6c39a9354da9eb9bc104cd7",
  "isFarcasterPowerUser": false,
  "followingCount": 117,
  "followerCount": 108,
  "socialCapital": {
    "socialCapitalScore": 0.00151243059,
    "socialCapitalRank": 1512
  }
}
```

## Params

|                           |                                                |                                                                        |
| ------------------------- | ---------------------------------------------- | ---------------------------------------------------------------------- |
| `userId`                  | `Int`                                          | Only get profile events (creation/update) from the specified user FID. |
| `userAddress`             | `String`                                       | User's custody address.                                                |
| `userAssociatedAddresses` | `String[]`                                     | User's connected & custody addresses.                                  |
| `profileName`             | `String`                                       | User's profile name.                                                   |
| `userRecoveryAddress`     | `String`                                       | User's recovery address.                                               |
| `isFarcasterPowerUser`    | `Boolean`                                      | Indicates whether a user has power badge or not.                       |
| `followingCount`          | `Int`                                          | Number of followings.                                                  |
| `followerCount`           | `Int`                                          | Number of followers.                                                   |
| `socialCapital`           | [`SocialCapital`](../objects/socialcapital.md) | Social capital score & rank associated to the Farcaster user.          |
