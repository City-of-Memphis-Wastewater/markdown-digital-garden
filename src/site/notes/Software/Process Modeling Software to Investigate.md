---
{"dg-publish":true,"permalink":"/software/process-modeling-software-to-investigate/","noteIcon":"","created":"2025-02-04T09:28:35.074-06:00"}
---

## Highlights:
Information:
- [Balanced Flow State](https://www.pipeflow.com/public/PipeFlowExpertSoftwareHelp/html/BalancedFlowState.html)
- [Loops, Nodes and Pipes](https://www.pipeflow.com/public/PipeFlowExpertSoftwareHelp/html/LoopsNodesandPipes.html)
- [Design of a Hydronic Pipe Network — Pipe and Duct Flow Calculations with Python](https://tomlxxvi.github.io/jbFluidFlow/closed_pipe_network_design.html)
- [Pipe network analysis - Wikipedia](https://en.wikipedia.org/wiki/Pipe_network_analysis)
- [Computer Aided Analysis of Flow and Pressure in Pipe Networks](https://doras.dcu.ie/18301/1/Mohamed_A_Salam_Ali_20130306165806.pdf)
- [WaterCAD example](https://www.bing.com/ck/a?!&&p=30665b78b66989c9f983995a2a3110e29dd84f9f4417dd800c6093c945fb97dfJmltdHM9MTczODYyNzIwMA&ptn=3&ver=2&hsh=4&fclid=138f8086-8921-64a9-205a-95fb884565f6&psq=pipe+and+tank+and+valve+network+analysis&u=a1aHR0cHM6Ly9lbmdyeGl2Lm9yZy9wcmVwcmludC9kb3dubG9hZC8xNzU1LzM1NzgvMjY1Mw&ntb=1)
- [Analsis of Centrifugal Pumps and Pipe Network Flow](https://cse.sc.edu/~gatzke/460/B-Pumps-2008.pdf)
- [CheGuide ‐ Pipe Network Analysis](https://www.cheguide.com/pipe_network.html)
- [Use of Excel-Solver as an Optimization Tool in Design of Pipe Network](http://article.sapub.org/10.5923.j.ijhe.20130204.01.html)
- [(PDF) UTILIZING EXCEL SOLVER FOR COMPLEX PIPE NETWORK ANALYSIS AND DEVELOPMENT OF AN EDUCATIONAL TOOL](https://www.researchgate.net/publication/385909951_UTILIZING_EXCEL_SOLVER_FOR_COMPLEX_PIPE_NETWORK_ANALYSIS_AND_DEVELOPMENT_OF_AN_EDUCATIONAL_TOOL)


Keywords:
- Pipe network

Key goal:
- Modelling of network that includes pipe, tanks, and values. Density designation for separate fluids would be great.

Conclusion:
- Look at EPANET, the Python fluids package, and QSDsan.
- Play with Dynamita SUMO free version.
- Talk to Henry about what to expect from the Hach WIMS Digital Twin
- Know what we already have available through Emerson Ovation.
- Create basic balance model in Excel. 


Free software candidates:
- [System - QSDsan 1.4.1](https://qsdsan.readthedocs.io/en/latest/tutorials/6_System.html)
- [EPANET | US EPA](https://www.epa.gov/water-research/epanet?utm_source=chatgpt.com)
- [fluids · PyPI](https://pypi.org/project/fluids/)

![Pasted image 20250204092854.png](/img/user/Pasted%20image%2020250204092854.png)

## Query 1
ChatGPT Query:
*Please recommend free process modeling softwares Linux, windows, or browser software for modelling water flow through a water treatment plant. I do not need CFD. Examples of software include FluidSim and Dynamita Sumo.*

Answer:
> Certainly! Based on your requirements for free software to model water flow through a water treatment plant (excluding CFD), here are some options:
> 
> 1. **STOAT** (sales model): Developed by WRc, STOAT is a PC-based tool designed to dynamically simulate the performance of wastewater treatment works. It can model individual treatment processes or entire treatment plants, including sludge processes and recycling.
>     
>     [wrcgroup.com](https://www.wrcgroup.com/services/stoat-wastewater-works-modelling-freeware/?utm_source=chatgpt.com)
>     
> 2. **GPS-X Lite** (sales model): Offered by Hydromantis, GPS-X Lite is a free, simplified version of the GPS-X dynamic wastewater treatment plant simulator. It complements the textbook "Theory and Practice of Water and Wastewater Treatment" and provides a user-friendly interface for modeling various treatment processes.
>     
>     [hydromantis.com](https://www.hydromantis.com/GPS-X-Limited/download.html?utm_source=chatgpt.com)
>     
> 3. **EPANET**: Developed by the U.S. Environmental Protection Agency, EPANET is a public domain software application used worldwide to model water distribution systems. It performs extended-period simulations of hydraulic and water-quality behavior within pressurized pipe networks and is useful for understanding the movement and fate of drinking water constituents within distribution systems.
>     
>     [epa.gov](https://www.epa.gov/water-research/epanet?utm_source=chatgpt.com)
>     
> 4. **QSDsan** (truly free, GitHub): An open-source tool written in Python, QSDsan is designed for the quantitative sustainable design of sanitation and resource recovery systems. It integrates process modeling, simulation, techno-economic analysis, and life cycle assessment, making it suitable for evaluating various treatment scenarios.
>     
>     [arxiv.org](https://arxiv.org/abs/2203.06243?utm_source=chatgpt.com)
>     [QSDsan](https://qsdsan.com/)
>     [QSD-Group/QSDsan: Quantitative Sustainable Design (QSD) of sanitation and resource recovery systems.](https://github.com/QSD-Group/QSDsan)
>     
>     
> 
> Each of these tools offers unique features that can assist in modeling water flow through treatment plants. I recommend exploring them to determine which best fits your specific needs.

People to talk to about this: [[People/Henry Nakayama\|Henry Nakayama]], [[People/Stacy Bullard\|Stacy Bullard]]
Check the functionality we can expect from [[Assignments/Hach WIMS Improvements\|Hach WIMS Improvements]]: talk to [[People/Peter Gabor\|Peter Gabor]]


Paid software:
- EnviroSuite (sales model): [Concept Design and Process Simulation Software for Water Treatment Applications](https://water.envirosuite.com/)
- [SUMO | dynamita](https://dynamita.com/products.html) (sales model)
- [Inctrl SIMBA Integrated Simulation Software For Urban Water Systems](https://www.environmental-expert.com/software/simba-water-version-50-process-simulator-for-modeling-simulation-optimization-and-management-of-wast-454291)
- VMGSim: [Enterprise Business Solutions W.L.L|Process Modeling and Simulation Optimization](https://ebs-group.net/Process-Modeling-and-Simulation-Optimization.html)
- [KYPipe Products | KYPipe](https://kypipe.com/kypipe-products/)


Free software:
- [fluids · PyPI](https://pypi.org/project/fluids/); GitHub: [CalebBell/fluids: Fluid dynamics component of Chemical Engineering Design Library (ChEDL)](https://github.com/CalebBell/fluids)
- [CBE30338 | Chemical Process Control](https://jckantor.github.io/CBE30338/)
- [chemicals · PyPI](https://pypi.org/project/chemicals/)
- [samadritakarmakar/LiquiNet: LiquiNet is a free, open source, pipe network software, that can solve complex Water, or any other Liquid, Pipe Network problems easily. Based on Paper published by Anthony G. Collins in May 1973, it applies Finite Element Method.](https://github.com/samadritakarmakar/LiquiNet)





## Query 2

ChatGPT Query: 
*Please recommend a pipe network building software that can balance pressures, flows, and volumes. It should include tanks and valves.*
Answer:
> A great free software option for building and analyzing pipe networks, including pressure balancing, flow rates, volumes, tanks, and valves, is:
> 
> ### **1. EPANET** (Windows, Open-source)
> 
> - Developed by the **U.S. EPA**, EPANET is a widely used software for **simulating water distribution networks**.
> - It models **pipes, pumps, valves, and storage tanks** to compute **flows, pressures, and water quality** over time.
> - Can simulate **pump operations, pressure-dependent demands, and tank level variations**.
> - Supports **extended period simulations** to analyze system performance over time.
> - **Download:** [EPA Website](https://www.epa.gov/water-research/epanet)
> 
> ### **2. WaterCAD (Bentley)** (Paid, but has a trial version)
> 
> - Advanced water distribution modeling software for pipe networks, tanks, pumps, and valves.
> - Performs hydraulic and energy cost analyses.
> - **Website:** Bentley WaterCAD
> 
> ### **3. OpenFlows WaterGEMS (Bentley)** (Paid, but offers a trial)
> 
> - A more advanced version of WaterCAD with **GIS integration and demand forecasting**.
> - Useful for large-scale utility water distribution modeling.
> - **Website:** Bentley WaterGEMS
> 
> ### **4. Pipe-Flo Lite** (Windows, Limited Free Version)
> 
> - Models complex piping systems, including **flow balancing, pressure calculations, and tank interactions**.
> - The **Lite** version is free but has limited features.
> - **Website:** Engineered Software Inc.
> 
> ### **5. KYPipe (KYPipe Freeware)**
> 
> - Used for **water, gas, and steam network modeling** with pipes, tanks, and valves.
> - Includes **steady-state and transient flow analysis**.
> - Freeware version available with limited features.
> - **Website:** [KYPipe](https://www.kypipe.com/)
> 
> If you need a completely **free and open-source** tool, **EPANET** is your best choice. Would you like recommendations for software with specific features like surge analysis or real-time monitoring?