### Common Attacks Vectors in Lending & Borrowing DeFi platforms

Article: https://dacian.me/lending-borrowing-defi-attacks

1. ### Liquidation Before Default:

   The Borrower may face a significant risk of losing funds if their collateral is liquidated by the Lender, Liquidator, or any other market participant before they default.

   1. https://code4rena.com/reports/2022-04-abranft/#h-04-lender-is-able-to-seize-the-collateral-by-changing-the-loan-parameters

   2. https://github.com/sherlock-audit/2023-02-blueberry-judging/issues/126

2. ### Borrower Can't Be Liquidated

   The borrower might have the ability to structure a loan proposal in such a way that it would prevent the liquidation of their collateral.

   1. https://github.com/sherlock-audit/2022-11-isomorph-judging/issues/72
   2. https://code4rena.com/reports/2022-04-abranft/#h-01-avoidance-of-liquidation-via-malicious-oracle

3. ### Closing of Debt Without Repayment

   The Lender becomes financially vulnerable to a significant loss of funds if the Borrower manages to settle the debt without fully paying off the outstanding amount and retaining possession of their collateral.

   1. https://github.com/sherlock-audit/2023-03-taurus-judging/issues/11
   2. https://github.com/sherlock-audit/2022-10-astaria-judging/issues/233

4. ### Repayment is Paused but Liquidation is Enabled

   It is unfair for Borrowers to be prevented from making repayments while still being subjected to liquidation on DeFi platforms for lending and borrowing. Therefore, if there is a possibility of pausing repayments, liquidations should also be paused simultaneously to maintain fairness.

   1. https://github.com/sherlock-audit/2022-11-isomorph-judging/issues/69

5. ### Disallowing Token Might Stop Existing Repayment & Liquidation

   Lending and Borrowing platforms have the capability to prevent the acceptance of previously approved tokens for repayment or collateral through governance. However, if this action also prevents loans that are currently using those tokens from being repaid or liquidated, it can create a significant risk of loss of funds vulnerability for both the Lender and/or the protocol.

   1. https://github.com/sherlock-audit/2022-11-isomorph-judging/issues/57
   2. https://github.com/sherlock-audit/2023-02-gmx-judging/issues/168

6. ### Liquidator Takes Collateral With Insufficient Repayment

   If the borrower defaults on the loan, there are two possible outcomes: either the lender seizes the collateral and forgives the repayment, or a liquidator repays the borrower's debt and takes the collateral. The latter option is available on some advanced platforms, where the liquidator can partially repay the borrower's bad debt and receive a corresponding portion of the collateral. However, if the liquidator can seize the collateral without making sufficient or any repayment, this exposes the lender to a significant risk of losing funds.

   - https://github.com/sherlock-audit/2023-02-blueberry-judging/issues/127

7. ### Infinite Loan Rollover

   The Lender needs to have the ability to restrict rollovers for the Borrower, either by setting limits on the number of times or the duration of rollovers, or by implementing other parameters. If the Borrower has unlimited rollover options, it poses a significant risk for the Lender as it may result in a loss of funds, and they may not be able to recover their collateral or receive repayment from the Borrower.

   - https://github.com/sherlock-audit/2023-01-cooler-judging/issues/215

8. ### Repayment Sent to Zero Address

   Care must be taken when implementing the repayment code such that the repayment is not lost by sending it to the zero address.

   - https://github.com/sherlock-audit/2022-11-bullvbear-judging/issues/127

9. ### Borrower Permanently Unable To Repay Loan

   Developers and auditors must ensure that the Borrower is able to repay loans at different stages of the loan, including active and overdue, unless the loan has been liquidated. This is because if the system enters a state where the Borrower cannot repay their loan because the repay() function reverts, it could result in a critical loss of funds vulnerability for both the Borrower and the Lender. The Borrower may lose their collateral and be liquidated, while the Lender will never receive repayment.

- https://github.com/sherlock-audit/2022-10-union-finance-judging/issues/133

10. ### Borrower Repayment Only Partially Credited

    The borrowing and lending systems enable borrowers to take out multiple loans, and they can make an effort to repay as much as they can with one repayment call to the "repay()" function. The underlying concept is that if the repayment amount can pay off the first loan, then the same amount should be used to pay off the second loan, and so on. However, if the overflow amount is not used to at least partially pay off the second loan after the first loan is paid off, the borrower may suffer a critical loss of funds error, and the lender may receive the full amount, resulting in only partial credit for the borrower's repayment.
    To ensure that the bulk repayment functionality indeed pays off as many loans as possible and that none of the repayment amount is lost, developers should perform testing, and auditors should verify the system's functionality.

    - https://github.com/sherlock-audit/2022-10-astaria-judging/issues/190
