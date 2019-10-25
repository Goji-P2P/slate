# Settlement

The Settlement API allows the investment manager to record trades that have been executed.

## Recording available products

The investment manager must record what investments are going to be available to investors and whether
they will be made available in the ISA.

## Making an investment

The investment manager will periodically record an investment in the Goji platform via the API. This will trigger a
record of the investment against the investors' accounts and also move the funds from the client money account to the investment manager.

![](/images/settlement/images/client-money-investment.png "")

## Bond repayments

The investment manager will calculate repayments and any withholding tax that are due to the investors in the bond. The investment manager will
record the due repayment via the API. Once the funds are received, they will be distributed to the appropriate investors and any tax or fees specified will
be returned to the investment manager to the specified payment destination

![](/images/settlement/images/client-money-repayment.png "")

## Bond maturity

When the bond matures, the investment manager will calculate repayments that are due to the investors in the bond. The investment manager will
record the due repayment via the API. Once the funds are received, they will be distributed to the appropriate investors and any tax or fees specified will
be returned to the investment manager to the specified payment destination.

![](/images/settlement/images/client-money-maturity.png "")

## Secondary market

When the investment manager wishes to transfer ownership of a loan (or part thereof), the sale is recorded via the API
to the Goji platform. If appropriate funds are available in the buyer's account in the Goji platform, the appropriate
transactions are recorded and funds are moved to the seller's account.

![](/images/settlement/images/client-money-secondary-market.png "")
