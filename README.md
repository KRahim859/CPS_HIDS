# CPS_HIDS : A Gcode based Dataset
## Overview

G-code is a programming language for computer numerical control (CNC). In simpler words, it’s the language spoken by a computer controlling a machine, and it communicates all commands required for movement and other actions. While G-code is the standard language for different desktop and industrial machinery, our 3D printers are the recent day examples of machines using G-code. In actual operation, 3D slicers generate the code “automatically”. 
For more information, please refer to https://all3dp.com/2/3d-printer-g-code-commands-list-tutorial/

The **CPS_HIDS** dataset compiles 3D printer data aimed at aiding researchers in creating and evaluating intrusion detection systems for Cyber Physical Systems (CPS). It includes extensive data from 3D printers, covering various metrics and operational parameters suitable for anomaly detection, predictive maintenance, and other machine learning tasks. Leveraging a 3D printer testbed, we developed an innovative **Gcode** Dataset **(NIST RS-274 / ISO 6983-1:2009)** for **supervised** and **semi-supervised learning**. 

This Gcode dataset enabled the creation of a Host based Intrusion Detection System (HIDS) on a Raspberry Pi4, which can be smoothly integrated into automated manufacturing workflows in 3D printers and CNC machines. The developed methodology can effortlessly extend to resource-constrained CPS and larger Systems of Systems (SoS). 

Our Host Intrusion Detection System (HIDS) is designed to fit the specific needs of the CPS' operational environment, taking into account physical limits, thresholds, and abnormal behaviors. We have developed a lightweight, independent HIDS that combines a signature-based ruleset with machine learning (ML) and deep learning (DL) models for detecting anomalies. A decision engine checks if commands are safe or malicious, allowing or blocking them from the local CPS execution queue. The proposed CPS-HIDS processes instructions in real-time from either remote or local administrators in a CPS setup. The signature-based system efficiently detects known attacks with minimal computing power. For anomaly detection, we are experimenting with compact, memory-efficient ML and DL models suited to the limited resources of an independent HIDS. The aim is to use simple algorithms capable of learning and adapting within the CPS environment.

## Dataset Description


To improve accuracy and to protect CPS from inflicting physical damage in its operating environment, an IDS has to understand domain specific knowledge of the instructions (commands, sensing and actuation) being passed to and from a CPS. Therefore, we have developed a Gcode based dataset to design an IDS that can understand CPS specifics from the OT security constraints and protect these from damaging instructions and behavioral anomalies. 
Supervised algorithms demand meticulously labeled datasets that encapsulate both benign and malicious traffic, strategically situated on the target network safeguarded by the IDS. Predominantly, ML-based IDS research hinges on network traffic captures. Currently, there is an evident gap in the availability of Gcode datasets tailored for 3D printers. To bridge this gap, we have meticulously crafted a comprehensive Gcode dataset that mirrors the diverse types of data and real-world behaviors pivotal for the robust training and evaluation of our CPS-HIDS. 
G-code is a sequential lines of instructions, each telling the 3D printer to perform a specific task. These lines are known as commands, and the printer executes them one by one until reaching the end of the code. very G-code command line follows a certain syntax. Each line corresponds to only one command, which can lead to codes that are awfully lengthy. While the term “G-code” is used to reference the programming language as a whole, it’s also one of two types of commands used in 3D printing: “General” and “Miscellaneous” commands.

  - General command lines are responsible for types of motion in a 3D printer. Such commands are identified by the letter ‘G’, as in G-commands. Besides controlling the three+ axes of movement performed by the printhead, they’re also in charge of filament extrusion.

  - The Miscellaneous commands, on the other hand, instruct the machine to perform non-geometric tasks. In 3D printing, such tasks include heating commands for the nozzle and bed, fan control, among many others – as we’ll see. Miscellaneous commands are identified with the letter ‘M’.

To empower ML models in detecting CPS intrusions, we painstakingly extracted a plethora of potential G-code features, cataloging their respective values as distinctive instances, as elucidated below:-
-  G0 & G1: Linear Motion : G0 & G1 commands are responsible for linear motion and extrusion 
  - G0 X[pos] Y[pos] Z[pos] F[rate] E[pos]
  - G1 X[pos] Y[pos] Z[pos] F[rate] E[pos]
- Other G-code commands:
  -   {G28} & {G29}: Auto-Home & -Bed Leveling: We call “homing” the process of setting the physical limits of all movement axes. The G28 command will perform this task by moving the printhead until it triggers endstops to acknowledge the limits.
  -   {G21}, Set Units to Millimeters
  -   {G91}: Set to Relative Positioning 
  -   {G92}, Set Position
  -   {G90} and {G91}: The G90 and G91 commands tell the machine how to interpret coordinates used for movement. G90 establishes “absolute positioning”, which is usually the default, while G91 is for “relative positioning”.
