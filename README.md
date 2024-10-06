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
