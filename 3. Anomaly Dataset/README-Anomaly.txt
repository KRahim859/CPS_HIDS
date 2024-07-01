Here abnormal Instruction introduced between normal execution of program to make it malicious.
Three differe kind of anomalies are indroduced in data. 

G28 -> Home All Axis 

This is added in files in such a way that axis go to home position and then return to normal flow over and over again.
For proper opperation of printing process this instruction should at the start or at the end but if this is active in some other position it would distrub the process.
--------------------------------------------------------------------------------------------------------------

M84 -> Disable Motors

This instruction should be at end of the normal flow but malicious files contain this instruction
during the middle of program again and again which is an abnormal behavior.


---------------------------------------------------------------------------------------------------------------

M104 S0 -> Turn off Temperature of Hot Bed

This instruction instantly turn the temperature off and with S0 parameter only introduced at the end of process.
Introducing this type instruction in middle portion will disrupts the gcode program invalid or malicious.