- M-code: These regulate non-movement operations, e.g. {M104} controls the temperature of extruder. We extracted
  - {M82}, Set extruder to absolute mode
  - {M84}, Stop idle hold on all axis and extruder
  - {M106} & {M107}: Fan Control, M106 turns a fan on and sets its speed. This is especially useful for the part cooling fan, as different speeds are required when printing the first layer and while bridging. The M107 command will power off a specified fan. If no index parameter is provided, the part cooling fan is usually the one to be shut down.
  - {M190}, S[temp], R[temp], T[index]; Wait for bed temperature to reach target temp
  - {M104}, S[temp], I[index]; Set Extruder Temperature
  - {M220}, Set speed factor override percentage
- Positioning or Directions Setting: 
  - {S}, Snnn (optional) Spindle index, defaults to 0. Duet 2 supports 4 spindles max.
  - {F}, Fnnn Set feedrate in mm/sec.
  - {E}, Enn minimum extrusion length of nozzle before a commanded/measured comparison is done, default 3mm. 

## Dataset Preparation
We processed dataset organiztion in such a way that it can be used for different machine learning tasks. Here’s an expanded breakdown of it:

1. *Subsequently, the dataset is systematically arranged*: This means after collecting or preparing the raw data, it is processed and organized into a specific format. The arrangement is done thoughtfully and in a methodical way, depending on the tasks the data is going to be used for.

2. *Varying distributions of features and samples*: This refers to how the dataset is partitioned or split, both in terms of:
   - **Features**: The individual measurable properties or attributes of the data (e.g., in a CPS, features might include system logs, network traffic data, sensor readings, etc.).
   - **Samples**: The individual instances or data points (e.g., multiple data logs, readings over time).
   
   The distributions can be altered to create different sub-datasets. For example, you could randomly sample from the dataset to create a balanced or imbalanced dataset (with different ratios of classes or labels). You might also select different combinations of features to be more suited for specific tasks.

3. *To facilitate distinct tasks*: This means that the dataset is restructured or partitioned in such a way that it becomes suitable for different machine learning objectives or tasks.

4. *Binary classification*: A task where the model classifies the data into one of two categories. For example, in an intrusion detection system (IDS), the task could be to classify whether network traffic is malicious or benign.

5. *Multi-class classification*: A task where the model classifies data into more than two categories. For instance, in an IDS, this could involve classifying the type of attack (e.g., DoS, phishing, malware, etc.).

6. *Anomaly detection*: This is a task where the model identifies data points that deviate significantly from the norm. In a CPS environment, anomaly detection would be used to flag abnormal behavior (e.g., an unexpected surge in sensor readings) that could indicate a system fault or security breach.

In summary, this process involves structuring the dataset in various ways to address different kinds of classification or detection tasks, depending on the objectives (binary, multi-class, anomaly detection). Each restructuring ensures that the model can learn the relevant features for the specific task at hand.

### CPS_HIDS includes:

**Benign Dataset:** A wide collection of open-source Gcode print files has been carefully gathered, containing commands used as safe and normal (benign) instructions for machines. These commands include:
- *Movement commands* like G1 and G21, which control precise movements of machine parts.
- *Control commands* like M28 and M109, which handle key operations like setting temperatures or writing to the SD card.
  
Each command, along with detailed parameters such as X, Y, Z (coordinates), F (feed rate), E (extruder), and S (speed), has been systematically classified as important features. This step is crucial for understanding the significance of each instruction within the machine's operation.

To make these Gcode instructions usable for machine learning models, the powerful Python library {gcodeparser} was used to convert the complex Gcode files into CSV format. This transformation makes the data much easier to analyze. Additionally, every benign (normal) instruction in the dataset is clearly labeled as 'No' or '0' to indicate that no suspicious or harmful activity is present.

The dataset includes Gcode files that follow the NIST RS-274 / ISO 6983-1:2009 standards and is designed for several machine learning tasks:
- *Supervised learning*, where models learn from labeled data,
- *Semi-supervised learning*, which can help develop mechanisms to update models over time,
- *Anomaly detection*, which identifies unusual patterns or activities within Cyber-Physical Systems (CPS).

This dataset provides a strong foundation for developing and testing intrusion detection mechanisms, as well as for improving the security of CPS environments.

**Malicious Dataset:** The creation of malicious data was carefully executed using Python’s {random} module to generate data that represents a wide variety of potential threats across the full operational space of the system. By using {random}, the malicious commands could be spread throughout different areas and scenarios, mimicking realistic attack patterns or abnormal behaviors that a Cyber-Physical System (CPS) might encounter.

For managing and structuring this data, Python’s {pandas} and {numpy} libraries were used extensively. These libraries allow for efficient data manipulation, enabling the seamless handling of large datasets, processing and transforming the data as required, and applying detailed labels to mark each malicious instance clearly. This comprehensive labeling process ensures that each command or instruction in the dataset is correctly identified as either normal or harmful.

Thousands of lines of Gcode data were meticulously generated, simulating a wide range of malicious activities within the CPS environment. Each of these lines was crafted to imitate potential security breaches or faults, such as commands that might cause machine malfunctions, unauthorized operations, or system disruptions.

