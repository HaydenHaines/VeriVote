# Verita
Open source vote system written in rust for absolutely trustworthy voting systems

The goal of a voting system is four fold. 
1) That the citizenry is assured that only legal voters cast legal ballots.
2) That the voter is assured that their vote is always counted.
3) That the voter is assured that their vote is always anonymous.
4) That any ballot is only counted one time. 

If these four hold, the rest will take care of itself. 
A software system can assure the last three. 

An imaging system located on top of every ballot collection box collects the ballots. 
An embedded system 
  a) Images each ballot, 
  b) Records each each ballot, 
  c) Collects each vote cast, 
  d) Uses an individual unique identifier system
  e) Prints a day end report
  f) Connects to central system uploads day end report
  g) Uses the unique identifier system to verify each ballot was scanned only once

No access to the system through TCP or Serial. 
The system can connects out to a central system through TCP but only after it has been moved to END_OF_DAY (EOD). 
1) Once a system is moved to EOD, no votes can be collected.
2) To move a system to BEGIN_OF_DAY (BOD) all previous votes will be cleared completely.
3) BOD, requires the precinct number, date and BOD Report to move to INITIALIZE_AND_TEST. 
4) INITIALIZE_AND_TEST (IAT) requires a program scan to be run. A ballot or ballots are run and options are set. 
5) In order to move forward, a ballot must be run for each option so that every ballot option has at least one vote. After each ballot optoin has one vote, then the precinct worker will be required to enter an id, and approve that the ballots are in line with the configuration.
6) Once IAT is approved, the system moves to OPERATIONAL (O_MODE). 
7) In O_MODE, no TCP or Serial connection is possible. 
8) In O_MODE, each ballot scanned is recorded as a incrementing number with a status of
  a) Full - all ballot options have one and only one selection and the unique mark is in place
  b) Partial - some ballot options are selected but no more than one selection for any option and the unique mark is in place
  c) Invalid - any number of ballot option has more than one selection or the unique mark is not in place
9) Any scanned ballot that is Invalid will not be accepted into the ballot box. 

Modules:
Embedded Vote Collection - EVC
Cloud Vote Verification - CVV

Services:
EVC::Imaging
EVC::UniqueID
EVC::Collection
EVC::Settings
EVC::Security

CVV::MachineLearning
CVV::Tabulation
CVV::Security
CVV::Settings

