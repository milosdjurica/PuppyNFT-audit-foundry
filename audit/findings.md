### [M-#] Looping over array of players in `PuppyRaffle::enterRaffle` is a potential Denial Of Service (DoS) attack, incrementing gas costs for future entrances.

IMPACT: MEDIUM/HIGH
LIKELIHOOD: MEDIUM

**Description**
The `PuppyRaffle::enterRaffle` loops through the `players` array to check for duplicates. However, the longer the `PuppyRaffle::players` array is, the more checks the new player will have to make. That means the newer players pay higher and higher fees (gas price) than older ones.

```javascript
// @audit DoS Attack
        for (uint256 i = 0; i < players.length - 1; i++) {
            for (uint256 j = i + 1; j < players.length; j++) {
                require(players[i] != players[j], "PuppyRaffle: Duplicate player");
            }
        }
```

**Impact**
The gas cost for raffle entrants will greatly increase as more players enter the raffle. Discouraging later users to enter, and causing a rush at the start to be one of the first entrants.
Attacker might make the `PuppyRaffle::entrants` array so big, that no one else enters, guaranteeing themselves a win.

**Proof Of Concept**
If we have 2 sets of 100 players, the gas costs will be:

- 1st 100 players : ~6252047gas
- 2nd 100 players : ~18068137

This is more than 3x more expensive for the 2nd 100 players.

PoC -> Check the following test -> `PuppyRaffleTest.t.sol::test_enterRaffle_DenialOfService_AttackProof()`

**Recommended Mitigation**
There are a few recommendations.

1. Consider allowing duplicates. Users can make new addresses anyway, so a duplicate check doesn't prevent the same person from entering multiple times.
2. Consider using mapping to check for duplicates.
3. Alternatively, you could use OpenZeppelin's EnumerableSet Library.
