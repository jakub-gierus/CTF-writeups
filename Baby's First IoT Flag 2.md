> Part 2 - What company makes the processor for this device? [https://fccid.io/Q87-WRT54GV81/Internal-Photos/Internal-Photos-861588](https://fccid.io/Q87-WRT54GV81/Internal-Photos/Internal-Photos-861588). Submit the answer to port 6318.

The link shows pictures for a Linksys WRT54G series. Going to the Wikipedia page of this router series (https://en.wikipedia.org/wiki/Linksys_WRT54G_series) reveals that Broadband makes all the router CPUs.

Thus, with `printf 'Broadcom\n\0' | nc 35.225.17.48 6318`, the flag is revealed: 
{Processor_Recon}

