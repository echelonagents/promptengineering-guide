# Case Example: Transaction reconciliation

## Account ledger reconciliation

The aim of the reconciliation process is to reconcile two account ledgers at a transaction level:

- **source of truth**: actual bank statements, which is always lagging due to the limitation of manual MFA for statement downloading. The actual bank statements lack tagging to user specific categories, budget accounts and taxonomy.
- **digital twin**: a user specific ledger, updated in real-time, with transactions tagged with user data model taxonomy.

## Motivation and aims for AI automation

The motivation for automating with AI:

- Current process is 
    - labor intensive, time-consuming
    - Subject to human-error, fatigue
- Largely deterministic and structured, making it a good candidate for AI-assisted automation

## Reconciliation workflow

### 00 Application load
The user logs in to the application and begins a new session, the state is updated and the application detects the current state and status of reconciliations by month and account. The application guides the user to next steps and issues to resolve if any

### 01 Statement upload
User manually downloads statements as either PDF or Excel compatible (.csv, .xlsx) file format from the bank authenticated using MFA and uploads them using a file uploader UI feature that drops the files to a designated cloud storage location such as an AWS S3 bucket 

### 02 Statement DB load
Robotic Process Automation (RPA) ingests, parses the statements, performs cleanup and formatting and uploads the transaction data to a structured SQL database. 

    _AI enhancement_ Although a fully RPA process without LLM may suffice for well structured statements, this process could optionally be enhanced with pre and post processing steps added with an LLM Agent to perform pre-upload checks and to review and resolve any processing errors in the first pass statement ingestion. 

    _Database_ The database can take many types of forms. On AWS, for example, could include RDS, Aurora or even an SQLite DB stored in S3 bucket

### 03 Transaction matching
The transaction matching is the main reconciliation workflow and can include multiple passes and methods, and update with learning as the program is exposed and trained from user input from past reconcilation cases.

    _Input validation pre-processing_: RPA process performs basic pre-processing input validation for easily detectable data integrity issues such as NULL values in non-NULL fields, duplicate transactions or empty ledgers.

    _LLM Agent sanitation pre-processing_: LLM Agent has a peek at the data to check for potential problematic cases, perform custom clean-up, formatting, tagging and pre-formatting transactions or run pre-defined clean-up operations not yet built into the RPA pre-processing.

    _RPA Special cases pre-processing_: Custom pre-processing runs first by RPA process for pre-defined known special cases involving recurring transaction pairs and difficult to handle cases of many-to-many transaction pairs that have been either learned or manually defined by the user.

    _transaction pre-processing_: Individual transactions are pre-processed with standard NLP processing to format into standard features [date, account, amount, status, currency, payee, description] remove stopwords, POS tagging, lemmatization, and custom tagging from user-specific model (learned + user-defined)

    _Hybrid classification model_: transaction pairs are classified from a hybrid similarity match (no training required) + trained model as either matched or not [0, 1]. The two models (similarity, trained) generate independent scores and the final score is taken as a weighted combination of the two component scores. Over time, the weight of the trained model can be dynamically incremented as accuracy improvement from exposure to more use cases.

    _Similarity model_: The similarity model can utilize different methods of similarity matching, each with different trade-offs of computational cost and accuracy. Simple token matching can be performed at high speed and low computational costs, whereas sentence embedding, BERT models can add accuracy boost with a noticeable trade-off in computational costs.

    _Trained classification model_: There are many types of standard classification models with Decision tree and Random Forest a good first choice and Neural Networks and Multi-Layered Neural Networks as advanced options.

### 04 Repair edits list
A list of repair edits is generated that will close the reconciliation gap between the ending balance between the reference source-of-truth and the digital twin ledgers.  Each transaction edit is either a REMOVE, ADD or UPDATE.  One fail-safe method for ensuring the process will always generate a solution is to begin with a naive set of edits: REMOVE ALL digital twin txns, ADD ALL source-of-truth, and then remove known matched transactions from the prior transaction matching step pair-wise that sum to zero, ensuring that each step of removed matched transactions preserves the condition of matched ending balance between source-of-truth and digital twin.

### 05 RPA repair
An RPA process can automatically process repair edits to the digital twin some limited cases of qualified RPA-allowed repairs. Examples of allowed cases may include only amount update by < +/- some threshold amount, or for foreign currency transactions or variable bills which are expected to vary from a range of expected amount in some range.

### 06 LLM Agent review
An LLM Agent can review the residual edits and follow a set of guidelines to apply repairs not caught by the RPA process, and for residual repairs to propose edits to the user which do not meet the guidelines for Agent repair. These guidelines can be seeded by the user and updated with learning and feedback by the AI Agent.

### 07 Manual repair
For any residual edits that do not qualify for either RPA or Agent repair, the user is prompted to manually repair either by manually matching transaction with instruction to update the digital twin to match the source-of-truth amount, or by manually updating the digital twin transaction. For example, in case where the transactions were not matched due to a miss-spelling or incorrect transaction category tagging, the user may decide to update the tag and regeneration the edits list in order to prevent corruption of the training model by feeding it bad training feedback cases.  

### 08 Agent session review
An LLM Agent is prompted with the session details, and plans workflow for model, guideline updates.

### 09 Training feedback
The complete set of matched transactions and edit repairs is stored to train each of the models involved, the classifier, the RPA repair and the LLM Agent guide, with specific workflows triggered by the output of the Agent session review.

### 10 Session close
The downstream reports are updated from the updated digital twin and the session status is updated as closed. Any cleanup and logging are recorded to log and record session activity.

## Tech stack

- **Embedding model**: all-MiniLM or similar for vector similarity
- **Vector store**: FAISS or Qdrant
- **LLM agent orchestrator**: LangChain or CrewAI
- **Document loader**: PyPDF, CSV parser, or structured ingest from databases
- **Output**: Markdown or spreadsheet with reconciliation results

## Other use cases

02. Case complaint classification
03. Job search profile matching
04. Personal finances coach
