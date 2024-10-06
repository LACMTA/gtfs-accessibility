# gtfs-accessibility

Our GTFS does not populate many fields used for accessibility information.  This is a big gap in information that could be communicated to riders.  This repository is a proof of concept to add some of that missing data by applying scripts to the the base GTFS data Metro generates.

Data gaps that can be addressed:
- Accessibility - [docs](https://gtfs.org/getting_started/features/accessibility/)
  - wheelchair accessibility of stops
    - rail stations
    - bus stops
  - wheelchair accessibility of trips (vehicle accomodation)
  - text-to-speech for stop names
- Pathways (station entrances/exits to boarding/disembarking locations) - [docs](https://gtfs.org/getting_started/features/pathways/)
  - pathways.txt - defines connections between nodes (Location Types) within a station. details that can be added about each pathway/connection:
    - length
    - width
    - slope (ramps)
    - stair count
    - in-station traversal time estimates can also be added
    - real-world wayfinding signage, allowing trip planners to provide directions such as "follow signs to ______"
  - levels.txt - defines all the different levels within a station, enabling teh use of elevators in conjunction with pathway connections

Challenges with providing some of this data:
- bus stops - the sheer number of stops that would need to be reviewed in person on a regular basis to keep the data accurate and fresh.
- pathways - implementation questions for complex stations, given that the spec is still very new.

## Notes

Valid options for `location_type` in stops.txt ([docs](https://gtfs.org/documentation/schedule/reference/#stopstxt)):

```
0 (or blank) - Stop (or Platform). A location where passengers board or disembark from a transit vehicle. Is called a platform when defined within a parent_station.
1 - Station. A physical structure or area that contains one or more platform.
2 - Entrance/Exit. A location where passengers can enter or exit a station from the street. If an entrance/exit belongs to multiple stations, it may be linked by pathways to both, but the data provider must pick one of them as parent.
3 - Generic Node. A location within a station, not matching any other location_type, that may be used to link together pathways define in pathways.txt.
4 - Boarding Area. A specific location on a platform, where passengers can board and/or alight vehicles.
```

Valid options for `wheelchair_boarding` in stops.txt ([docs](https://gtfs.org/documentation/schedule/reference/#stopstxt)):

```
For parentless stops:
0 or empty - No accessibility information for the stop.
1 - Some vehicles at this stop can be boarded by a rider in a wheelchair.
2 - Wheelchair boarding is not possible at this stop.

For child stops:
0 or empty - Stop will inherit its wheelchair_boarding behavior from the parent station, if specified in the parent.
1 - There exists some accessible path from outside the station to the specific stop/platform.
2 - There exists no accessible path from outside the station to the specific stop/platform.

For station entrances/exits:
0 or empty - Station entrance will inherit its wheelchair_boarding behavior from the parent station, if specified for the parent.
1 - Station entrance is wheelchair accessible.
2 - No accessible path from station entrance to stops/platforms.
```

Valid options for `wheelchair_accessible` in trips.txt ([docs](https://gtfs.org/documentation/schedule/reference/#tripstxt)):

```
0 or empty - No accessibility information for the trip.
1 - Vehicle being used on this particular trip can accommodate at least one rider in a wheelchair.
2 - No riders in wheelchairs can be accommodated on this trip.
```

Additionally, we can indicate `bikes_allowed` in trips.txt as well:

```
0 or empty - No bike information for the trip.
1 - Vehicle being used on this particular trip can accommodate at least one bicycle.
2 - No bicycles are allowed on this trip.
```

Potentially update OpenStreetMap simultaneously, to make sure the pedestrian pathways exist and are properly connected.  Can OSM do indoor mapping at multi-level stations?

## Questions

__Implementation of Platforms__

* Are we meant to identify door boarding locations?
* Can we have a single node for a platform? If so, should the coordinates for that single node be at the geometric center?

