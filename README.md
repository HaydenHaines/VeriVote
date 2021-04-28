# VeriVote
Open source voting system written in Rust. Unless a system is open it can never be secure. Open source repository will house all code and schematics.

The goal of a voting system is four fold. 
1) That the citizenry is assured that only legal voters cast legal ballots.
2) That the voter is assured that their vote is always counted.
3) That the voter is assured that their vote is always anonymous.
4) That any ballot is only counted one time. 

If these four hold, the rest will take care of itself. 
*** A software system can assure the last three.
Therefore the goal of the VeriVote system is to
1) Assure that each voter's vote is always counted.
2) Assure that each voter's vote is always anonymous.
3) Assure that each voter's vote is only counted one time.

New Process:
To do this, we will employ a new technology. An imaging system will be located on top of every ballot collection box. Each ballot will include an open space for a random hand drawing by the voter. It can be any shape, any consistency as long as it include no less than 75% whitespace, and is not the voter's signature.  Machine learning algorithms will be employed to render the shape into a score. Any two ballots that have equal scores will be flagged for hand examination to ensure that the same ballot was never run through a machine twice. 

Embedded system
  1) Images each ballot, 
  2) Records each each ballot, 
  3) Collects each vote cast, 
  4) Uses an individual unique identifier system non-signature
  5) Prints a day end report
  6) Connects once and only once to central system uploads day end report only when vote system is closed.
  7) Uses the unique identifier system to verify each ballot was scanned only once
  8) Manages states for each stage of the vote day.
  9) Manages the terminus of ballots.

Central System 
  1) Tabulates all the collected votes
  2) Requires a user to enter the day end report that was printed with a unique identifier
  3) Uses ML to compare the unique identifiers across all ballots to verify that each ballot was counted only once. 
  4) Allows recount entry and verification of unique identification system manually.

No access to the system through TCP or Serial. 
The system can connect out to a central system through TCP once and only once but only after it has been moved to END_OF_DAY (EOD). 
1) Once a system is moved to EOD, no votes can be collected.
2) To move a system to BEGIN_OF_DAY (BOD) all previous votes will be cleared completely.
3) BOD, requires the precinct number, date and BOD Report to move to INITIALIZE_AND_TEST. 
4) INITIALIZE_AND_TEST (IAT) requires a program scan to be run. A ballot or ballots are run and options are set. 
5) In order to move forward, a ballot must be run for each option so that every ballot option has at least one vote. After each ballot optoin has one vote, then the precinct worker will be required to enter an id, and approve that the ballots are in line with the configuration.
6) Once IAT is approved, the system moves to OPERATIONAL (O_MODE). 
7) In O_MODE, no TCP or Serial connection is possible. 
8) In O_MODE, each ballot scanned is recorded as a incrementing number with a status of
  1) Full - all ballot options have one and only one selection and the unique mark is in place
  2) Partial - some ballot options are selected but no more than one selection for any option and the unique mark is in place
  3) Invalid - any number of ballot option has more than one selection or the unique mark is not in place
9) Any scanned ballot that is Invalid will not be accepted into the ballot box. 

Modules:
  1) Embedded Vote Collection - EVC
  2) Cloud Vote Verification - CVV

Services:
  1) EVC::Imaging
  2) EVC::UniqueID
  3) EVC::Collection
  4) EVC::Settings
  5) EVC::Security

  1) CVV::MachineLearning
  2) CVV::Tabulation
  3) CVV::Security
  4) CVV::Settings
