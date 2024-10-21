# CPS_HIDS : A Gcode based Dataset
## Overview

G-code is a programming language for computer numerical control (CNC). In simpler words, it’s the language spoken by a computer controlling a machine, and it communicates all commands required for movement and other actions. While G-code is the standard language for different desktop and industrial machinery, our 3D printers are the recent day examples of machines using G-code. In actual operation, 3D slicers generate the code “automatically”. 
For more information, please refer to https://all3dp.com/2/3d-printer-g-code-commands-list-tutorial/

The **CPS_HIDS** dataset compiles 3D printer data aimed at aiding researchers in creating and evaluating intrusion detection systems for Cyber Physical Systems (CPS). It includes extensive data from 3D printers, covering various metrics and operational parameters suitable for anomaly detection, predictive maintenance, and other machine learning tasks. Leveraging a 3D printer testbed, we developed an innovative **Gcode** Dataset **(NIST RS-274 / ISO 6983-1:2009)** for **supervised** and **semi-supervised learning**. 

This Gcode dataset enabled the creation of a Host based Intrusion Detection System (HIDS) on a Raspberry Pi4, which can be smoothly integrated into automated manufacturing workflows in 3D printers and CNC machines. The developed methodology can effortlessly extend to resource-constrained CPS and larger Systems of Systems (SoS). 

Our HIDS incorporates context-aware security models tailored to meet the operational environment's requirements, including critical physical limits, thresholds, and behavioral anomalies. We have introduced an independent, lightweight HIDS with a signature-based ruleset, augmented by ML and DL-based anomaly detection models. A decision engine will assess whether commands are legitimate or malicious, thus permitting or blocking instructions from the local CPS execution queue. A feedback loop can enhance its detection engine by formulating new signatures or detecting new anomalies. This feedback also offers sensory data for the management and oversight of CPS devices or systems of systems. The proposed CPS-HIDS on RPi4 executes real-time processing of instructions from either remote or local administrators for a locally deployed CPS. Signature-based detection provides precise protection against known attacks with minimal processing load. For anomaly detection using ML and DL techniques, we are testing memory-efficient and compact pre-trained models appropriate for a resource-limited processing unit in an independent HIDS. The goal is to identify simple algorithms capable of conducting limited learning within CPS environments.

## Dataset Description


To improve accuracy and to protect CPS from inflicting physical damage in its operating environment, an IDS has to understand domain specific knowledge of the instructions (commands, sensing and actuation) being passed to and from a CPS. Therefore, we have developed a Gcode based dataset to design an IDS that can understand CPS specifics from the OT security constraints and protect these from damaging instructions and behavioral anomalies. 
Supervised algorithms demand meticulously labeled datasets that encapsulate both benign and malicious traffic, strategically situated on the target network safeguarded by the IDS. Predominantly, ML-based IDS research hinges on network traffic captures. Currently, there is an evident gap in the availability of Gcode datasets tailored for 3D printers. To bridge this gap, we have meticulously crafted a comprehensive Gcode dataset that mirrors the diverse types of data and real-world behaviors pivotal for the robust training and evaluation of our CPS-HIDS. 
G-code is a sequential lines of instructions, each telling the 3D printer to perform a specific task. These lines are known as commands, and the printer executes them one by one until reaching the end of the code. very G-code command line follows a certain syntax. Each line corresponds to only one command, which can lead to codes that are awfully lengthy. While the term “G-code” is used to reference the programming language as a whole, it’s also one of two types of commands used in 3D printing: “General” and “Miscellaneous” commands.

  - General command lines are responsible for types of motion in a 3D printer. Such commands are identified by the letter ‘G’, as in G-commands. Besides controlling the three+ axes of movement performed by the printhead, they’re also in charge of filament extrusion.

  - The Miscellaneous commands, on the other hand, instruct the machine to perform non-geometric tasks. In 3D printing, such tasks include heating commands for the nozzle and bed, fan control, among many others – as we’ll see. Miscellaneous commands are identified with the letter ‘M’.

To empower ML models in detecting CPS intrusions, we painstakingly extracted a plethora of potential G-code features, cataloging their respective values as distinctive instances, as elucidated below:-
-  G0 & G1: Linear Motion : G0 & G1 commands are responsible for linear motion and extrusion 
  - G0 X<pos> Y<pos> Z<pos> F<rate> E<pos>
  - G1 X<pos> Y<pos> Z<pos> F<rate> E<pos>
- Other G-code commands:
  -   {G28} & {G29}: Auto-Home & -Bed Leveling: We call “homing” the process of setting the physical limits of all movement axes. The G28 command will perform this task by moving the printhead until it triggers endstops to acknowledge the limits.
  -   {G21},
  -   {G92},
  -   {G90} and {G91}: The G90 and G91 commands tell the machine how to interpret coordinates used for movement. G90 establishes “absolute positioning”, which is usually the default, while G91 is for “relative positioning”.
- M-code: These regulate non-movement operations, e.g. {M104} controls the temperature of extruder. We extracted
  - {M82},
  - {M84},
  - {M107},
  - {M190},
  - {M104},
  - {M220} etc.
- Position and Directions: These variables are relative to the {X}, {Y}, and {Z} axes.
  - {S} feature is used for spindle speed.
  - {F} feature measures feedrate.
  - {E} feature is extrusion measure of nozzle. 

Subsequently, the dataset is systematically arranged into varying distributions of features and samples to facilitate distinct tasks, such as binary classification, multi-class classification, and anomaly detection.

**CPS_HIDS** includes:

