# kroki/fg-navaid

Kroki/fg-navaid is a Perl script for querying navigation data cache
that is kept by [FlightGear] (http://www.flightgear.org/).  The cache
is normally located at

    ~/.fgfs/navdata_X_Y.cache

where `X` and `Y` are major and minor version numbers.  The script is
known to work with 2.12 and 2.99 (to become 3.0).  I hadn't the chance
to test it with 2.10.


## Usage

In it's basic form (and the only one supported for the moment) the
usage is `kroki-fg-navaid LOCATION`, where `LOCATION` is either a
comma-separated pair of coordinates (refer to `kroki-fg-navaid --help`
for syntax), or an object identifier:

    $ kroki-fg-navaid OAK
    --
    Coordinates: N37°43'33.30", W122°13'24.90" (37.725917, -122.223583)
    Magnetic declination: E13°55' (13.91)
    Ident: OAK
    Object: OAKLAND VORTAC
    Elevation: 10'
    Orientation: E17°00' (17.00)
    Frequency: 116.8mhz (130nm)


    $ kroki-fg-navaid KSFO
    --
    Coordinates: N37°37'15.04", W122°22'51.77" (37.620845, -122.381048)
    Magnetic declination: E13°55' (13.91)
    Ident: KSFO
    Object: San Francisco Intl airport
    Elevation: 13'
    Runway 01R-19L: 8651' x 200', asphalt, ILS on 19L
    Runway 01L-19R: 7502' x 200', asphalt
    Runway 10R-28L: 10577' x 200', asphalt, ILS on 28L
    Runway 10L-28R: 11843' x 200', asphalt, ILS on 28R
    AWOS 1 (50nm): 118.05mhz
    CLNC DEL (50nm): 118.2mhz
    GND (50nm): 121.8mhz
    NORCAL APP (50nm): 135.65mhz
    NORCAL DEP (50nm): 135.1mhz
    TWR (50nm): 120.5mhz
    UNICOM (50nm): 122.95mhz


Detailed information about a particular runway can also be requested:

    $ kroki-fg-navaid KSFO-28R
    --
    Coordinates: N37°36'48.71", W122°21'25.71" (37.613532, -122.357141)
    Magnetic declination: E13°54' (13.90)
    Ident: KSFO 28R
    Object: San Francisco Intl airport, runway 28R
    Surface: asphalt
    Dimensions: 11843' x 200'
    Elevation: 13'
    Heading: 284°M (298°T)
    ILS-cat-III (18nm): IGWQ 111.7mhz 284°M (298°T)
    Glideslope: 3°
    Middle marker: DME 2.5nm, RA 226', 0.5nm to runway edge
    Inner marker: DME 2.1nm, RA 101', 0.1nm to runway edge
    Runway edge: DME 2.0nm, RA 57'
    Touchdown: 0.1nm past runway edge


Output in metric system is enabled by `--metric` or `-m`:

    $ kroki-fg-navaid -m RJCC-01L
    --
    Coordinates: N42°45'41.90", E141°41'34.17" (42.761640, 141.692826)
    Magnetic declination: W8°50' (-8.84)
    Ident: RJCC 01L
    Object: NEW CHITOSE airport, runway 01L
    Surface: asphalt
    Dimensions: 3001m x 60m
    Elevation: 24m
    Heading: 001°M (353°T)
    ILS-cat-I (33km): ICN 110.9mhz 001°M (353°T)
    Glideslope: 3°
    Middle marker: DME 0.9km, RA 66m, 0.9km to runway edge
    Runway edge: DME 0.0km, RA 15m
    Touchdown: 0.3km past runway edge


Bearings from the location to nearest beacons are reported with
`--bearings` or `-b`, for instance:

    $ kroki-fg-navaid --bearings=3 IBURI
    --
    Coordinates: N41°55'59.24", E141°28'10.71" (41.9331220, 141.4696410)
    Magnetic declination: W8°37' (-8.62)
    Ident: IBURI
    Object: fix
    Airways: V11 (low, high)
    Bearing to: HAKODATE VOR-DME (HWE) 112.3mhz 260°R (251°T) 30.1nm
    Bearing to: OMINATO NDB (OM) 353khz 208°M (200°T) 43.9nm
    Bearing to: CHITOSE NDB (CH) 347khz 021°M (012°T) 45.1nm

All distances and heights are rounded downwards.

Note that the output is only as accurate and complete as the cache
contents, which itself is derived from text navdata files that are
known to be flawed in many locations.  Use your good judgment when
relaying on the output.


## Required Perl modules

Kroki/fg-navaid will work with Perl 5.12 and above, and requires the
following modules to be installed:

    DBI
    DBD::SQLite
    Geo::Inverse
    Regexp::Grammars
    DateTime