The dataset was systematically constructed to create a large Gcode databank specifically for security testing. In this process:
- Every **malicious instruction** was clearly marked with 'Yes' or '1', indicating that the command is harmful or poses a threat.
  
This structured labeling of malicious instructions plays a key role in distinguishing between normal (benign) and harmful (malicious) actions within a machine learning model, making it possible to train or test models for tasks like anomaly detection and intrusion prevention in CPS. The dataset is extensive, ensuring that various types of attacks and anomalies are represented, allowing for robust detection mechanisms to be built and refined.

## Data Preprocessing 

Preprocessing is a critical and transformative step that must be done before deploying any AI or machine learning model. It ensures that the data is clean, consistent, and in the correct format for analysis. This process involves various tasks such as handling missing values, normalizing data, and other operations depending on the quality and structure of the dataset.

In this case, we specifically used a method called **zero-filling** to handle missing data. Zero-filling involves filling in any missing or empty values with zeroes. While this approach is simple and effective, it can sometimes introduce biases into the data. However, in our context of Gcode instrucitons execution, it is highly useful for maintaining the continuity and structure of the dataset.

In Gcode files, each instruction (and its corresponding parameters like X, Y, Z, etc.) must be executed in a specific sequential order for the machine to function correctly. As part of preprocessing, every instruction is marked based on whether it was executed or not:
- *Executed instructions* are labeled with a '1', and their parameter values are included on the same line.
- *Non-executed instructions* are labeled with a '0', indicating that they did not run.

By applying **zero-filling**, we ensure that any missing or non-executed instructions are clearly marked, which mirrors how the instructions are processed in real-world scenarios. This method guarantees that the dataset maintains its structural integrity, reflecting the actual flow of Gcode commands in Cyber-Physical Systems (CPS). Consequently, our preprocessing strategy ensures that the data is properly organized, facilitating accurate and reliable training for AI models that depend on this data.

## Multi-class and Semi-Supervised Dataset 

This dataset is structured to support both multi-class classification and semi-supervised learning tasks, which are crucial for developing robust AI models capable of detecting various types of activities or anomalies in a CPS environment.The classification is done based on the values of different Gcode parameters:

- *X, Y, and Z parameters*: These represent the positional coordinates in Gcode, controlling the movement of the machine in three-dimensional space. When the values of these parameters exceed a certain threshold, the instruction is classified as **'High'** in the "Drop" column. This labeling indicates that the command involves significant or potentially risky movement.

- *F, E, and S parameters*: These parameters control functions like feed rate (F), extruder movements (E), and speed (S). If any of these parameters exceed their respective thresholds, the instruction is labeled as **'Medium'** in the "Drop" column, representing a moderate level of activity or risk.

- *Remaining instructions*: For commands that don't exceed the thresholds for any of the above parameters, they are assigned the label **'Low'** in the "Drop" column, signifying lower activity levels or minimal risk.

Additionally, some instructions have been deliberately left **unlabeled** to facilitate **semi-supervised learning**. In semi-supervised learning, the model is trained on both labeled and unlabeled data, allowing it to infer patterns and make predictions based on the partially labeled dataset. This approach is especially useful for situations where fully labeled datasets are scarce or costly to create, enabling the AI model to learn and generalize better.

This carefully constructed dataset with **high**, **medium**, and **low** classifications, along with the inclusion of unlabeled data, provides a comprehensive framework for training models to handle both supervised and semi-supervised tasks. It reflects different levels of potential activity or risk, which is crucial for effective anomaly detection and intrusion prevention in Cyber-Physical Systems.

## Modeling of Behavioral Anomalies
Several Gcode print files containing the intricate start and end sequences of a printing job are meticulously utilized for dataset creation, meticulously aiding in the identification of specific, subtle patterns of normal and malicious behaviors. Anomalous instructions can be extraordinarily rare, for instance, appearing ominously only once in a colossal print file brimming with thousands of other legitimate instructions, or they may reappear persistently, causing frustrating print delays. Our robust method must adeptly address both these elusive rare and repetitive malicious events. If such events occur precisely in their expected positions, they will be validated and marked as legitimate instructions; however, if the same instructions are detected in abnormal, unexpected positions, they will be stringently excluded from the execution list. For empirical and rigorous testing purposes, three distinct anomalies have been expertly modeled, each ingeniously introducing a unique, conspicuous deviation from the expected behavior.
  - {The {G28} - Move to Origin (Home) Anomaly}: This command homes all printer axes and is usually used at the start. Using G28 elsewhere causes delays and is considered an anomaly.
  - {The {M104} - Set Extruder Temperature Anomaly}: This command is used to control temperature and is considered an anomaly if it occurs before the end of printing.
  - {The {M84} -  Stop idle hold Anomaly}: This anomaly disables or closes down opertion of a motor before the actual start of job or task close down sequence. 

## Sources
Collected from an operational reprap based 3D printer in a controlled environment with free Gcode printfiles downloaded from below sources:-
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
