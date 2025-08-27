+++
title = 'Fixing Excess IRA Contributions'
date = 2023-04-08T00:00:00-00:00
lastmod = 2025-08-27T00:00:00-00:00
draft = false
description = '''
My experience with overcontributing to my IRA and how I fixed it.
'''
tags = ['finance']
+++

Saturday, April 8th, 2023. It's a beautiful spring day–sunny, mid-sixties. I am
in my office poring over financial documents. Why you might ask? Because
I overcontributed to my IRAs.

## My financial history

Like a true financial wizard (or rube?) I have a mix of 7 IRA accounts (3
traditional, 4 Roth) across 4 financial institutions (Vanguard, Merrill Edge,
Principal Financial, Fidelity). Why do I have so many accounts? I opened my
first Roth IRA in 2016 and have been steadily contributing to it for several
years. In addition, I have worked at 3 different companies since 2018. During
my employment at each company I contributed to both their 401(k) and Roth IRA
retirement plans. After leaving each company, I rolled my 401(k) into
a Rollover IRA (traditional IRA) and began contributing \$100 to both this and
the Roth IRA every month.

Upon reaching a total monthly contribution of \$800, things started to unravel,
as I was quickly exceeding my IRA contribution limits for the year. This, in
addition to the fact that we (my wife and I) blew past the IRA contribution
income limit in 2022.

This article details my research and remediation of my excess IRA contributions
dating back to 2021.

## What is an IRA?

An IRA, or Individual Retirement Account, is a type of investment account that
is designed to help individuals save money for retirement. There are two main
types of IRAs: *traditional* and *Roth*.

- With a traditional IRA, contributions to the account may be tax-deductible in
  the year they are made, and the investment earnings grow tax-deferred until
  they are withdrawn. However, when withdrawals are made in retirement, they
  are taxed as ordinary income.

- With a Roth IRA, contributions are made with after-tax dollars, but the
  investment earnings grow tax-free and qualified withdrawals in retirement
  are also tax-free.

Additionally, there are Rollover IRAs. A Rollover IRA is a type of traditional
IRA. It is typically created by rolling over funds from a qualified retirement
plan, such as a 401(k), into a traditional IRA. The rollover is usually done to
avoid penalties and taxes that would otherwise apply if the funds were
withdrawn from the qualified plan. Like other traditional IRAs, withdrawals
from a Rollover IRA are taxed as ordinary income.

## Contribution limits for IRAs

For both 2021 and 2022, the [contribution limit for traditional and Roth
IRAs][contribution limit for traditional and Roth IRAs] was \$6,000 for
individuals under age 50, and \$7,000 for those age 50 and older, known as the
catch-up contribution. It should be noted that in 2023, the contribution limit
is \$6,500 for individuals under age 50, and \$7,500 for those age 50 and
older.

In addition to the general contribution limit that applies to both Roth and
traditional IRAs, your Roth IRA contribution may be limited based on your
filing status and income. In 2021 and 2022, I filed jointly with my wife,
therefore the following limits applied:

| Year | Up to Limit    | Reduced Amount                  | Zero          |
| ---- | -------------- | ------------------------------- | ------------- |
| 2021 | &lt;\$198,000  | &ge;\$198,000 and &lt;\$208,000 | &ge;\$208,000 |
| 2022 | &lt;\$204,000  | &ge;\$204,000 and &lt;\$214,000 | &ge;\$214,000 |

It should be noted that these amounts use [Modified Adjusted Gross Income
(MAGI)][Modified Adjusted Gross Income (MAGI)].

# Determining total IRA contributions

If one has multiple IRA accounts across multiple financial institutions, how
does the IRS know the total amount you contributed to *all* IRAs? The answer:
[IRS Form 5498][IRS Form 5498].

IRS Form 5498 is a tax form used by IRA custodians, trustees, and issuers to
report contributions, rollovers, conversions, and other information related to
an individual retirement account (IRA).

The form is typically sent to the IRA account holder by May 31st of the year
following the tax year in which the activity occurred. For example, if
contributions were made to an IRA in 2021, the IRA custodian would send Form
5498 to the account holder by May 31st, 2022.

However, and most notably, this form is *also* sent to the IRS by your
financial institution.

## My financial landscape

Gathering my Form 5498s from all my financial institutions for 2020, 2021, and
2022, I could make the following calculations:

### 2020

| Institution         | Account Type | Amount  |
| ------------------- | ------------ | ------- |
| Vanguard            | Roth IRA     | \$2,400 |
| Merrill Edge        | Rollover IRA | \$1,200 |
| Merrill Edge        | Roth IRA     | \$1,200 |
| Principal Financial | Rollover IRA | \$0     |
| Principal Financial | Roth IRA     | \$0     |

**Total**: \$4,800

**Modified AGI**: \$172,033.00

### 2021

| Institution         | Account Type | Amount  |
| ------------------- | ------------ | ------- |
| Vanguard            | Roth IRA     | \$2,400 |
| Merrill Edge        | Rollover IRA | \$1,200 |
| Merrill Edge        | Roth IRA     | \$1,200 |
| Principal Financial | Rollover IRA | \$1,100 |
| Principal Financial | Roth IRA     | \$1,100 |

**Total**: \$7,000

**Modified AGI**: \$188,160.00

**Excess contribution**: \$1,000

### 2022[^1]

