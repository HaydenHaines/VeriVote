# VeriVote - Only a fully open system can be fully secure
An open source voting system written in Rust. Unless a system is open it can never be truly secure. Obfuscation is useless as a security measure. This Open source repository will house all code and schematics so that many universities and companies can contribute code to the repository. The code will always be free. The hope is that worldwide, we can expect in future generations to have free, fair and legal elections. 

Object diagrams, state diagrams and flow diagrams will be uploaded a separate documents. 

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

New Process IUIS:
To do this, we will employ an additional new technology. IUIS is an individual unique identifier system. Each ballot will include an open space for a random hand drawing by the voter. It can be any shape, any consistency as long as it include no less than 85% whitespace, and is not the voter's signature.  An imaging system will be located on top of every ballot collection box. Machine learning algorithms will be employed to render the shape into a score. Any two ballots that have equal scores will be flagged for hand examination to ensure that the same ballot was never run through a machine twice. 

Embedded system
  1) Image each ballot and if absentee, the envelope separately,
  2) No timestamps are used but rather GUIDs so that video of voters cannot be tied to individual ballots or that envelopes can be linked to ballots directly, 
  3) Record each ballot, 
  4) Collect each vote cast, 
  5) Uses an IUIS individual unique identifier system (non-signature)
  6) Prints a day end report
  7) Connects to central system uploads day end report only when vote system is closed.
  8) Uses the unique identifier system to verify each ballot was scanned only once
  9) Manages states for each stage of the vote day.
  10) Manages the terminus of ballots.
  11) Once attached to a ballot box, cannot be removed without error until EOD (End of Day) is complete.

Central System 
  1) Tabulates all the collected votes
  2) Requires a user to enter the day end report that was printed with a unique identifier
  3) Uses ML to compare the unique identifiers across all ballots to verify that each ballot was counted only once. 
  4) Allows recount entry and verification of unique identification system manually.

States
  1) Initialize
  2) Test and Configure
  3) Operational Mode
  4) End of Day
  5) Hand / Error Mode
  6) Closed / Archived
  
Modules:
  1) Embedded Vote Collection - EVC
  2) Cloud Vote Verification - CVV
  3) UIUC Machine Learning

Services:
  1) EVC::Imaging
  2) EVC::UniqueID
  3) EVC::Collection
  4) EVC::Settings
  5) EVC::Security
  6) UIUC::ML

  1) UIUC::ML
  2) CVV::Tabulation
  3) CVV::Security
  4) CVV::Settings
