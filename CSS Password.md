> My web developer friend said JavaScript is insecure so he made a password vault with CSS. Can you find the password to open the vault?
> 
> Wrap the flag in `uoftctf{}`
> 
> Make sure to use a browser that supports the CSS `:has` selector, such as Firefox 121+ or Chrome 105+. The challenge is verified to work for Firefox 121.0.
> 
> Author: notnotpuns

You are given an html file that has 19 "bytes", each with 8 "bits" where the user can set each bit from 0 to 1 using CSS. Additionally, as the site tells us, if the five circles on the bottom of the site are green, we have successfully found the password.

Inspecting the CSS of the site yields the fact that each CSS LED with class .checker (which are green) is covered by numerous red  LEDS with class .checker__state: 
![[Pasted image 20240113221224.png]] 
For every correct bit, one of these red checker_state elements gets translated off the green checker with the following css: 
```
        /* b3_4_l5_c13 */
        .wrapper:has(.byte:nth-child(3) .latch:nth-child(4) .latch__reset:active) .checker:nth-of-type(6) .checker__state:nth-child(13) {
            transform: translateX(0%);
            transition: transform 0s;
        }

        .wrapper:has(.byte:nth-child(3) .latch:nth-child(4) .latch__set:active) .checker:nth-of-type(6) .checker__state:nth-child(13) {
            transform: translateX(-100%);
            transition: transform 0s;
        }
```

e.g the above example corresponds to the 3rd byte, 4th bit being changed to a 1. 

Once this is understood, we only have the long and tedious task of changing every bit according to the css. Once we have done so, all the LED checkers should be green, and we get the following binary: 
```
1 - 01000011
2 - 01110011
3 - 01010011
4 - 01011111
5 - 01101100
6 - 00110000
7 - 01100111
8 - 00110001
9 - 01100011
10- 01011111
11- 01101001
12- 01110011
13- 01011111
14- 01100110
15- 01110101
16- 01101110
17- 01011111
18- 00110011
19- 01101000
```
We can convert this binary to 19 ASCII characters to get the flag, which we wrap around with uoftctf: uoftctf{CsS_l0g1c_is_fun_3h}