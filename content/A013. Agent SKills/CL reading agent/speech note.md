# Slide 9 (15 Seconds)

Here I am gonna introduce the methodology, this study took place in a "Critical Literacy: Theme-based Reading and Writing" course with 31 EFL freshmen with a B2 proficiency. The research process is eight weeks using surveys, interviews, reading logs, and the AI agent.

# Slide 10 (15 Seconds)

To guide the students' reading interaction, we developed thirteen reading log questions based on Lewison et al.'s framework. In the questions, students need to think about missing voices, multiple viewpoints and sociopolitical issues, to propose social action plans.

# Slide 11 (30 Seconds)

To make and improve the agent, this study develops 3 versions of agent. In the prototype and pilot testing using Dify, feedback showed the agent spoke unnaturally with not much cognitive and affective support. 

For the official one, we switched to Gemini due to technical considerations. By refining the prompt structure including roles, skills, and integrating Retrieval-Augmented Generation, which is the knowledge base of 2 CL frameworks, and the agent is said to be empathetic and supportive.

# Slide 12 (45 Seconds)

While the core features of AI agent design. 
The first is Contextualization from Transactional Theory that the agent can fit into the student’s self-selected reading materials for personal interests. Also, the agent can read uploaded documents or links to know student's reading context. 

Second, Immediate Feedback on Self-Determination Theory. The agent initiates an engaging dialogue by  gently asking what motivated them to select it. 

Third, the agent serves as the companion to encourage students to share ideas in the dialogue according to CL.

# Slide 13 (30 Seconds)

Fourth, As for Adaptive Probing on Social Constructivism, the agent pushes students to explore hidden values or textual "blind spots". 

Fifth, guided by Reflective Practice, the agent reviews all the questions discussed before and generates an open-ended synthetic question to engage students in much deeper reflection.

# Slide 14 (15 Seconds)

For the overall design of AI reading agent, as seen in the figure, it is divided into 3 parts: 

agent.md for the role and CL context, 

skill.md for the execution and rules, 

and the RAG containing both Lewison et al., which is the concrete structure of CL, and Paul's CL frameworks, which is the soft skills for agent's responses. 
# Slide 15 (30 Seconds)

Finally, diving deeper into the workflow, as seen in the figure,

agent.md as the role taken by agent, it acts as a supportive companion that takes students' CL ideas as the top importance.

while the interaction procedures are:  asking for article information, processing 13 questions, summarizes the thoughts to confirm if it meets what student thinks, and then synthesizing the discussion with a larger question, and finally generating a final summary and expressing appreciation for the student's effort.

now Leo will introduce data analysis.