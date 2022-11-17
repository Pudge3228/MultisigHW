# MultisigHW
Changed proposing and executing functions
Can't propose to send over 66 eth (this thing is not nessesary)
Can't execute TX if balance changes for more than 66 eth
```
--- a/contracts/MultisigWallet.sol
+++ b/contracts/MultisigWallet.sol
@@ -227,7 +227,7 @@ contract MultiSigWallet {
         confirmed(transactionId, msg.sender)
         notExecuted(transactionId)
     {
-        //uint256 initialBalance = address(this).balance;
+        uint256 initialBalance = address(this).balance;
         if (isConfirmed(transactionId)) {
             Transaction storage txn = transactions[transactionId];
             txn.executed = true;
@@ -238,8 +238,8 @@ contract MultiSigWallet {
                 txn.executed = false;
             }
         }
-        //uint256 postBalance = address(this).balance;
-        //require((initialBalance - postBalance) <= 66000000000000000000, "cant send over 66 eth");
+        uint256 postBalance = address(this).balance;
+        require((initialBalance - postBalance) <= 66000000000000000000, "cant send over 66 eth");
     }

     // call has been separated into its own function in order to take advantage
@@ -294,7 +294,7 @@ contract MultiSigWallet {
         notNull(destination)
         returns (uint transactionId)
     {
-        //require(value <= 66000000000000000000, "cant send over 66 Eth");
+        require(value <= 66000000000000000000, "cant send over 66 Eth");
         transactionId = transactionCount;
         transactions[transactionId] = Transaction({
             destination: destination,
```
