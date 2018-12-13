# UAV/Operator Flight Declaration Exchange Protocol

- [UAV/Operator Flight Declaration Exchange Protocol](#uavoperator-flight-declaration-exchange-protocol)
	- [1 Introduction](#1-introduction)
	- [2 Definitions](#2-definitions)
	- [3 Example Declarations](#3-example-declarations)
	- [4 Details](#4-details)
		- [4.1 message](#41-message)
		- [4.2 flight_declaration](#42-flightdeclaration)
			- [4.3 idents](#43-idents)
		- [4.4 operation_mode enum](#44-operationmode-enum)
		- [4.3 flightPart](#43-flightpart)
			- [4.3.1 Notes](#431-notes)
			- [4.3.2 Altitude Datum enum](#432-altitude-datum-enum)
			- [4.6 error](#46-error)
	- [5 Standard Concepts](#5-standard-concepts)
		- [5.1 Dates & Times](#51-dates--times)
		- [5.2 Geospatial Data](#52-geospatial-data)
		- [5.3 Distances](#53-distances)
		- [5.4 Flight Identifier](#54-flight-identifier)
		- [5.5 Atomic updates](#55-atomic-updates)
		- [5.6 Timestamp and Sequence Number](#56-timestamp-and-sequence-number)
		- [5.7 Deletion](#57-deletion)
	- [6 References](#6-references)
	- [7 Revision History](#7-revision-history)
	- [8 Attribution](#8-attribution)

## 1 Introduction
_A protocol designed to facilitate the secure exchange of flight situation data between UTM Providers, while allowing each UTM Provider to retain ownership of their customer data._

## 2 Definitions

In this specification, we introduce and propose the following definitions:

| Term | Meaning |
| --- | --- |
| Originating Party | The UTM Provider who wishes to publish data to other members |
| Interested Party | Any party receiving a flight declaration that is not the Originating Party. For the purposes of this specification, this party is essentially the "consumer" of data published by Originating Parties. |
| Flight declaration | Information, originating from a user, that describes their intent to fly or that they are flying within the notified region. Only one drone can participate in a flight declaration at any one time. |
| Operating Area/Notified Area | The area that a drone pilot is or is planning to operate in. Unless the operator defines a more specific area, this can be assumed to be a circle around the operator's location with a radius equal to the maximum distance the operator is permitted to fly. |
| Route | One or more consecutive straight lines the drone will follow. |
| User/Pilot | The party declaring the flight (i.e. a customer of an Originating Party). |
| Very low altitude | Describes the airspace below which General Aviation is not permitted to operate – typically below 500 feet AGL. |


## 3 Example Declarations


**Example 1:** A VLOS survey flight with a polygonal area of operation.

	{
	"exchange_type":"flight_declaration",
	"flight_id": "5a7f3377-b991-4cc8-af2d-379d57f786d1",
	"plan_id": "05ccbdc5-81cd-4da4-aaf9-50feb3d3673c",
	"flight_state":2,
	"flight_approved":0,
	"flight_completed":1,
	"sequence_number":0,
	"time_stamp":"2018-08-15T14:29:08.842Z",
	"version":"1.0.0",
	"flight_declaration":{
		"parts":{
			"type":"FeatureCollection",
			"features":[
				{
				"type":"Feature",
				"properties":{
					"start_time":"2017-02-01T15:00:00+00:00",
					"end_time":"2017-02-01T15:30:00+00:00",
					"max_altitude":{
						"metres":152.4,
						"datum":"agl"
					},
					"min_altitude":{
						"metres":132.0,
						"datum":"agl"
					}
				},
				"geometry":{
					"type":"Polygon",
					"coordinates":[
						[
							[
							-6.290252208709717,
							53.21870259514697
							],
							[
							-6.285746097564697,
							53.21870259514697
							],
							[
							-6.285746097564697,
							53.22109226443749
							],
							[
							-6.290252208709717,
							53.22109226443749
							],
							[
							-6.290252208709717,
							53.21870259514697
							]
						]
					]
				}
				}
			]
		},
		"purpose":"Agricultural survey",
		"expect_telemetry":false,
		"originating_party":"Originating Party, Inc.",
		"contact_url":"https://utm.originatingparty.com/contact?d6c8cec9-2d57-43f6-8301-53efee5702b4",
		"operation_mode":"vlos",
		"vehicle_id": "157de9bb-6b49-496b-bf3f-0b768ce6a3b6",
		"operator_id": "4a725cb5-02d2-4f78-888f-b93088d324be",
		"actual_take_off_time" : "2018-08-04T22:44:30.652Z",
		"actual_landing_time" : "2018-08-30T14:51:10.802773Z"
	}
	}

**Example 2:** A BVLOS delivery flight with an out bound leg and a return leg, the return leg being flown an hour 
after the return leg. This drone is expecting to provide telemetry during the flight

	{
	"exchange_type":"flight_declaration",
	"flight_id": "5a7f3377-b991-4cc8-af2d-379d57f786d1",
	"plan_id": "a5b5484c-a23c-4e83-8bb8-a6a5c294e45b",
	"flight_state":2,
	"flight_approved":0,
	"flight_completed":1,
	"sequence_number": 0,
	"time_stamp": "2018-08-15T14:29:08.842Z",
	"version": "1.0.0",
	"flight_declaration": {
		"parts": {
		"type": "FeatureCollection",
		"features": [
			{
			"type": "Feature",
			"properties": {
				"id": "0",
				"start_time": "2017-02-01T15:00:00+00:00",
				"end_time": "2017-02-01T15:30:00+00:00",
				"max_altitude": {
				"metres": 152.4,
				"datum": "agl"
				},
				"min_altitude": {
				"metres": 102.4,
				"datum": "agl"
				}
			},
			"geometry": {
				"type": "LineString",
				"coordinates": [
				[
					-6.294693946838379,
					53.21507929385216
				],
				[
					-6.284565925598144,
					53.22237697776495
				]
				]
			}
			},
			{
			"type": "Feature",
			"properties": {
				"id": "1",
				"start_time": "2017-02-01T16:00:00+00:00",
				"end_time": "2017-02-01T16:30:00+00:00",
				"max_altitude": {
				"metres": 152.4,
				"datum": "agl"
				},
				"min_altitude": {
				"metres": 102.4,
				"datum": "agl"
				}
			},
			"geometry": {
				"type": "LineString",
				"coordinates": [
				[
					-6.28392219543457,
					53.22213288519814
				],
				[
					-6.294286251068114,
					53.2148865564753
				]
				]
			}
			}
		]
		},
		"purpose": "Delivery",
		"expect_telemetry": true,
		"originating_party": "Originating Party, Inc.",
		"contact_url": "https://utm.originatingparty.com/contact?5a7f3377-b991-4cc8-af2d-379d57f786d1",
		"operation_mode": "bvlos",
		"vehicle_id": "157de9bb-6b49-496b-bf3f-0b768ce6a3b6",
		"operator_id": "4a725cb5-02d2-4f78-888f-b93088d324be",
		"idents": [
		{
			"method": "adsb",
			"ident": "4840D6"
		}
		],
		"actual_take_off_time" : "2018-08-04T22:44:30.652Z",
		"actual_landing_time" : "2018-08-30T14:51:10.802773Z"
	}
	}


## 4 Details
### 4.1 message

The primary entity exchanged between Originating and Interested Parties.

	{
		"flight_id": "5a7f3377-b991-4cc8-af2d-379d57f786d1",
		"plan_id": "a5b5484c-a23c-4e83-8bb8-a6a5c294e45b",
		"sequence_number": 0,
		"time_stamp":"2018-08-15T14:29:08.842Z",
		"flight_declaration": { ... },
		"version": "1.0.0"
	}

| Name | Description | Type |
| --- | --- | --- |
| **exchange_type** | Specify the type of exchange . At the moment only allowed type is flight_declaration | string |
| **flight_id** | Identifier provided by the Originating Party that uniquely identifies this declaration from other declarations provided by the same Originating Party. | string |
| **plan_id** | Identifier provided by the Originating Party that uniquely identifies this declaration as a part of a flight plan. | string |
| **sequence_number** | **(optional)** A number that represents the version of this message data. When a record is modified, the sequence number must be numerically greater than the previous update. | number (uint64) |
| **time_stamp** |  **ISO-8601** **[[5]](#Ref-5)** formatting standard. Local times are not supported; all times must be in UTC or have a time zone offset specified. | YYYY-MM-DDTHH:mm:ss.sssZ |
| **flight_declaration** | A flight_declaration object describing this proposed flight. To delete a flight, this field should be null. | flight_declaration |
| **version** | The version of this protocol that the message has been implemented from. | string - currently "0.2.0" |
| **flight_state** | The state of the flight at the given time_stamp 0 - proposed, 1 - active, 2 - completed | integer |
| **flight_approved** | Is the flight approved at the given time_stamp 0 - False, 1 - True | integer |
| **flight_completed** | Is the flight completed at the given time_stamp 0 - False, 1 - True. | integer |

### 4.2 flight_declaration

	{
			"parts": {...},
			"purpose": "Delivery",
			"expect_telemetry": true,
			"originating_party": "Originating Party, Inc.",
			"contact_url": "https://utm.originatingparty.com/contact?5a7f3377-b991-4cc8-af2d-379d57f786d1",
			"operation_mode": "bvlos",
			"idents": [
					{
						"method": "adsb",
						"ident": "4840D6"
					}
			]
			"actual_take_off_time" : "2018-08-04T22:44:30.652Z",
			"actual_landing_time" : "2018-08-30T14:51:10.802773Z"
	}


| Name | Description | Type |
| --- | --- | --- |
| **parts** | One or more part that make up this flight | GeoJSON FeatureCollection |
|**purpose** | A human readable description of the reason the flight is being conducted. This field can be omitted if the end user chooses not to share the purpose of their flight. (_See notes_)| string |
| **expect_telemetry** | A flag indicting whether it is expected that telemetry will be available during the flight. | boolean |
| **originating_party** | The name of the party that the flight was originally declared with. | string |
| **contact_url** | The URL to be use to initiate contact with the user. This can be used to make nuisance report about this flight or for law enforcement to start the process of identifying a drone operator. It is expected that the flight_id will be used as part of this Url as the Url must be standalone and not require any other information. | string |
| **operation_mode** | The mode that the drone is being operated in. | operation_mode { "vlos", "evlos", "bvlos", "automated" } |
| **idents** | Any idents that are associated with this flight | array _[optional]_ |
| **actual_take_off_time** | The time the flight took off. This value can be null or omitted if the take-off time is not known | datetime _[optional]_ |
| **actual_landing_time** | The time the flight completed. This value can be null or omitted if the landing time is not known | datetime _[optional]_ |

It is expected that a future version of this specification will include a telemetryEndPoint field. This is an endpoint that an interest party would be able to call to get live telemetry while the flight was in progress.


   
#### 4.3 idents

To allow a flight to be correlated with positional data from other sources (e.g. ADSB or RADAR) a flight declaration can 
include one or more _idents_ that will be associated with this flight.

| Name | Description | Type |
| --- | --- | --- |
| **method** | The name of the technology providing the ident | string { "adsb", "flarm" } |
| **ident** | The identifier in the specified data source that will identify this flight. | string |

    {
        "method": "adsb",
        "ident": "4840D6"
    }

It is recognised that not all idents that a drone may be allocated will be available at the time of flight declaration. If a drone is allocated an ident it should update the flight declaration to include the new ident. It must not be treated as an error if the Interested Party does not recognise a method that is provided.

### 4.4 operation_mode enum

| Datum | Description |
| --- | --- |
| **vlos** | The drone is being flown by a human pilot within visual line of sight |
| **evlos** | The drone is being flown by a human pilot with extended visual line of sight – typically enabled by the use of observers. |
| **bvlos** | The drone is being flown by a human pilot beyond visual line of sight |
| **automated** | The drone does not have a human pilot |


### 4.3 flightPart 
A flight consists of one or more parts and these parts must be declared as a GeoJSON FeatureCollection. Each part has a start and end time as well as a geography and maximum altitude. In addition to describing the the Geogrpahic area of operation in a Polygon or LineString format. The following properties must be declared for each format. 

| Name | Description | Type |
| --- | --- | --- |
| **id** | An identifier that uniquely identifies this part within this flight. | string |
| **geography** | A GeoJSON Feature Collection describing the planned operating area or route. | geometry |
| **start_time** | The time that the flight is expected to start. | datetime |
| **end_time** | The time that the flight is expected to be completed by. This must always be greater than start_time. | datetime |
| **max_altitude** | The maximum altitude that the drone will achieve during the _flightPart_. | altitude |
| **min_altitude** | The minumum altitude that the drone will achieve during the _flightPart_. | altitude |


	{
			"type": "FeatureCollection",
			"features": [
				{
				"type": "Feature",
				"properties": {
					"id": "0",
					"start_time": "2017-02-01T15:00:00+00:00",
					"end_time": "2017-02-01T15:30:00+00:00",
					"max_altitude": {
						"metres": 152.4,
						"datum": "agl"
					},
					"min_altitude": {
						"metres": 102.4,
						"datum": "agl"
					}
				},
				"geometry": {
					"type": "LineString",
					"coordinates": [
					[
						-6.294693946838379,
						53.21507929385216
					],
					[
						-6.284565925598144,
						53.22237697776495
					]
					]
				}
				},
				{
				"type": "Feature",
				"properties": {
					"id": "1",
					"start_time": "2017-02-01T16:00:00+00:00",
					"end_time": "2017-02-01T16:30:00+00:00",
					"max_altitude": {
						"metres": 152.4,
						"datum": "agl"
					},
					"min_altitude": {
						"metres": 102.4,
						"datum": "agl"
					}
				},
				"geometry": {
					"type": "LineString",
					"coordinates": [
					[
						-6.28392219543457,
						53.22213288519814
					],
					[
						-6.294286251068114,
						53.2148865564753
					]
					]
				}
				}
			]
	}

#### 4.3.1 Notes
- No parts of a declared flight can have overlapping start and end times.
- For geography, a _FeatureCollection_ is to be used to describe :
  - _Polygon_ is used to define the area where a drone is expected to operate. When the geography is a _Polygon_, multiple linear rings representing the exterior ring (and optionally 'holes') should be supported. This allows the geography to define parts of the area where the drone will not operate.
  - _LineString_ should be used when there is a defined route that the drone is expected to follow. When the geography is a _LineString_, it may have more than two points.
  - No other GeoJSON types are supported.
  - For version 1.0 of the specification, neither sps nor amsl are supported datums for max_altitude or min_altitude


#### 4.3.2 Altitude Datum enum

This specification defines the following altitude datums, however specific messages may not allow altitudes to be defined using certain datums (e.g. it makes no sense to declare the maximum altitude of a pre-planned very low altitude flight relative to the SPS datum). The following string values are supported for datums effectively creating an enumeration.

| Datum | Description |
| --- | --- |
| **agl** | Above Ground Level |
| **amsl** | Above Mean Sea Level. This value is included for completeness. As this datum is not valid for any of the messages in this specification, the issue of defining the tidal datum for Mean Sea Level has not been included. |
| **sps** | Altitude where a barometric altimeter would be set to the Standard Pleasure Setting. This is effectively of the Flight Level multiplied by 100 and converted to metres. |
| **wgs84** | Distance above the WGS 84 datum. |

An altitude 152.4 metres above ground level would be represented by the following entity:

    {
        "metres": 152.4,
        "datum": "agl"
    }


#### 4.6 error


| Name | Description | Type |
| --- | --- | --- |
| **error_description** | A human-readable description of what the error was. | string |
| **error_type** | A type of error that was encountered: e.g. SchemaError, RangeError, etc. | string |
| **should_retry** | A flag to indicate that the whether the message should be queued for resending. | bool |
| **param_name** | If the error was caused due to validation failure, the name of the parameter should be provided in this field. | string _[optional]_ |

**Example 1: Retry not required**

	{
		"flight_id":"5a7f3377-b991-4cc8-af2d-379d57f786d1",
		"plan_id": "a5b5484c-a23c-4e83-8bb8-a6a5c294e45b",
		"sequence_number":1,
		"time_stamp":"2018-08-15T14:29:08.842Z",
		"version":"1.0.0",
		"parts": {...},
		"error": {
			"error_description": "Server doesn't require sender to retry",
			"error_type": "SchemaError",
			"should_retry": false
		}
		
	}

## 5 Standard Concepts

### 5.1 Dates & Times

Dates and times will follow the **ISO-8601** **[[5]](#Ref-5)** formatting standard. Local times are not supported; all times must be in UTC or have a time zone offset specified.


The format pattern for date times is `YYYY-MM-DDTHH:mm:ss.sssZ` where Z is either the character _Z_ to represent UTC, _or_ the +/- timezone offset from UTC. If a message is received with no timezone offset, it should be regarded as an error and the correct error code returned.

The requirement to specify times or dates does not apply to the specification, so all temporal data will be defined as a _datetime_.

No guarantees that a timezone offset will be preserved should be made.

### 5.2 Geospatial Data

Geospatial data must be described using a geometry object as defined in the _GeoJSON_ specification [[6]](#Ref-6) with the following specific requirements:

- Using the default CRS - geographic coordinate reference system, using the WGS84 datum, and with longitude and latitude units of decimal degrees

- Bounding box is not expected

Latitude and Longitude should not be specified to more than 8 decimal places. This gives an accuracy of approximately 1.1 mm at the equator making any further digits superfluous.

### 5.3 Distances

All distances (both horizontal and vertical) are specified in metres.

### 5.4 Flight Identifier

This is a unique identifier generated by the Originating Party that should provide an _anonymous_ identifier for this flight. The Originating Party must be able to use this identifier to identify the original records that resulted in this flight. Together with knowledge of who the Originating Party is, the flight identifier constitutes a globally unique identifier for this flight. i.e. Identifiers may be shared across originators, but the must be unique within an originator.

### 5.5 Atomic updates

When a Flight Declaration is updated by an Originating Party, that _entire_ Flight Declaration record will be sent. This allows the receiving service to replace the record in its entirety and allows the system to self-heal in the event of a missing message. This also removes the need to differentiate from a create and update – both are handled in the same way.

### 5.6 Timestamp and Sequence Number

To account for the possibility of messages being delivered out-of-order, a combination of time_stamp and sequence number must be used. A time_stamp of the creation of the message must be added at every declaration. When a flight declaration is updated, the sequence number must also be increased to make it numerically greater than the previous update. This allows the receiver to ensure that they are not overwriting more recent data if they process a delayed update message. The time_stamp and the sequence numeber used in combination serve as the primary method of verification of sequence. The time_stamp should be considered a **primary** method while the sequence number should be considered as a **secondary** form of verification. 

Sequence numbers do not have to be consecutive, nor do they need to start from zero, however they do need to be an unsigned integer – i.e. a whole number greater than or equal to zero.

### 5.7 Deletion

To delete a Flight Declaration a null flight_declaration element should be sent in the [message](#message). The flight_id must be provided and the sequence number increased. It is regarded as invalid to update a previously deleted record, once a flight declaration is deleted it must not be re-activated.

## 6 References
|||
| --- | --- |
| [1] | "RFC 2119: Key words for use in RFCs to Indicate Requirement Levels" - [https://www.ietf.org/rfc/rfc2119.txt](#https://www.ietf.org/rfc/rfc2119.txt) |
| [2] | "RFC 7159: The JavaScript Object Notation (JSON) Data Interchange Format" - [https://tools.ietf.org/html/rfc7159](#https://tools.ietf.org/html/rfc7159) |
| [3] | "RFC 2818: HTTP Over TLS" - [https://tools.ietf.org/html/rfc2818](#https://tools.ietf.org/html/rfc2818) |
| [4] | "Semantic Versioning 2.0.0" - [http://semver.org/](#http://semver.org/) |
| [5] | "ISO 8601 - Date and time format" - [http://www.iso.org/iso/home/standards/iso8601.htm](#http://www.iso.org/iso/home/standards/iso8601.htm) |
| [6] | "RFC 7946: The GeoJSON Format" - [https://tools.ietf.org/html/rfc7946](#https://tools.ietf.org/html/rfc7946)|

## 7 Revision History
**REVISION HISTORY**

| Version | Date | Comments |
| --- | --- | --- |
| 0.2.3-draft | 26/11/18 | Added additional fields for flight state |
| 0.2.2-draft | 01/07/18 | Refactor to remove significant portions  |
| 0.2.1-draft | 01/07/18 | Revisions from GUTMA |
| 0.1.0-draft | 01/09/16 | Initial draft specification by Altitude Angel |
| 0.1.1-draft | 03/11/16 | Updates to terminology and inclusion of example message flows |
| 0.2.0-draft | 16/01/17 | Version 0.2 ready for public release and comment |

## 8 Attribution

**ATTRIBUTION NOTICE**

| Author | Company | Role |
| --- | --- | --- |
| H. Ballal | GUTMA | Revisions |
| N. Kidd | Altitude Angel | Original author |
| R. Garfoot | Altitude Angel | Contributor |
| R. Parker | Altitude Angel | Contributor |
