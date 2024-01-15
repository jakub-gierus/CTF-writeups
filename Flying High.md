We are given an image of a plane and told to figure out the airport, airline and aircraft model of the plane in red-orange. ![[airplane.png]]

The first thing I did was run the image through reverse image search, but unfortunately nothing turned up. Then I looked up the only legible text in the image, Novespace on the building. Novespace is a company offering gravity-free flights *only* in the Bordeaux-Mérignac airport area. 

Using the Google Street View, on the Novespace area yields the exact image given. We have part one of the flag: the IATA airport designation **BOD**.

Then, looking through all regularly active airlines at Bordeauz-Merignac (https://www.bordeaux.aeroport.fr/en/flights-destinations/airlines), the one with the correct color scheme was Iberia, which with an additional check shows that it has the right logo. 

Finally, looking through all airplane models active for Iberia at least since 2012 (when the photo was taken), and the only one with two engines per wing was the Airbus A340-300. 

This gives the final answer of UofTCTF{BOD_Iberia_A340-300}.