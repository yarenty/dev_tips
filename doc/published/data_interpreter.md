- Data Interpreter is an LLM-based agent designed to automatically solve data science problems end-to-end. 
- It incorporates Hierarchical Graph Modeling and Programmable Node Generation to manage complex tasks and improve code generation. 
- Experiments show Data Interpreter achieves significant performance boosts on various datasets, including a 25% increase on InfiAgent-DABench. 
**Introduction** 
- LLMs show adaptability across applications, but data science performance remains limited due to complex, interconnected tasks. 
- Data Interpreter addresses these challenges by reframing data science workflows as a Hierarchical Graph Modeling problem. 
- Programmable Node Generation automates node refinement and verification, improving workflow robustness and precision. 
**Related Work** 
- LLMs have automated data science tasks, initially focusing on code generation and function-calling mechanisms. 
- Recent work introduces LLM-based agents for data analysis but lacks evaluation on predictive tasks and full pipeline construction. 
- Enhancing LLMs with external tools and graph-based planning is being explored, yet struggles with multi-step, task-dependent data science problems. 
**Methodology** 
- Hierarchical Graph Modeling decomposes data science problems into manageable tasks and actions, represented as nodes and edges .
- The solving process is represented as a series of sub-processes that can be directly solved and verified. 
- Programmable Node Generation refines and verifies subproblems iteratively to improve code generation results and robustness. 
**Hierarchical Graph Modeling for Complex Task Decomposition** 
- Data science problems are organized via a hierarchical structure that initially decomposes the intricate data science problem into manageable tasks. 
- Solving a data science problem can be formulated as applying a series of operators to produce an output. 
- The framework represents all subprocesses as nodes, forming a graph that embodies the entire function. 
**Task Graph: Iterative Graph Refinement** 
- Data Interpreter dynamically adjusts the task graph in response to changing environments through iterative graph optimization. 
- Data Interpreter uses a task graph generator to initialize the task graph. 
- The task graph generator manages tasks, monitors statuses/dependencies, and adjusts the graph by adding, removing, or modifying tasks as needed. 
**Action Graph: Programmable Node Generation** 
- An action node represents an executable code snippet that encapsulates the computational logic required for task execution. 
- Effective tool selection and integration play a crucial role in the success of task execution. 
- Unlike conventional LLM-based agent frameworks, Data Interpreter generates comprehensive code snippets that seamlessly integrate selected tools within the broader logic of the task. 
**Experiments** 
- The experimental setup includes InfiAgent-DABench, ML-Benchmark, an open-ended task benchmark, and the MATH dataset. 
- InfiAgent-DABench evaluates data analysis tasks, while ML-Benchmark assesses real-world machine learning challenges .
- The open-ended task benchmark verifies dynamic data handling, and the MATH dataset tests mathematical problem-solving. 
**Main Result** 
- Data Interpreter achieved a 25% performance boost on InfiAgent-DABench with longer context windows in LLMs. 
- On ML-Benchmark, Data Interpreter outperformed AutoGen and OpenDevin with a comprehensive score of 0.95. 
- Data Interpreter achieved a completion rate of 0.97 on open-ended tasks, a substantial improvement compared to baselines, and showed a 26.5% relative improvement compared to AutoGen on the MATH dataset. 
**Ablation Study** 
- Iterative graph refinement improved performance by 0.48, enhancing dataset preparation and real-time tracking. 
- Programmable node generation further boosted the comprehensive score by 10.6%, reaching 0.94. 
- Data Interpreter showed further improvement in task completion with GPT-4o and GPT-4o-mini. 
**Conclusion** 
- Data Interpreter tackles data science challenges using hierarchical graph representation and iterative task refinement. 
- This framework enhances data analysis, machine learning performance, and reasoning capabilities. 
- Evaluations demonstrate Data Interpreter's outperformance compared to open-source frameworks in various tasks. 
**References** 
- This section lists all the references used in the paper. 
**Limitations** 
- The diversity and complexity of datasets used were insufficient, being limited to entry-level Kaggle datasets. 
- Precise self-improvement is lacking, as the model does not use numerical feedback from multiple experiences to refine specific strategies. 
- Full-scale evaluation on mathematical problems was limited due to budget constraints. 
**Broader Impact** 
- The work has the potential to significantly reduce costs associated with customized data science tasks, empowering professionals. 
- Flexibility in tools integration, while convenient, comes with potential risks such as security vulnerabilities if users provide malicious code. 
- Mitigation strategies include prompting Data Interpreter to check codes before generation and collaborating with LLMs that adhere to robust safety policies. 
**Implementation Details** 
- This section includes implementation details about the programmable node generation process with tools. 
- An example of a tool schema is provided for a feature engineering tool. 
- It also lists the tools of the Data Interpreter and the prompts used for tool utilization. 
**Experiment Details** 
- Additional information about the datasets (InfiAgent-DABench, ML-Benchmark, Open-ended task benchmark, MATH dataset) is provided. 
- The evaluation metrics used (completion rate, normalized performance score, comprehensive score) are defined. 
- Additional results of the ML-Benchmark and MATH dataset, along with ablation study results, are presented. 
**Additional Examples** 
- An example of a task graph is provided for a machine operational status prediction scenario. 
- Runtime results of the Data Interpreter for machine learning, webpage imitation, and math problem-solving are showcased. 
- Additional results of open-ended tasks and data visualization capabilities are illustrated. 
**Details of Datasets** 
- It also includes a description of the ML-Benchmark dataset. 
- Several typical open-ended tasks are showcased, including necessary data, user requirements, and assessment pipelines .
- Table 9 provides details of the ML-Benchmark dataset, including dataset name, description, user requirements, and metrics .
