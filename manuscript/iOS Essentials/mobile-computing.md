## Multiplexing
 - Multiplexing describes how several users can share a medium with minimum or no interference.
 - The task of multiplexing is to assign space, time, frequency, and code to each communication channel with a minimum of interference and a maximum of medium utilization.
 - Communication channel here only refers to an association of sender(s) and receiver(s) who want to exchange data.
 - In wireless communication, multiplexing can be carried out in four dimensions: space, time, frequency, and code.

## Space division multiplexing
- Following image shows six channels ki and introduces a three dimensional coordinate system, where system shows the dimensions of code c, time t and frequency f.
- For space division multiplexing (SDM), the (three dimensional) space si represented via circles indicates the interference range.
![Space division multiplexing (SDM)](https://s3.postimg.org/r79hwwaab/space_division.png)
- The channels k1 to k3 can be mapped onto the three ‘spaces’ s1 to s3 which clearly separate the channels and prevent the interference ranges from overlapping. 
- The space between the interference ranges is sometimes called guard space. 
- Such a guard space is needed in all four multiplexing schemes presented.
- This procedure clearly represents a waste of space.
- This principle was used by the old analog telephone system where each subscriber is given a separate pair of copper wires to the local exchange.
- In wireless transmission, SDM implies a separate space for each communication channel with a wide enough distance between senders. 
- Eg In FM radio stations where the transmission range is limited to a certain region, many radio stations around the world can use the same frequency without interference.
- Problems arise if two or more channels were established within the same space, for example, if several radio stations want to broadcast in the same city, then one of the following multiplexing schemes must be used (frequency, time, or code division multiplexing).

## Frequency division multiplexing
- In this scheme we subdivide the frequency dimension into several non-overlapping frequency bands.
- Each channel ki is now allotted its own frequency band.
- Guard spaces are needed to avoid frequency band overlapping (also called adjacent channel interference).
- This scheme is used for radio stations within the same region, where each radio station has its own frequency.
- This is simple and does not need complex coordination between sender and receiver: the receiver only has to tune in to the specific sender.
![Frequency division multiplexing](https://s4.postimg.org/85eztrhcd/frequency_division.png)
- Some mobile communication typically takes place for only a few minutes at a time. Assigning a separate frequency for each possible communication scenario would be a tremendous waste of (scarce) frequency resources.
- Also fixed assignment of a frequency to a sender makes the scheme very inflexible and limits the number of senders.

## Time division multiplexing
- A more flexible multiplexing scheme for typical mobile communications.
- Here a channel ki is given the whole bandwidth for a certain amount of time.
- All senders use the same frequency but at different points in time.
- Again, guard spaces, which now represent time gaps, have to separate the different periods when the senders use the medium.
![Time division multiplexing](https://s9.postimg.org/uivnxugdb/time_division.png)
- If two transmissions overlap in time, this is called co-channel interference, to avoid this a precise synchronization between different senders is necessary.
- This is clearly a disadvantage, as all senders need precise clocks or, alternatively, a way has to be found to distribute a synchronization signal to all senders.
- For a receiver tuning in to a sender this does not just involve adjusting the frequency, but involves listening at exactly the right point in time.
- This scheme is quite flexible as one can assign more sending time to senders with a heavy load and less to those with a light load.