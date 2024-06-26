---
title: 3. Development-time threats
weight: 4
---
## 3.0 Development-time threats - Introduction
**Background:**

Data science (data engineering and model engineering - for machine learning often referred to as _training phase_) uses a development environment typically outside of the regular application development scope, introducing a new attack surface. Data engineering (collecting, storing, and preparing data) is typically a large and important part of machine learning engineering. Together with model engineering, it requires appropriate security to protect against data leaks, data poisoning, leaks of intellectual property, and supply chain attacks (see further below). In addition, data quality assurance can help reduce risks of intended and unintended data issues.

**Particularities:**

- Particularity 1: the data in the AI development environment is real data that is typically sensitive, because it is needed to train the model and that obviously needs to happen on real data, instead of fake data that you typically see in standard development environment situations (e.g. for testing). Therefore, data protection activities need to be extended from the live system to the development environment.
- Particularity 2: elements in the AI development environment (data, code, configuration & parameters) require extra protection as they are prone to attacks to manipulate model behaviour (called _poisoning_)
- Particularity 3: source code, configuration, and parameters are typically critical intellectual property in AI

ISO/IEC 42001 B.7.2 briefly mentions development-time data security risks.

**Controls for development-time protection:**

- See [General controls](/goto/generalcontrols/)
- The below control(s), each marked with a # and a short name in capitals

#### **#DEVDATAPROTECT**
(development-time infosec). Development data protect: protect (train/test) data, source code, configuration & parameters

This data protection is important because when it leaks it hurts confidentiality of intellectual property and/or the confidentiality of train/test data which also may contain company secrets, or personal data for example. Also the integrity of this data is important to protect, to prevent data or model poisoning.

