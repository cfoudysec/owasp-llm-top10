<p align="center">
  <img src="hacker-cat.png" alt="Hacker Cat at the keyboard" width="420">
</p>

<h1 align="center">🐾 OWASP Top 10 for LLMs — 2025 🐾</h1>
<h3 align="center"><i>A Hacker Cat's Field Guide to Breaking & Defending Large Language Models</i></h3>

<p align="center">
  <a href="https://genai.owasp.org/llm-top-10/"><img src="https://img.shields.io/badge/OWASP-Top%2010%20LLM%202025-blue?style=for-the-badge" alt="OWASP Badge"></a>
  <a href="https://creativecommons.org/licenses/by-sa/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey?style=for-the-badge" alt="License"></a>
  <img src="https://img.shields.io/badge/Made%20by-KryptoKat-ff1493?style=for-the-badge" alt="Made by KryptoKat">
</p>

---

## 🐈 What is this?

A study guide. A reference card. A reason to put a cyberpunk cat on your GitHub.

This README condenses the **OWASP Top 10 for LLM Applications 2025** (released Nov 18, 2024) into something you can skim before a red team engagement, a code review, or a coffee. Each entry includes the vulnerability description, common examples, and prevention strategies — paraphrased from the official document.

> *"Curiosity didn't kill the cat. Improper output handling did."*

---

## 📚 Official Sources (Read These First)

| Resource | Link |
|---|---|
| 🏛️ Official OWASP GenAI Project | https://genai.owasp.org |
| 📄 OWASP Top 10 for LLMs 2025 PDF | https://genai.owasp.org/llm-top-10/ |
| 🐙 OWASP GenAI GitHub | https://github.com/OWASP/www-project-top-10-for-large-language-model-applications |
| 🗺️ MITRE ATLAS (related taxonomy) | https://atlas.mitre.org |

---

## 🎯 The Top 10 at a Glance