- **Benign Dataset:** A diverse selection of open-source Gcode print files has been meticulously gathered. These files harbor commands that have been ingeniously employed as benign instructions. Every command, from the precise maneuvers of G1 and G21 to the critical functionalities of M28 and M109, along with the detailed parameters for X, Y, Z, F, E, S, etc., have been meticulously classified as significant parameters or features. Utilizing the powerful capabilities of the Python library {gcodeparser}, these intricate Gcode files have been seamlessly transformed into accessible CSV files. Every single benign instruction has been distinctly marked with 'No' or '0'.
The dataset includes data generated from Gcodes files (NIST RS-274 / ISO 6983-1:2009) for different tasks like supervised learning, semi-supervised learning for develping an update mechanism and anomaly detection over CPS.

- **Malicious Dataset:** To meticulously craft malicious data, Python’s {random} module was expertly harnessed to spawn data spanning the entire perilous spectrum of the designated workspace. Python’s {pandas} and {numpy} modules were deftly utilized for comprehensive data manipulation and intricate labeling. A myriad of datasets, each encompassing thousands of lines, were meticulously assembled to forge a vast Gcode databank. Malicious instructions were emphatically labeled as Drop ‘Yes’ or ‘1’.

- **Data Preprocessing:** Preprocessing data is an absolutely vital and transformative step before deploying any AI model. This meticulous process encompasses managing missing values, normalization, and a myriad of other tasks that are contingent on the dataset's quality. We have astutely employed zero-filling to address the challenge of missing data. The straightforward yet potent zero-filling method, though it has the potential to introduce certain biases, is instrumental in maintaining data integrity. In the realm of Gcode files, every instruction (along with its parameters) is executed in a strict sequential order. Executed instructions are distinctly marked with a '1' and their corresponding parameter values on the same line, while all other non-executed instructions are marked with a '0'. Thus, our strategic choice of zero-filling authentically reflects the actual execution process of Gcode, ensuring an accurate simulation of real-world scenarios.

- **Multi-class and Semi-Supervised Dataset:** 
  - {X}, {Y} and {Z} parameters greater than threshold have Drop column ‘High'.
  - {F}, {E} and {S} parameters greater than threshold have Drop column ‘Medium'.
  - Remaining instructions are labeled ‘Low'.
  - A number of instructions have been left unlabeled for semi-supervised testing. 

- **Modeling of Behavioral Anomalies:** Several Gcode print files containing the intricate start and end sequences of a printing job are meticulously utilized for dataset creation, meticulously aiding in the identification of specific, subtle patterns of normal and malicious behaviors. Anomalous instructions can be extraordinarily rare, for instance, appearing ominously only once in a colossal print file brimming with thousands of other legitimate instructions, or they may reappear persistently, causing frustrating print delays. Our robust method must adeptly address both these elusive rare and repetitive malicious events. If such events occur precisely in their expected positions, they will be validated and marked as legitimate instructions; however, if the same instructions are detected in abnormal, unexpected positions, they will be stringently excluded from the execution list. For empirical and rigorous testing purposes, three distinct anomalies have been expertly modeled, each ingeniously introducing a unique, conspicuous deviation from the expected behavior.
  - {The {G28} Anomaly (Home All Axis)}: The G28 command homes all printer axes and is usually used at the start. Using G28 elsewhere causes delays and is considered an anomaly.
  - {The {M104} Anomaly (Turn Off Temperature)}: The M104 command, used to control temperature, is considered an anomaly if it occurs before the end of printing.
  - {The {M84} Anomaly (Disable Motor)}: The M84 anomaly involves disabling a motor before the task close down phase. 

- **Sources**: Collected from an operational reprap based 3D printer in a controlled environment with free Gcode printfiles downloaded from below sources:-
  - www.3dbenchy.com
  - www.Thingverse.com
  - www.Printables.com
  - www.prusaprinters.com

- **Format**: CSV files; after pre-processing and necessary formatting corrections as per the requirement of the dataset.
  
For more description of the dataset and features collected, including the Gcode files please refer to the description in the `docs` folder.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Usage Policy
This dataset is open source and available for academic purposes. Researchers, students, and academics are encouraged to use this data for their projects, and experiments or fruther imporvemt to data. Please cite appropriately to use this data in your work. 
Stay tuned for first set of results.

## Contribution
We welcome contributions to improve the CPS_HIDS dataset and its associated tools. Here are the steps to contribute:

### Steps to Contribute

1. **Fork the repository**
   - Click the "Fork" button on the top right of the repository page on GitHub.

2. **Clone your forked repository**
   - Clone your forked repository to your local machine:
     ```bash
     git clone https://github.com/KRahim859/CPS_HIDS.git
     ```
   - Navigate into the cloned directory:
     ```bash
     cd CPS_HIDS
     ```

3. **Create a new branch**
   - Create a new branch for your feature or fix:
     ```bash
     git checkout -b feature-branch
     ```

4. **Make your changes**
   - Implement your feature or fix in your local repository.

5. **Commit your changes**
   - Stage your changes:
     ```bash
     git add .
     ```
   - Commit your changes with a meaningful commit message:
     ```bash
     git commit -m "Description of your changes"
     ```

6. **Push to the branch**
   - Push your changes to your forked repository:
     ```bash
     git push origin feature-branch
     ```

7. **Submit a pull request**
   - Go to the original repository on GitHub and you should see a prompt to create a pull request.
   - Click on "Compare & pull request".
   - Provide a meaningful title and description for your pull request.
   - Click "Create pull request".

### Guidelines

- Write clear and concise commit messages.
- Update documentation as necessary.
- Make sure your changes do not break existing functionality.

We appreciate your contributions and thank you for helping to improve CPS_HIDS!
We will continue to support this dataset and strive to share our results on this page soon. 
