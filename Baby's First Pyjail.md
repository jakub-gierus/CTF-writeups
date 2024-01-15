We are given a connection `nc 35.226.249.45 5000`. 

Once connected, there is a python shell. 

Trying to import anything only returns the message "Try harder". 

Printing globals() shows that there is a variable called `blacklist: ['import', 'exec', 'eval', 'os', 'open', 'read', 'system', 'module', 'write', '.']`

Setting the blacklist variable to an empty list allows us to import packages again, specifically `os`. 

Then,   `print(os.listdir(os.getcwd()))` yields that there is a flag file in the current working directory.

Running` os.system("cat flag")` gives us the flag: uoftctf{you_got_out_of_jail_free}.

