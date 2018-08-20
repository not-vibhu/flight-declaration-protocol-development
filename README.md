## Overview

The Flight Declaration protocol aims to facilitate the secure exchange of flight situation data between UTM Providers. It was originally developed by the Altitude Angel team who offered it as a contribution to the Global UTM Association's data exchange initiative in February 2017.


## Read First
This is the "development" version of the Flight Declaration Protocol. The goal of this repository is to push the protocol froward and conduct tests as the UTM space moves forward. The protocol detailed in the repositry is backwards compatible with the production version and builds upon the production version by making the following changes: 

1) _Standardizing GeoJSON_: This protocol enforces a GeoJSON FeatureCollection instead of individual Features as specified in the production version. 
2) _Using Timestamps_: The production version of the protocol mandates a sequenceNumber to control the changes and updates to a flight declaration. Upon review, this was found out to be too flexible therefore this version proposes the using timeStamps instead of sequences to control updates and changes. 
3) _Volumetric Information_: This version of the protocol introduces max and min Alt definitions to better capture the airspace volume. (work in progress)
4) _Doucment Cleanup_: This repository has removed significant portions of text from the production version to have better flow and focus. 
   
## Get involved

External contributions are welcome. The best way to provide feedback and get involved is to open new issues and comment on existing open ones. 