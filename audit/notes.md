<!-- ? Questions??? -->

1. Can add yourself multiple time or not? Can have duplicates or no
2. What is `total fees` used for?
3. `raffleDuration` can be immutable?
4. Should event parameters be indexed?
5. Is this a problem??? Player 1 adds player1 and player2 into raffle and he pays tickets for both. Player2 refunds his ticket and gets money that player1 paid for ticket???
6. Should Organizer take 20% of all money??? Maybe should make constant for this
7. Who and when is calling `withdrawFees` ???
8. Should owner be `feeAddress` or deployer?

<!-- ! Issues -->

1. Weird compiler version and should be limited to only 1 version
2. Confusing contract comment -> multiple times and duplication
3. `enterRaffle()` should be external
4. `require(msg.value == entranceFee * newPlayers.length, "PuppyRaffle: Must send enough to enter raffle");` -> To be changed to -> "PuppyRaffle: Must send exact price of the tickets"
5. Try to change architecture to not use arrays, and when using, should iterate more efficiently
6. `refund()` should be external
7. `players[playerIndex] = address(0);` -> should not put address(0), because later it can get picked as a winner. Should be deleted from array
8. `getActivePlayerIndex()` -> inefficient for loop
9. It also returns 0 if player is not active. It could create problem if player on index 0 is active
10. `selectWinner()` -> first line -> Better description -> Not enough time passed
11. Using bad randomness for picking winner
12. `totalAmountCollected is calculated badly because there can be address(0) that refunded money`
13. reentrancy in select winner?
14. `withdrawFees()` It is not automated, and with the way first require statement is placed, it can get into a problem when owner doesn't pull out funds after 1 lottery iteration
15. Doesn't need -> `uint256 feesToWithdraw = totalFees` , can just use address(this).balance and withdraw that
16. `_isActivePlayer()` -> internal function that is never called
17. `_baseURI()` should not be a function but a constant
18. Check `tokenURI()`, not sure if it is good
19. DoS Attack -> higher number of players it is more gas expensive for user to enter lottery and eventually wont be able to join. BECAUSE OF FOR LOOPS
20. Use ++i in For Loops

<!-- TODO Info -->

1.  Naming convention for immutable and constants
2.  Change to custom errors for better gas optimization
3.  `totalFees = totalFees + uint64(fee);` -> Can make it += for better readability
