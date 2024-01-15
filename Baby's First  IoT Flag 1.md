> Part 1 - Here is an FCC ID, Q87-WRT54GV81, what is the frequency in MHz for Channel 6 for that device? Submit the answer to port 3895.

Looking Q87-WRT54GV81, leads me to this website: https://fccid.io/Q87-WRT54GV81.

Looking through the given specs, specifically the RF Exposure Info document (https://fccid.io/Q87-WRT54GV81/RF-Exposure-Info/RF-Exposure-Info-861595), results in a table for the frequencies for channels, including channel 6, being 2437 MHz. 

Then, as told by the contest organizers, I submitted like this: 

`printf '2437\n\0' | nc 35.225.17.48 3895`

Giving us the flag:  {FCC_ID_Recon}