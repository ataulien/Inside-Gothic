# Rain

![Rain in the Old Camp](images/rain.gif)

On the (almost) yearly _Moddertreffen_, a Community Meetup, I have been told,
that while the programmers thought Rain was important for an authentic feeling,
management (or the publisher, can't remember) didn't think so. The legend goes
that some members of the team met at the weekend and implemented rain into the
engine, because it felt like the right thing to do. Management liked it (or
didn't care) and they left it in.

## When does it rain?

The two main properties for controlling the rain are its _Start_ and _Stop_ times.

- There are no limitations on when the rain happens during the day (or night).
- The minimum duration for rain is 1 hour.
- The maximum duration is about 2.5 hours.

The following special cases exist:

1.  The first rain-event always happens between 16:30 and 17:30, so that the
    player will definitely see it.
2.  On the first 3 Days, it only rains. After those, there is also lightning with a chance of 40%.
