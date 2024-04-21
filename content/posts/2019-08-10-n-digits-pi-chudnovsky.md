---
title: 'Calculating n digits of pi using Chudnovsky Algorithm'
date: 2019-08-10T00:00:00-07:00
tags: ["algorithm"]
summary: "Chudnovsky Algorithm for calculating π to N digits precision, along with a simple CLI for testing"
---

Today I stumbled upon [Chudnovsky Algorithm](https://en.wikipedia.org/wiki/Chudnovsky_algorithm) to
calculate the value of π to N digits of precision. Python code for this algorithm looks like 
the following:

```python
import decimal

def compute_pi(n):
    decimal.getcontext().prec = n + 1
    C = 426880 * decimal.Decimal(10005).sqrt()
    K = 6.
    M = 1.
    X = 1
    L = 13591409
    S = L

    for i in range(1, n):
        M = M * (K ** 3 - 16 * K) / ((i + 1) ** 3)
        L += 545140134
        X *= -262537412640768000
        S += decimal.Decimal(M * L) / X
    
    pi = C / S
    return pi
```

I wrote a simple CLI to test this out:

```python
#!/usr/bin/env python3

import argparse
from lib import compute_pi

parser = argparse.ArgumentParser(description='Print n digits of pi.')
parser.add_argument('num_digits', metavar='N', type=int, help='no. of digits of pi to print')
args = parser.parse_args()

print(compute_pi(args.num_digits))
```

Here's the CLI in action:

```
> ./pi.py 30
3.141592653589741586517727315459

> ./pi.py 300
3.141592653589741586517727315457865035780252691261563179943288214795808630531389642185274931230804430454419117074147967105366083976712333542218321180274249883145873143454428446008580088034341219473373000151443532721504141865178673966393142941520166862874509797611548477147655085787688540025728361617601
```
