# Blacklisting and Whitelisting

The black-and-white list functions provide the DAO, or other trusted projects, a mechanism to blacklist or whitelist certain accounts. For example, certain accounts may not have a way to prove they are legitimate other than being whitelisted. On the flip side we can use the blacklist to block all known Sybil accounts providing a strong deterrent for this type of behavior.

## Endpoints

Other apps or contracts can interact with this module through the following functions of the VePassport contract:

```solidity
  interface IVeBetterPassport {
    ...
    function whitelist(address _user) external;
    function isWhitelisted(address _user) external view returns (bool);
    function removeFromWhitelist(address _user) external;
  
    function blacklist(address _user) external;
    function isBlacklisted(address _user) external view returns (bool);
    function removeFromBlacklist(address _user) external;
    ...
  }
```
