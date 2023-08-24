## golang-date
---

### The layout parameter

    Mon Jan  2 15:04:05 MST 2006

Easier to remember

    01/02 03:04:05PM '06 -0700

### Standard time and date formats

    January 2, 2006                   Date
    01/02/06
    Jan-02-06

    15:04:05                          Time
    3:04:05 PM

    Jan _2 15:04:05                   Timestamp
    Jan _2 15:04:05.000000            with microseconds

    2006-01-02T15:04:05-0700          ISO 8601 (RFC 3339)
    2006-01-02
    15:04:05

    02 Jan 06 15:04 MST               RFC 822
    02 Jan 06 15:04 -0700             with numeric zone

    Mon, 02 Jan 2006 15:04:05 MST     RFC 1123
    Mon, 02 Jan 2006 15:04:05 -0700   with numeric zone


### Predefined date formats

    ANSIC        = "Mon Jan _2 15:04:05 2006"
    UnixDate     = "Mon Jan _2 15:04:05 MST 2006"
    RubyDate     = "Mon Jan 02 15:04:05 -0700 2006"
    RFC822       = "02 Jan 06 15:04 MST"
    RFC822Z      = "02 Jan 06 15:04 -0700"
    RFC850       = "Monday, 02-Jan-06 15:04:05 MST"
    RFC1123      = "Mon, 02 Jan 2006 15:04:05 MST"
    RFC1123Z     = "Mon, 02 Jan 2006 15:04:05 -0700"
    RFC3339      = "2006-01-02T15:04:05Z07:00"
    RFC3339Nano  = "2006-01-02T15:04:05.999999999Z07:00"
    Kitchen      = "3:04PM"
    Stamp        = "Jan _2 15:04:05"
    StampMilli   = "Jan _2 15:04:05.000"
    StampMicro   = "Jan _2 15:04:05.000000"
    StampNano    = "Jan _2 15:04:05.000000000"


### Layout options

    Year	    06   2006
    Month	    01   1   Jan   January
    Day	        02   2   _2   (width two, right justified)
    Weekday	    Mon   Monday
    Hours	    03   3   15
    Minutes	    04   4
    Seconds	    05   5
    ms μs ns	.000   .000000   .000000000
    ms μs ns	.999   .999999   .999999999   (trailing zeros removed)
    am/pm	    PM   pm
    Timezone    MST
    Offset	    -0700   -07   -07:00   Z0700   Z07:00


