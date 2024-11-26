# crypto/rubix-cube

We are presented with the following code:

```
from Crypto.Util.number import *

bit_length = 256

p = getPrime(bit_length)
q = getPrime(bit_length)

n = p*q
e = 3

t = (p-1)*(q-1)
d = pow(e,-1,t)

FLAG = b"cyber{REDACTED}"
# len(FLAG) = 19
m = bytes_to_long(FLAG)

c = pow(m,e,n)

print(n)
# 6278037311384915135452964844708138627361730188752003745357301334279851353282124691789127462298533821912021385379022775606478845581097315496944821799543681
print(c)
# 10916638714479161939606357984458637892891782630077289045596358028832248407366786075041059162925173489817133372444440510505884485989248613


# NOTE: THIS FUNCTION MAY BE USEFUL
def find_invpow(x,n):
    """Finds the integer component of the n'th root of x,
    an integer such that y ** n <= x < (y + 1) ** n.
    """
    high = 1
    while high ** n <= x:
        high *= 2
    low = high // 2
    while low < high:
        mid = (low + high) // 2
        if low < mid and mid**n < x:
            low = mid
        elif high > mid and mid**n > x:
            high = mid
        else:
            return mid
    return mid + 1
```

This code is performing RSA with an exponent of 3, making it a small exponent which can be solved by simply using a cube root.

  


  