| Institution         | Account Type | Amount  |
| ------------------- | ------------ | ------- |
| Vanguard            | Roth IRA     | \$2,400 |
| Merrill Edge        | Rollover IRA | \$1,200 |
| Merrill Edge        | Roth IRA     | \$1,200 |
| Principal Financial | Rollover IRA | \$1,200 |
| Principal Financial | Roth IRA     | \$1,200 |
| Fidelity            | Rollover IRA | \$400   |
| Fidelity            | Roth IRA     | \$400   |

**Total**: \$8,000

**Modified AGI**: \$265,634.00

**Excess contribution**: \$8,000

### 2023

| Institution         | Account Type | Amount |
| ------------------- | ------------ | ------ |
| Vanguard            | Roth IRA     | \$800  |
| Merrill Edge        | Rollover IRA | \$400  |
| Merrill Edge        | Roth IRA     | \$400  |
| Principal Financial | Rollover IRA | \$400  |
| Principal Financial | Roth IRA     | \$400  |
| Fidelity            | Rollover IRA | \$400  |
| Fidelity            | Roth IRA     | \$400  |

**Total**: \$3,200

**Modified AGI**: - (but &gt;\$228,000)

**Excess contribution**: \$3,200

As you can see, my clever strategy of moving companies, making more money, and
increasing my automatic contributions became nonviable in 2021. In addition,
I had also already contributed \$3,200 to my IRA accounts in 2023 even though
our modified AGI far exceeded the limit.

## Taking corrective action

If you have contributed more than the maximum allowable contribution to your
IRAs for the year, you will need to take corrective action to avoid potential
tax penalties.

1. **Adjust your future contributions**  
Since I can no longer make contributions to my IRA accounts, I adjusted my
contribution amounts accordingly (i.e., to zero).

2. **Withdraw the excess contribution**  
It should be noted that if you withdraw an excess contribution and any
associated earnings by the tax-filing deadline for the tax year in which the
excess contribution was made, you will not be subject to the early withdrawal
penalty. Furthermore, if you have multiple IRA accounts and have made an excess
contribution to all of them, you cannot simply withdraw the excess from
a single account to correct the error.

The excess contribution must be corrected across all of your IRA accounts in
proportion to the amount of the excess contribution made to each account. This
is known as a "proportional correction" method. The proportional correction
method ensures that you do not receive a tax benefit from the excess
contribution.

Thus applying the proportional correction method, I had to withdraw the excess
amount from each financial institution:

**2022**
- Vanguard: \$2,400
- Merrill Edge: \$2,400
- Principal Financial: \$2,400
- Fidelity: \$800

**2023**
- Vanguard: \$800
- Merrill Edge: \$800
- Principal Financial: \$800
- Fidelity: \$800

Now, for 2021 it is a little tricky. Basically, the formula is:

```
(Contribution to Account X / Total contribution) x (Excess contribution)
```

So, solving for Vanguard, Merrill Edge, and Principal Financial for 2021:

```
Vanguard = ($2,400 / $7,000) x $1,000 = $342.86
```

```
Merrill Edge = ($2,400 / $7,000) x $1,000 = $342.86
```

```
Principal Financial = ($2,200 / $7,000) x $1,000 = $314.28
```

- Vanguard: \$3,542.86
- Merrill Edge: \$3,542.86
- Principal Financial: \$3,514.28
- Fidelity: \$1,600

**NOTE**: Distributions need to be made per year and per account (first Roth,
then traditional).

I ended up having to call all 4 financial institutions in order to get
clarification on how exactly to code the distribution as an *excess
contribution distribution* as opposed to a normal or premature distribution in
order to avoid paying the early distribution penalty.

For Vanguard, I used their [Fix a contribution to an IRA][Fix a contribution to
an IRA] flow.

Merrill Edge provides the forms for you: **Accounts** > **Transfer Money
& Securities** > **Transfer Case**. Merrill had the worst customer service
/ user experience.

Principal Financial did this all for me, calculating the earnings/losses, etc.
I believe this was just a limitation of their software, since their website
could not provide the required forms in order to process the excess
contribution. I simply gave them the amounts for 2021, 2022, and 2023 and they
processed the request.

For Fidelity, I used their [Return of excess IRA contributions][Return of
excess IRA contributions].

## Additional Notes

- Do not withhold federal taxes. This will be taxed as an early withdrawal.
- The amount of earnings returned to you, if any, will be reported as taxable
  income for the year in which the excess contribution was made. If you are
  younger than age 59 ½, the earnings may be subject to a 10% early
  withdrawal penalty

## Conclusion

Being financially illiterate grants you no special dispensation regarding tax
law. Few people realize that you can be penalized for being financially
negligent, as I was. For example, you are required to take a minimum
distribution each year beginning with the year you turn age 72, in order to
avoid incurring a penalty. This was a wake up call for me. You have to be
diligent with your finances. No one is going to check up on you–except maybe
the IRS :)

## Update: 2025-08-27

It is now August 2025. I have not been audited, so I think I'm good.

[^1]: At the time of this writing, I had not received my Form 5498s, however,
given my past contributions and financial statements, I was able to forecast
the contributions to these accounts to date.

[contribution limit for traditional and Roth IRAs]: https://www.irs.gov/retirement-plans/plan-participant-employee/retirement-topics-ira-contribution-limits
[Modified Adjusted Gross Income (MAGI)]: https://apps.irs.gov/app/IPAR/resources/help/MAGI.html
[IRS Form 5498]: https://www.irs.gov/pub/irs-pdf/f5498.pdf
[Fix a contribution to an IRA]: https://personal1.vanguard.com/eel-esign-summary-webapp/
[Return of excess IRA contributions]: https://www.fidelity.com/retirement-ira/excess-ira-contributions
