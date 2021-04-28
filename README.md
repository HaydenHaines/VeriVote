# VeriVote - Only a fully open system can be fully secure
Open source voting system written in Rust. Unless a system is open it can never be secure. Open source repository will house all code and schematics.

The goal of any trusted voting system is five(5) fold. 
1) That the citizenry is assured that every legal voter can cast a ballot.
2) That the citizenry is assured that only legal voters cast ballots.
3) That the voter is assured that their ballot is always counted.
4) That the voter is assured that their ballot is secret.
5) That any ballot is only counted one time. 

If these five(5) hold, the rest will take care of itself. 
*** A software system can only insure the final three.
Therefore the goal of the VeriVote system is to
1) Assure that each ballot is counted.
2) Assure that each ballot is secret.
3) Assure that each ballot is counted only once.

New Process:
To do this, we will employ an additional new technology. An imaging system will be located on top of every ballot collection box. Each ballot will include an open space for a random hand drawing by the voter. It can be any shape, any consistency as long as it include no less than 85% whitespace, and is not the voter's signature.  Machine learning algorithms will be employed to render the shape into a score. Any two ballots that have equal scores will be flagged for hand examination to ensure that the same ballot was never run through a machine twice. 

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

No incoming access to the ballot box system through TCP or Serial. 
The ballot box system can connect outboud to a central collection system through TCP once and only once and only after it has been moved to END_OF_DAY (EOD). 
BALLOT BOX STATE MACHINE
1) Turning the System on will allow it to stay in the current mode or move to BEGIN_OF_DAY (BOD)
2) Moving a system to BOD will clear all previous votes.
3) In BOD, no TCP or Serial connection is possible. 
4) BOD, requires the precinct number, date and BOD Report to move to INITIALIZE_AND_TEST. 
5) INITIALIZE_AND_TEST (IAT) requires a program scan to be run. A ballot or ballots are run and options are set. 
6) In IAT, no TCP or Serial connection is possible.
7) In order to move forward, a ballot must be run for each option so that every ballot option has at least one vote. After each ballot option has one vote, then the precinct worker will be required to enter an id, and approve that the ballots are in line with the configuration.
8) Once IAT is approved, the system moves to OPERATIONAL (O_MODE). 
9) In O_MODE, no TCP or Serial connection is possible. 
10) In O_MODE, each ballot scanned is recorded as a incrementing number with a status of
  1) Full - A) all ballot options have one and only one selection and B) the unique mark is in place
  2) Partial - A) some ballot options are selected but no more than one selection for any option and B) the unique mark is in place
  3) Invalid - A) any number of ballot options has more than one selection or B) the unique mark is not in place or C) no options are selected
11) Any scanned ballot that is Invalid will not be accepted into the ballot box.
12) Once a system is moved to EOD, no votes can be collected.
13) EOD will print a report of every vote cast. 
14) The EOD report must match the display on the top of the ballot collection box. 
15) A verification must be entered that the displaty and the report match with an id of the verifier for prosecutor identification.
16) EOD can make a TCP connection to the central collection server ONCE and ONLY_ONCE. 
17) In the central system the totals of the ballot box must be verified by a certified user.
18) EOD cannot move to BOD if it has not transfered the report to the central server. 

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