Training data is in most cases only present during development-time, but there are exceptions:
  - A machine learning model may be continuously trained with data collected runtime, which puts (part of the) train data in the runtime environment, where it also needs protection
  - For GenAI, information can be retrieved from a repository to be added to a prompt, for example to inform a large language model about the context to take into account for an instruction or question. This principle is called _in-context learning_. For example [OpenCRE-chat](https://opencre.org/chatbot) uses a repository of requirements from security standards to add to a user question so that the large language model is more informed with background information. In the case of OpenCRE-chat this information is public, but in many cases the application of this so-called Retrieval Augmented Generation (RAG) will have a repository with company secrets or otherwise sensitive data. Organizations can benefit from unlocking their unique data, to be used by themselves, or to be provided as service or product. This is an attractive architecture because the alternative would be to train an LLM or to finetune it, which is expensive and difficult. An RAG approach may suffice. Effectively this puts the repository data to the same use as training data is used: control the behaviour of the model. Therefore, the security controls that apply to train data, also apply to this run-time repository data.

Protection strategies:

- Encryption of data at rest  
  Links to standards:
  - ISO 27002 control 5.33 Protection of records. Gap: covers this control fully, with the particularities
  - [OpenCE on encryption of data at rest](https://www.opencre.org/cre/400-007)
- Technical access control for the data, to limit access following the least privilege principle  
  Links to standards:
  - ISO 27002 Controls 5.15, 5.16, 5.18, 5.3, 8.3. Gap: covers this control fully, with the particularities
  - [OpenCRE](https://www.opencre.org/cre/724-770)
  - Centralized access control for the data  
  Links to standards:
  - There is no ISO 27002 control for this
  - [OpenCRE](https://www.opencre.org/cre/117-371)
- Operational security to protect stored data  
  Links to standards:
  - Many ISO 27002 controls cover operational security. Gap: covers this control fully, with the particularities.
    - ISO 27002 control 5.23 Information security for use of cloud services
    - ISO 27002 control 5.37 Documented operating procedures
    - Many more ISO 27002 controls (See OpenCRE link)
  - [OpenCRE](https://www.opencre.org/cre/862-452)
- Logging and monitoring to detect suspicious manipulation of data, (e.g. outside office hours)  
  Links to standards:
  - ISO 27002 control 8.16 Monitoring activities. Gap: covers this control fully
  - [OpenCRE on Detect and respond](https://www.opencre.org/cre/887-750)

#### #DEVSECURITY
Description: Development security: sufficient security of the AI development infrastructure, also taking into account the sensitive information that is typical to AI: training data, test data, model parameters and technical documentation. This can be achieved by adding these assets to the existing security management system. Security involves for example screening of development personnel, protection of source code/configuration, virus scanning on engineering machines.
> Category: management  
> Permalink owaspai.org/goto/devsecurity/

Apart from the AI-specific assets there is also the AI-specific supply chain of data and models, plus the fact that obtained software components are running in the development environment instead of in test, acceptance or production. This creates new risks of these components being a threat to development security. See [SUPPLYCHAINMANAGE](/goto/supplychainmanage/).

Links to standards:

- ISO 27001 Information Security Management System, with the particularity

#### #SEGREGATEDATA
(development-time infosec). Segregate data: store sensitive training or test data in a separated environment with restricted access.

Links to standards:

- ISO 27002 control 8.31 Separation of development, test and production environments. Gap: covers this control partly - the particularity is that the development environment typically has the sensitive data instead of the production environment - which is typically the other way around in non-AI systems. Therefore it helps to restrict access to that data within the development environment. Even more: within the development environment further segregation can take place to limit access to only those who need the data for their work, as some developers will not be processing data.

#### #CONFCOMPUTE
(development-time infosec). 'Confidential compute': If available and possible, use features of the data science environment to hide training data and model parameters from model engineers - even while it is in use.

Links to standards:

- Not covered yet in ISO/IEC standards

#### #FEDERATEDLEARNING
Description: Federated (or better: federated) learning can be applied when a training set is distributed over different organizations, preventing that the data needs to be collected in a central place - increasing the risk of leaking.  
> Category: development-time data science  
> Permalink: https://owaspai.org/goto/federatedlearning/

Federated Learning is a decentralized Machine Learning architecture wherein a number of clients (e.g. sensor or mobile devices) participate in collaborative, decentralized, asynchronous training, which is orchestrated and aggregated by a controlling central server. Advantages of Federated Learning include reduced central compute, and the potential for preservation of privacy, since training data may remain local to the client.

Broadly, Federated Learning generally consists of four high-level steps: First, there is a server-to-client broadcast; next, local models are updated on the client; once trained, local models are then returned to the central server; and finally, the central server updates via model aggregation.

**Federated machine learning benefits & use cases**  
Federated machine learning may offer significant benefits for organizations in several domains, including regulatory compliance, enhanced privacy, scalability and bandwidth, and other user/client considerations.  
- **Regulatory compliance**. In federated machine learning, data collection is decentralized, which may allow for greater ease of regulatory compliance. Decentralization of data may be especially beneficial for international organizations, where data transfer across borders may be unlawful.
- **Enhanced confidentiality**. Federated learning can provide enhanced conidentiality, as data does not leave the client, minimizing the potential for exposure of sensitive information.
- **Scalability & bandwidth**. Decreased training data transfer between client devices and central server may provide significant benefits for organizations where data transfer costs are high. Similarly, federation may provide advantages in resource-constrained environments where bandwidth considerations might otherwise limit data uptake and/or availability for modeling. Further, because federated learning optimizes network resources, these benefits may on aggregate allow for overall greater capacity & flexible scalability.  
- **Data diversity**. Because federated learning relies on a plurality of models to aggregate an update to the central model, it may provide benefits in data & model diversity. The ability to operate efficiently in resource-constrained environments may further allow for increases in heterogeneity of client devices, further increasing the diversity of available data.

**Challenges in federated machine learning**  
- **Remaining risk of data disclosure by the model**. Care must be taken to protect against  _data disclosure by use_ threats (e.g. membership inference), as sensitive data may still be extracted from the model/models. Therefore, _model theft_ threats also need mitigation, as training data may be disclosed from a stolen model. The federated learning architecture has specific attack surfaces for _model theft_ in the form of transfering the model from client to server and storage of the model at the server. These require protection.
- **More attack surface for poisoning**. Security concerns also include attacks via data/model poisoning; with federated systems additionally introducing a vast network of clients, some of which may be malicious.
- **Device Heterogeneity**. User- or other devices may vary widely in their computational, storage, transmission, or other capabilities, presenting challenges for federated deployments. These may additionally introduce device-specific security concerns, which practitioners should take into consideration in design phases. While designing for constraints including connectivity, battery life, and compute, it is also critical to consider edge device security.
- **Broadcast Latency & Security**. Efficient communication across a federated network introduces additional challenges. While strategies exist to minimize broadcast phase latency, they must also take into consideration potential data security risks. Because models are vulnerable during transmission phases, any communication optimizations must account for data security in transit.
- **Querying the data creates a risk**. When collected data is stored on multiple clients, central data queries may be required for analysis work, next to Federated learning. Such queries would need the server to have access to the data at all clients, creating a security risk. In order to analyse the data without collecting it, various Privacy-preserving techniques exist, including cryptographic and information-theoretic strategies, such as Secure Function Evaluation (SFE), also known as Secure Multi-Party Computation (SMC/SMPC). However, all approaches entail tradeoffs between privacy and utility.

Links to standards:

- Not covered yet in ISO/IEC standards

#### #SUPPLYCHAINMANAGE
(development-time infosec) Supply chain management: Managing the supply chain to minimize the security risk from externally obtained elements. In regular software engineering these elements are source code or software components (e.g. open source). The particularities for AI are:
1. supplied elements can include data and models, 
2. many of the software components are executed development-time instead of just in production (the runtime of the application),
3. as explained in the development-time threats, there are new vulnerable assets during AI development: training data and model parameters.

Security risks in obtained data or models can arise from accidental mistakes or from manipulations - just like with obtained source code or software components.

The AI supply chain can be complex. Just like with obtained source code or software components, data or models may involve multiple suppliers. For example: a model is trained by one vendor and then fine-tuned by another vendor. Or: an AI system contains multiple models, one is a model that has been fine-tuned with data from source X, using a base model from vendor A that claims data is used from sources Y and Z, where the data from source Z was labeled by vendor B.
Because of this supply chain complexity, data and model provenance is a helpful activity.  The Software Bill Of Materials (SBOM) becomes the AI Bill Of Materials (AIBOM) or Model Bill of Material (MBOM).

Standard supply chain management includes provenance & pedigree, verifying signatures, using package repositories, frequent patching, and using dependency verification tools.

As said, in AI many of the software components are executed development-time, instead of just in production. Data engineering and model engineering involve operations on data and models for which often external components are used (e.g. tools such as Notebooks, or other MLOps applications). Because AI development has new assets such as the data and model parameters, these components pose a new threat. To make matters worse, data scientists also install dependencies on the Notebooks which makes the data and model engineering environment a dangerous attack vector and the classic supply chain guardrails typically don’t scan it.

See [MITRE ATLAS - ML Supply chain compromise](https://atlas.mitre.org/techniques/AML.T0010).

Links to standards:

- ISO  Controls 5.19, 5.20, 5.21, 5.22, 5.23, 8.30. Gap: covers this control fully, with said particularity, and lacking controls on data provenance.
- ISO/IEC AWI 5181 (Data provenance). Gap: covers the data provenance aspect to complete the coverage together with the ISO 27002 controls - provided that the provenance concerns all sensitive data and is not limited to personal data.
- ISO/IEC 42001 (AI management) briefly mentions data provenance and refers to ISO 5181 in section B.7.5
- [OpenCRE](https://www.opencre.org/cre/613-285)

---

## 3.1. Broad model poisoning development-time
Description: manipulating model behaviour by altering training data, engineering, or model parameters during development-time

Impact: Integrity of model behaviour is affected, leading to issues from unwanted model output (e.g. failing fraud detection, decisions leading to safety issues, reputation damage, liability).

The type of impact on behaviour using broad model poisoning is typically more profound than with an evasion attack, for example:

- Backdoors - which trigger unwanted responses to specific input variations (e.g. a money transaction is wrongfully marked as NOT fraud because it has a specific amount of money for which the model has been manipulated to ignore). Other name: _Trojan attack_
- Unavailability by sabotage, leading to e.g. business continuity problems or safety issues

This poisoning is **hard to detect** once it has happened: there is no code to review in a model to look for backdoors, the model parameters make no sense to the human eye, and testing is typically done using normal cases, with blind spots for backdoors. This is the intention of attackers - to bypass regular testing. The best approach is 1) to prevent poisoining by protecting development-time, and 2) to assume training data has been compromised.

References

- [Summary of 15 backdoor papers at CVPR '23](https://zahalka.net/ai_security_blog/2023/09/backdoor-attacks-defense-cvpr-23-how-to-build-and-burn-trojan-horses/)


**Controls for broad model poisoning:**

- See [General controls](/goto/generalcontrols/), especially [Limiting the effect of unwanted behaviour](/goto/limitunwanted/)
- See [controls for development-time protection](/goto/developmenttimeintro/)
- The below control(s), each marked with a # and a short name in capitals
  
#### #MODELENSEMBLE
(development-time data science). Model ensemble: include the model as part of an ensemble, where each model is trained in a separately protected environment. If one model's output deviates from the others, it can be ignored, as this indicates possible manipulation.

Links to standards:
  - Not covered yet in ISO/IEC standards


### 3.1.1. Data poisoning
> Permalink: owaspai.org/goto/datapoison
 
The attacker manipulates (training) data to affect the algorithm's behavior. Also called _causative attacks_. There are mutiple ways to do this (attack vectors):
- Changing the data while in storage during development-time (e.g. by hacking the database)
- Changing the data while in transit to the storage (e.g. by hacking into a data connection)
- Changing the data while at the supplier, before the data is obtained from the supplier
- Changing the data while at the supplier, where a model is trained and then that model is obtained from the supplier
- Manipulaing data entry, for example by creating fake accounts to enter positieve reviews for products, making these products get recommended more often

Example 1: an attacker breaks into a training set database to add images of houses and labels them as 'fighter plane', to mislead the camera system of an autonomous missile. The missile is then manipulated to attack houses. With a good test set this unwanted behaviour may be detected. However, the attacker can make the poisoned data represent input that normally doesn't occur and therefore would not be in a testset. The attacker can then create that abnormal input in practice. In the previous exmaple this could be houses with white crosses on the door.  See [MITRE ATLAS - Poison traing data](https://atlas.mitre.org/techniques/AML.T0020)
Example 2: a malicious supplier poisons data that is later obtained by another party to train a model. See [MITRE ATLAS - Publish poisoned datasets](https://atlas.mitre.org/techniques/AML.T0019)
Example 3: false information in documents on the internet causes a Large Language Model (GenAI) to output false results. That false information can be planted by an attacker, but of course also by accident. The latter case is a real GenAI risk, but technically comes down to the issue of having false data in a training set which falls outside of the security scope. ([OWASP for LLM 03](https://llmtop10.com/llm03/))

**Controls for data poisoning:**

- See [General controls](/goto/generalcontrols/), especially [Limiting the effect of unwanted behaviour](/goto/limitunwanted/)
- See [controls for development-time protection](/goto/developmenttimeintro/)
- See controls for broad model poisoning
- The below control(s), each marked with a # and a short name in capitals

#### #MORETRAINDATA
(development-time data science): More train data: increasing the amount of non-malicious data makes training more robust against poisoned examples - provided that these poisoned examples are small in number. One way to do this is through data augmentation - the creation of artificial training set samples that are small variations of existing samples.

Links to standards:

- Not covered yet in ISO/IEC standards

#### #DATAQUALITYCONTROL
(development-time data science). Data quality control: Perform quality control on data including detecting poisoned samples through statistical deviation or pattern recognition. For important data and scenarios this may involve human verification.

Particularity for AI and security: standard quality control needs to take into account that data may have maliciously been changed. This means that extra checks can be placed to detect changes that would normally not happen by themselves. For example: safely storing hash codes of data elements, such as images, and regularly checking to see if the images have been manipulated.

A method to detect statistical deviation is to train models on random selections of the training dataset and then feed each training sample to those models and compare results.

Links to standards:

- ISO/IEC 5259 series on Data quality for analytics and ML. Gap: covers this control minimally. in light of the particularity - the standard does not mention approaches to detect malicious changes (including detecting statistical deviations). Nevertheless, standard data quality control helps to detect malicious changes that violate data quality rules.
- ISO/iEC 42001 B.7.4 briefly covers data quality for AI. Gap: idem as ISO 5259
- Not further covered yet in ISO/IEC standards

#### #TRAINDATADISTORTION
Description: Train data distortion: making poisoned samples ineffective by smoothing or adding noise to training data (with the best practice of keeping the original training data, in order to expertiment with the filtering)
> Category: development-time data science  
> Permalink: owaspai.org/goto/traindatadistortion/

Effectiveness: 
- The level of effectiveness needs to be tested by experimenting, which will not give conclusive results, as an attacker my find more clever ways to poison the data than the methods used during testing.
- This control has no effect against attackers that have direct access to the training data after it has been distorted. For example, if the distorted training data is stored in a file or database to which the attacker has access, then the poisoned samples can still be injected. In other words: if there is zero trust in protection of the engineering environment, then train data distortion is only effective against data poisoning that took place outside the engineering environment (collected during runtime or obtained through the supply chain). This problem can be reduced by creating a trusted environment in which the model is trained, separated from the rest of the engineering environment. By doing so, controls such as train data distortion can be applied in that trusted environment and thus protect against data poisoning that may have taken place in the rest of the engineering environment.

See also EVASTIONROBUSTMODEL on adding noise against evasion attacks and [OBFUSCATETRAININGDATA](/goto/obfuscatetrainingdata/) to minimize sensitive data through randomisation.

Examples:

- [Transferability blocking](https://arxiv.org/pdf/1703.04318.pdf). The true defense mechanism against closed box attacks is to obstruct the transferability of the adversarial samples. The transferability enables the usage of adversarial samples in different models trained on different datasets. Null labeling is a procedure that blocks transferability, by introducing null labels into the training dataset, and trains the model to discard the adversarial samples as null labeled data.
- DEFENSE-GAN
- Local intrinsic dimensionality
- (weight)Bagging - see Annex C in ENISA 2021
- TRIM algorithm - see Annex C in ENISA 2021
- STRIP technique (after model evaluation) - see Annex C in ENISA 2021

Link to standards:

- Not covered yet in ISO/IEC standards

#### #POISONROBUSTMODEL
Description: Poison robust model: select a model type and creation approach to reduce sensitivity to poisoned training data.
> Category: development-time data science  
> Permalink: owaspai.org/goto/poisonrobustmodel/

The general principle of reducing sensitivity to poisoned training data is to make sure that the model does not memorize the specific malicious input pattern (or _backdoor trigger_). The following two examples represent different strategies, which can also complement each other in an approach called **fine pruning** (See [paper on fine-pruning](https://arxiv.org/pdf/1805.12185.pdf)):
1. Reduce memorization by removing elements of memory using **pruning**. Pruning in essence reduces the size of the model so it does not have the capacity to trigger on backdoor-examples while retaining sufficient accuracy for the intended use case. The approach removes neurons in a neural network that have been identified as non-essential for sufficient accuracy.
2. Overwrite memorized malicious patterns using **fine tuning** by retraining a model on a clean dataset(without poisoning).

Links to standards:
- Not covered yet in ISO/IEC standards

### 3.1.2. Development-time model poisoning

This threat refers to manipulating behaviour of the model by not poisoning the training data, but insead alter the engineering elements that lead to the model or represent the model (i.e. model parameters) during development time, e.g. by attacking the engineering environment to manipulate storage. When the model is trained by a supplier in a manipulative way and supplied as-is, then it is a [Transfer learning attack](goto/transferlearningattack/).
Data manipulation is referred to as data poisoning and is covered in separate threats.

**Controls:**

- See [General controls](/goto/generalcontrols/), especially [Limiting the effect of unwanted behaviour](/goto/limitunwanted/)
- See [controls for development-time protection](/goto/developmenttimeintro/)
- See controls for broad model poisoning

### 3.1.3 Transfer learning attack
Description: An attacker supplies a manipulated pre-trained model which is then obtained and unknowingly further used and/or trained/fine tuned, with still having the unwanted behaviour.
>Category: development-time threat  
>Permalink: https://owaspai.org/goto/transferlearningattack/

AI models are sometimes obtained elsewhere (e.g. open source) and then further trained or fine-tuned. These models may have been manipulated(poisoned) at the source, or in transit. See [OWASP for LLM 05: Supply Chain Vulnerabilities.](https://llmtop10.com/llm05/).

The type of manipulation can be through data poisoning, or by specifically changing the model parameters. Therefore, the same controls apply that help against those attacks. Since changing the model parameters requires protection of the parameters at the moment they are manipulated, this is not in the hands of the one who obtained the model. What remains are the controls against data poisoning, the controls against model poisoning in general (e.g. model ensembles), plus of course good supply chain management, and some specific controls that help against transfer learning attacks.

**Controls specific for transfer learning:**

- - See [General controls](/goto/generalcontrols/), especially [Limiting the effect of unwanted behaviour](/goto/limitunwanted/)
- See [controls for development-time protection](/goto/developmenttimeintro/), especially #[SUPPLYCHAINMANAGE](/goto/supplychainmanage/) to manage the source of the obtained model
- See controls for broad model poisoning
- Choose a model type resilient against a transfer learning attack

---

## 3.2. Sensitive data leak development-time

### 3.2.1. Development-time data leak

Impact: Confidentiality breach of sensitive train/test data.

Training data or test data can be confidential because it's sensitive data (e.g. personal data) or intellectual property. An attack or an unintended failure can lead to this training data leaking.  
Leaking can happen from the development environment, as engineers need to work with real data to train the model.  
Sometimes training data is collected at runtime, so a live system can become attack surface for this attack.  
GenAI models are often hosted in the cloud, sometimes managed by an external party. Therefore, if you train or fine tune these models, the training data (e.g. company documents) needs to travel to that cloud.

**Controls:**

- - See [General controls](/goto/generalcontrols/), especially [Sensitive data limitation](/goto/dataminimize/)
- See [controls for development-time protection](/goto/developmenttimeintro/)

### 3.2.2. Model theft through development-time model parameter leak

Impact: Confidentiality breach of model intellectual property.

**Controls:**

- See [General controls](/goto/generalcontrols/), especially [Sensitive data limitation](/goto/dataminimize/)
- See [controls for development-time protection](/goto/developmenttimeintro/)

### 3.2.3. Source code/configuration leak

Impact: Confidentiality breach of model intellectual property.

**Controls:**

- See [General controls](/goto/generalcontrols/), especially [Sensitive data limitation](/goto/dataminimize/)
- See [controls for development-time protection](/goto/developmenttimeintro/)
