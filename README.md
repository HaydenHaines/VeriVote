# VeriVote - Only a fully open system can be fully secure
Open source voting system written in Rust. Unless a system is open it can never be truly secure. Obfiscation is useless as a security measure. This Open source repository will house all code and schematics so that many universities and companies can contribute code to the repository. The code will always be free. The hope is that worldwide, we can expect in future generations to have free, fair and legal elections. 

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
5) INITIALIZE_AND_TEST (IAT) requires a program scan to be run on a configuration ballot which identifies each option. 
6) Sample ballots are run to test configuration. 
7) In IAT, no TCP or Serial connection is possible.
8) In order to move forward, sample ballots must be run for each option so that every ballot option has at least one vote. 
9) After each ballot option has at least one vote, then a precinct worker will be required to enter an id, and approve that the ballots are in line with the configuration.
10) Once IAT is approved, the system moves to OPERATIONAL (O_MODE). 
11) In O_MODE, no TCP or Serial connection is possible. 
12) In O_MODE, each ballot scanned is recorded as a incrementing number with a status of
  1) Full - A) all ballot options have one and only one selection and B) the unique mark is in place
  2) Partial - A) some ballot options are selected but no more than one selection for any option and B) the unique mark is in place
  3) Invalid - A) any number of ballot options has more than one selection or B) the unique mark is not in place or C) no options are selected
13) Any scanned ballot that is Invalid will not be accepted into the ballot box.
14) No Time stamps on ballot scans only session ID
15) Absentee ballot envelopes are scanned separately into the same box. 
16) Each Absentee envelop has a unique identifier.
17) If Absentee envelopes != absentee ballots O_MODE changes to HAND
18) If the ballot box is tampered with O_MODE changes to HAND
19) If the same ballot is processed twice O_MODE changes to HAND
20) If a configuration Ballot is processed O_MODE changes to HAND
21) O_MODE can only move to EOD or HAND
22) O_MODE moves to HAND
23) HAND will no longer accept ballots
24) HAND is an error condition requiring a hand count in that case that someone has illegally scanned a ballot and therefore destroyed the integrity of the ballot box. 
25) HAND can only move to BOD
26) HAND will print a report
27) HAND can make a TCP connection to the central collection server ONCE and ONLY_ONCE. 
28) HAND reports the error condition and the image of each ballot
29) In the central system the totals of the ballot box must be entered by a certified user.
30) HAND cannot move to BOD if it has not transfered the report to the central server. 
31) O_MODE moves to EOD
32) Once a system is moved to EOD, no ballots can be collected.
33) EOD will print a report of every vote cast. 
34) The EOD report must match the display on the top of the ballot collection box. 
35) A verification must be entered that the displaty and the report match with an id of the verifier for prosecutor identification.
36) EOD can make a TCP connection to the central collection server ONCE and ONLY_ONCE. 
37) EOD transfers the vote totals and the image of each ballot
38) In the central system the totals of the ballot box must be verified by a certified user.
39) In the central system the envelope identifiers to cross reference with all requested absentee votes to verify only one per person and that only requested absentee ballots were received. 
40) EOD cannot move to BOD if it has not transfered the report to the central server. 

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