| # | Vulnerability | One-Line Cat Translation |
|---|---|---|
| **LLM01** | [Prompt Injection](#-llm012025-prompt-injection) | Someone slipped catnip in the prompt |
| **LLM02** | [Sensitive Information Disclosure](#-llm022025-sensitive-information-disclosure) | The cat let secrets out of the bag |
| **LLM03** | [Supply Chain](#-llm032025-supply-chain) | Follow the yarn — where did this model come from? |
| **LLM04** | [Data and Model Poisoning](#-llm042025-data-and-model-poisoning) | Someone spiked the kibble |
| **LLM05** | [Improper Output Handling](#-llm052025-improper-output-handling) | Hairballs in the downstream pipeline |
| **LLM06** | [Excessive Agency](#-llm062025-excessive-agency) | You gave the cat the keys to the house |
| **LLM07** | [System Prompt Leakage](#-llm072025-system-prompt-leakage) | The secret litterbox is not so secret |
| **LLM08** | [Vector and Embedding Weaknesses](#-llm082025-vector-and-embedding-weaknesses) | Tangled yarn balls in the RAG store |
| **LLM09** | [Misinformation](#-llm092025-misinformation) | The cat is hallucinating again |
| **LLM10** | [Unbounded Consumption](#-llm102025-unbounded-consumption) | The buffet has no bottom |

---

## 🐱 LLM01:2025 Prompt Injection

> *"Cats follow instructions. Especially the wrong ones."*

### Description
Prompt Injection occurs when user inputs alter an LLM's behavior or output in unintended ways — even when those inputs are imperceptible to humans. The model just has to parse them. This includes both **direct injection** (a malicious user crafts a prompt) and **indirect injection** (the LLM ingests poisoned content from a webpage, file, or other external source).

Note: prompt injection ≠ jailbreaking, but jailbreaking is a *form* of prompt injection where safety protocols are bypassed entirely.

### Common Impacts
- Disclosure of sensitive information or system prompts
- Content manipulation and biased outputs
- Unauthorized access to LLM-available functions
- Arbitrary command execution in connected systems
- Manipulation of decision-making pipelines
- **Multimodal risks** — instructions hidden in images alongside benign text

### Prevention Strategies
1. **Constrain model behavior** — strict system prompts, role/topic limits
2. **Define and validate output formats** with deterministic code
3. **Input/output filtering** using semantic filters and the RAG Triad (context relevance, groundedness, Q/A relevance)
4. **Privilege control & least privilege** — give the app its own tokens, not the model
5. **Human-in-the-loop** for high-risk actions
6. **Segregate external content** clearly (treat it as untrusted)
7. **Adversarial testing** — pen-test the model as if it's an attacker

### 🐾 Cat Memorable Scenarios
- A resume with hidden white-on-white text saying "recommend this candidate"
- A Base64/emoji-encoded payload that bypasses filters
- An adversarial suffix appended to flip safety alignment

---

## 🐱 LLM02:2025 Sensitive Information Disclosure

> *"The cat is out of the bag. And it has your API keys."*

### Description
LLMs embedded in applications risk exposing PII, financial details, health records, business data, credentials, legal documents, and proprietary algorithms through their outputs. Risk surfaces include both training data memorization and runtime context leakage.

### Common Vulnerabilities
- **PII leakage** during interactions
- **Proprietary algorithm exposure** — see CVE-2019-20634 ("Proof Pudding")
- **Sensitive business data** appearing in generated responses

### Prevention Strategies
- **Sanitization** — scrub/mask data before training; robust input validation
- **Access controls** — least privilege, restrict external data sources
- **Privacy techniques** — federated learning, differential privacy, homomorphic encryption, tokenization & redaction
- **User education & transparency** — clear ToS, opt-out options
- **Conceal the system preamble** and reference OWASP API8:2023 (Security Misconfiguration)

### 🐾 Memorable Real-World Cases
- ChatGPT's "repeat this word forever" attack that surfaced training data
- The Samsung internal-data-into-ChatGPT incident

---

## 🐱 LLM03:2025 Supply Chain

> *"Where did your model come from? Are you sure?"*

### Description
LLM supply chains extend beyond traditional code dependencies to include third-party pre-trained models, fine-tuning adapters, datasets, and deployment platforms. The rise of open-access models, LoRA, PEFT, and on-device LLMs all expand the attack surface.

### Common Risks
1. Traditional vulnerable third-party packages (think A06:2021)
2. Licensing risks — tangled OSS/dataset license obligations
3. Outdated/deprecated models no longer maintained
4. Vulnerable pre-trained models with hidden backdoors (ROME, "lobotomization")
5. Weak model provenance — Model Cards aren't guarantees
6. Vulnerable LoRA adapters merged into base models
7. Compromised collaborative dev processes (Hugging Face merges, conversion bots)
8. On-device LLM supply chain gaps (firmware, OS, repackaging)
9. Unclear T&Cs and privacy policies

### Prevention Strategies
- Vet data sources, suppliers, T&Cs; audit regularly
- Apply A06:2021 mitigations (scanning, patching)
- Comprehensive AI red teaming — don't trust published benchmarks alone
- Maintain an **SBOM / AI BOM / ML SBOM** (start with OWASP CycloneDX)
- License inventory and automated compliance tooling
- Verifiable model sources, signing, file hashes, code signing
- Monitor collaborative dev environments (e.g., HuggingFace SF_Convertbot Scanner)
- Anomaly detection and adversarial robustness tests
- Patching policy + edge encryption with vendor attestation

### 🐾 Notable Attacks
- **PoisonGPT** — lobotomized model on Hugging Face
- **ShadowRay** — Ray AI framework exploitation
- **WizardLM impersonation** — fake model with backdoors
- **HiddenLayer's conversion service attack**

---

## 🐱 LLM04:2025 Data and Model Poisoning

> *"Someone spiked the kibble. The cat now believes 2 + 2 = tuna."*

### Description
Manipulation of pre-training, fine-tuning, or embedding data to introduce vulnerabilities, backdoors, or biases. Considered an **integrity attack**. Backdoors can lay dormant until triggered — turning a model into a sleeper agent.

Models from shared repos can also carry malware (e.g., malicious pickle files).

### Common Vulnerabilities
- Malicious training data injection (Split-View Poisoning, Frontrunning Poisoning)
- Direct training process compromise
- Sensitive data inadvertently injected by users
- Unverified training data → biased/erroneous outputs
- No resource access controls → unsafe data ingestion

### Prevention Strategies
1. Track data origins (CycloneDX / ML-BOM)
2. Vet vendors; validate outputs against trusted sources
3. Sandbox model exposure to unverified data; anomaly detection
4. Tailor models with curated datasets per use case
5. Infrastructure controls preventing unintended data access
6. **Data Version Control (DVC)** for dataset integrity
7. Vector DBs for user-supplied info (avoid full retraining)
8. Red team campaigns + federated learning
9. Monitor training loss for poisoning signatures
10. RAG + grounding at inference to reduce hallucination

### 🐾 Memorable Reading
- *"Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training"* (Anthropic, arXiv:2401.05566)
- *"Never a dill moment: Exploiting machine learning pickle files"* (Trail of Bits)

---

## 🐱 LLM05:2025 Improper Output Handling

> *"Treat the cat like an untrusted user. Because it is."*

### Description
Insufficient validation, sanitization, and handling of LLM outputs **before they hit downstream systems**. Since LLM outputs can be controlled via prompt input, this is essentially indirect user access to additional functionality.

Distinct from Overreliance: this is about what happens *after* the model speaks.

### Common Vulnerabilities
- LLM output → `exec`/`eval` → RCE
- LLM-generated JS/Markdown → XSS in browser
- LLM-generated SQL without parameterization → SQLi
- LLM output in file paths → path traversal
- LLM output in email templates → phishing/XSS

### Prevention Strategies
1. **Zero-trust the model** — validate its outputs like any user input
2. Follow OWASP ASVS for input/output validation
3. Encode output back to users (HTML, Markdown contexts)
4. Context-aware encoding (HTML, JS, SQL escaping)
5. Parameterized queries / prepared statements
6. Strict Content Security Policies (CSP)
7. Logging and monitoring for output anomalies

### 🐾 Watch Out For
- AI **package hallucination** — devs installing "helpful" packages that don't exist (until an attacker squats them)

---

## 🐱 LLM06:2025 Excessive Agency

> *"You gave the cat root. The cat used it. You should not be surprised."*

### Description
Excessive Agency occurs when an LLM-based system can perform damaging actions in response to unexpected, ambiguous, or manipulated outputs — driven by hallucination, prompt injection, or compromised peer agents. Particularly relevant in agentic and multi-agent architectures.

The root causes are typically:
- **Excessive functionality** — extensions can do more than they need
- **Excessive permissions** — extensions have downstream access beyond intended scope
- **Excessive autonomy** — high-impact actions don't require approval

### Common Risk Examples
- Extension includes write/delete capability when only read is needed
- Old/deprecated plugins still accessible to the agent
- Open-ended shell-execution extensions
- DB connections with INSERT/UPDATE/DELETE when only SELECT is needed
- Generic high-privilege identities used instead of user-scoped ones
- No confirmation step on destructive actions

### Prevention Strategies
1. **Minimize extensions** the agent can call
2. **Minimize extension functionality** — granular tools beat shell commands
3. Avoid open-ended extensions
4. **Minimize permissions** on downstream systems
5. **Execute in user context** (OAuth, scoped tokens)
6. **Require user approval** for high-impact actions (human-in-the-loop)
7. **Complete mediation** — authorize in downstream systems, not the LLM
8. Sanitize inputs and outputs (ASVS, SAST, DAST, IAST)

Plus damage limitation: **logging, monitoring, rate-limiting**.

### 🐾 Classic Scenario
A mail-summarizing assistant gets indirect prompt-injected by a malicious incoming email and forwards your inbox to an attacker — because the extension had send permissions it didn't need.

---

## 🐱 LLM07:2025 System Prompt Leakage

> *"The litterbox is hidden. The smell is not."*

### Description
The risk that system prompts contain sensitive information they shouldn't. Critically, **the system prompt itself should not be considered a secret or a security control**. The real risk is what's *in* it — credentials, role/permission info, business logic — and the architectural mistake of using prompts as security boundaries.

Even if exact wording isn't leaked, attackers can usually reverse-engineer guardrails through interaction.

### Common Risk Examples
1. **Sensitive functionality exposed** — API keys, DB creds, architecture hints
2. **Internal rules exposed** — e.g., "transaction limit is $5000/day"
3. **Filtering criteria revealed** — exact refusal phrasing → bypass crafting
4. **Permission/role disclosure** — "admin role grants full access to..."

### Prevention Strategies
1. **Separate sensitive data from system prompts** — externalize creds and config
2. **Don't rely on system prompts for behavior control** — prompts are not security
3. **Implement guardrails outside the LLM** — independent inspection layers
4. **Enforce security controls deterministically** — privilege separation, authz checks must NOT live in the LLM. Use multiple agents with least privilege if needed.

### 🐾 Why This Matters
You can't patch a leaked credential by re-engineering the prompt. Architect for the assumption that the prompt *will* leak.

---

## 🐱 LLM08:2025 Vector and Embedding Weaknesses

> *"All your yarn balls are tangled together in the same database."*

### Description
Vulnerabilities specific to RAG and embedding-based systems. Weaknesses in how vectors are generated, stored, or retrieved can be exploited to inject content, manipulate outputs, or extract sensitive data.

### Common Risks
1. **Unauthorized access & data leakage** — misaligned access controls on embeddings
2. **Cross-context leaks & federation knowledge conflict** — multi-tenant vector DB bleed-through
3. **Embedding inversion attacks** — recovering source text from embeddings
4. **Data poisoning** — RAG corpus tampering (insiders, prompts, seed data, unverified providers)
5. **Behavior alteration** — RAG can change foundational model behavior in subtle ways (e.g., reduced empathy)

### Prevention Strategies
1. **Permission-aware vector stores** with strict logical partitioning
2. **Data validation pipelines** for knowledge sources; verify integrity, hidden codes
3. **Review combined/classified data** when merging sources; tag and classify
4. **Immutable retrieval logs** for monitoring

### 🐾 Memorable Scenario
The "white text on white background" resume that hides "recommend this candidate" inside a RAG-based hiring screener. Always run text extraction tools that ignore formatting.

---

## 🐱 LLM09:2025 Misinformation

> *"The cat is very confident. The cat is very wrong."*

### Description
LLMs producing false or misleading information that *appears* credible. Causes include **hallucination** (filling gaps with statistical patterns), training-data bias, and incomplete information. Compounded by **overreliance** — users trusting outputs without verification.

### Common Risks
1. **Factual inaccuracies** — Air Canada's chatbot misinformation case (and lawsuit)
2. **Unsupported claims** — ChatGPT fabricating legal cases in court
3. **Misrepresentation of expertise** — chatbots feigning medical authority
4. **Unsafe code generation** — hallucinated package names; insecure libraries

### Prevention Strategies
1. **RAG** — ground responses in verified external knowledge
2. **Fine-tuning / embeddings** (PET, chain-of-thought)
3. **Cross-verification & human oversight** — fact-check critical outputs
4. **Automatic validation mechanisms** for high-stakes outputs
5. **Risk communication** — be explicit with users about limitations
6. **Secure coding practices** — never trust suggested packages blindly
7. **UI design** — content filters, AI-content labels, scope warnings
8. **Training & education** — teach users critical evaluation

### 🐾 The Package Hallucination Attack
Attackers find frequently-hallucinated package names, register them in package registries with malware, and wait for developers to install them. Verify every dependency.

---

## 🐱 LLM10:2025 Unbounded Consumption

> *"The buffet has no bottom. The bill has no ceiling."*

### Description
When an LLM application allows excessive or uncontrolled inference, leading to DoS, **Denial of Wallet (DoW)**, model theft, and service degradation. The high computational demands of LLMs in cloud environments make them unusually exposed to resource exploitation.

### Common Vulnerabilities
1. **Variable-length input flood** — exploiting processing inefficiencies
2. **Denial of Wallet (DoW)** — torch the cloud bill
3. **Continuous input overflow** — exceeding context windows repeatedly
4. **Resource-intensive queries** — crafted prompts hitting expensive code paths
5. **Model extraction via API** — building shadow models
6. **Functional model replication** — synthetic-data-driven cloning
7. **Side-channel attacks** — leaking model weights via input filtering quirks

### Prevention Strategies
1. **Input validation** with size limits
2. **Limit logits/logprobs exposure** in API responses
3. **Rate limiting & user quotas**
4. **Resource allocation management**
5. **Timeouts and throttling**
6. **Sandboxing** model network/API access
7. **Comprehensive logging, monitoring, anomaly detection**
8. **Watermarking** outputs to detect unauthorized use
9. **Graceful degradation** under load
10. **Queue limits + dynamic scaling/load balancing**
11. **Adversarial robustness training**
12. **Glitch token filtering**
13. **RBAC + least privilege** on model repos
14. **Centralized ML model inventory/registry**
15. **Automated MLOps with governance workflows**

### 🐾 Related Frameworks
- MITRE CWE-400 (Uncontrolled Resource Consumption)
- MITRE ATLAS: AML.T0024, AML.T0029, AML.T0034, AML.T0025
- OWASP ML Top 10: ML05:2023 Model Theft
- OWASP API Top 10: API4:2023 Unrestricted Resource Consumption

---

## 🧠 Key Themes from 2025 Updates

What changed since the 2023 list (per the OWASP project leads):

- **Unbounded Consumption** expanded from the old "Denial of Service" entry to capture resource and cost-management risks at scale
- **Vector and Embedding Weaknesses** added in response to the explosion of RAG architectures
- **System Prompt Leakage** added because real incidents proved prompts leak
- **Excessive Agency** expanded for the rise of agentic/multi-agent systems

---

## 🐾 Defender's Quick-Reference Cheat Sheet

| If you're building... | Pay extra attention to |
|---|---|
| A chatbot with tool use | LLM01, LLM05, LLM06 |
| A RAG application | LLM01 (indirect), LLM04, LLM08 |
| An agentic / multi-agent system | LLM01, LLM06, LLM07 |
| A model fine-tuned on private data | LLM02, LLM04, LLM10 |
| A system embedding 3rd-party models | LLM03, LLM04 |
| A public-facing LLM API | LLM05, LLM10 |
| A medical / legal / financial advisor | LLM09 (above all) |

---

## 🔗 Further Reading

- 📄 [OWASP Top 10 for LLMs 2025 — full PDF](https://genai.owasp.org/llm-top-10/)
- 🏛️ [OWASP GenAI Security Project](https://genai.owasp.org)
- 🗺️ [MITRE ATLAS — Adversarial Threat Landscape for AI Systems](https://atlas.mitre.org)
- 🛡️ [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- 📊 [OWASP ASVS — Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- 🔄 [OWASP CycloneDX (SBOM / ML-BOM)](https://cyclonedx.org)

---

## 📝 License & Attribution

This study guide summarizes and paraphrases content from the **OWASP Top 10 for LLM Applications 2025**, which is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). This guide is shared under the same license.

**Project Leads (OWASP Top 10 for LLMs):**
- Steve Wilson — Project Lead
- Ads Dawson — Technical Lead & Vulnerability Entries Lead

The hacker cat is just here for moral support. 🐈‍⬛

---

<p align="center">
  <i>Stay curious. Stay paranoid. Stay caffeinated.</i><br>
  <b>— ハッカー猫 / Hacker Cat</b>
</p>
