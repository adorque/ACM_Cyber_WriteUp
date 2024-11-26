# crypto/rubix-cube
Tags: cyber-academy, week 9, modern-cryptography 2024

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
## About RSA

With the Rivest-Shamir-Adleman (RSA) algorithm, a key is encrypted with the use of public keys e (usually 65537) and n, a private key d, and a modulus n. This is encrypted with the equation:

(m^e)^d % n = message

## Method to solve CTF
I notice that if the exponent of the message is a small enough number, the nth root of the encrypted message can simply be taken to find the original message. This code performs RSA with an exponent of 3, making it a small exponent that can be solved by simply using a cube root. By identifying the value of the exponent, we notice that the message can be recovered simply by taking the cube root of the printed value. 

I used the function find_invpow which finds the nth root of a number. Using this function to take the cube root of the encrypted message, I converted the returned long value to bytes:

```
10916638714479161939606357984458637892891782630077289045596358028832248407366786075041059162925173489817133372444440510505884485989248613
ans = long_to_bytes(find_invpow(c, 3)) 
```
Then the following flag was returned:
```
cyber{r00t_my_cub3}'
```



  


  
