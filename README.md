# NGQA: Next-Gen Software Quality Accelerator using AI Agents and LLM Reasoning

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

## 🚀 Overview

NGQA is a comprehensive framework that automates software quality assurance through the intelligent orchestration of static analysis tools and Large Language Models (LLMs). This repository contains the complete implementation of the NGQA pipeline as described in our research paper.

### Key Features

- **Fully Automated QA Pipeline**: Six-stage automated quality assurance with minimal human intervention
- **Multi-Language Support**: Works with JavaScript, TypeScript, Python, Java, C, C++, and C#
- **AI-Powered Code Revision**: LLM-based intelligent code remediation using RAG techniques
- **Advanced Test Generation**: Local Chain-of-Thought (LCoT) framework for comprehensive test suite creation
- **False Positive Mitigation**: AI agent with 89.0% F1-score for filtering incorrect static analysis results
- **Comprehensive Evaluation**: Multiple metrics including PassRatio, CodeBLEU, and CodeScore

## 📊 Performance Results

Our experimental evaluation across 70 repositories demonstrates significant improvements:

| Metric | Original | Revised | Improvement |
|--------|----------|---------|-------------|
| PassRatio | 73.8% | 86.0% | **+16.5%** |
| CodeBLEU | 61.2% | 78.8% | **+28.8%** |
| CodeScore | 67.0% | 83.1% | **+24.0%** |

- **Issue Resolution Rate**: 83.5% of validated issues successfully resolved
- **Test Case Generation**: Average of 260 test cases per repository
- **False Positive Mitigation**: 89.0% F1-score with 598 false positives filtered

## 🏗️ Architecture

The NGQA pipeline consists of six sequential stages:

```
1. Static Issue Detection → 2. False Positive Mitigation → 3. LLM-Based Revision
                                        ↓
6. Evaluation & Validation ← 5. Test Suite Generation ← 4. Structural Analysis
```

### Pipeline Components

1. **Static Issue Detection**: Uses SonarQube for automated code quality issue identification
2. **False Positive Mitigation**: RAG-enhanced AI agent filters incorrect detections
3. **LLM-Based Revision**: Intelligent code remediation using external knowledge sources
4. **Structural Dependency Analysis**: Comprehensive project mapping for context-aware testing
5. **Test Suite Generation**: LCoT framework with 4-agent sequential reasoning
6. **Evaluation & Validation**: Multi-metric assessment using PassRatio, CodeBLEU, and CodeScore

## 🛠️ Installation

### Prerequisites

- Python 3.8+
- Node.js (for JavaScript/TypeScript support)
- Java SDK (for Java support)
- SonarQube Community Edition
- n8n workflow automation tool
- Ollama (for local LLM deployment)

### Setup Instructions

1. **Clone the repository**:

2. **Install Python dependencies**:
```bash
pip install -r requirements.txt
```

3. **Set up SonarQube**:
```bash
# Download and start SonarQube server
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
unzip sonarqube-9.9.0.65466.zip
cd sonarqube-9.9.0.65466/bin/linux-x86-64
./sonar.sh start
```

4. **Install n8n**:
```bash
npm install -g n8n
```

5. **Set up Ollama and download models**:
```bash
# Install Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Download required models
ollama pull deepseek-code-v2:16b
ollama pull codellama:13b
ollama pull qwen2.5-code:14b
```

## 🚀 Usage

### Quick Start

1. **Load the n8n Workflow**:
```bash
# Start n8n
n8n start

# Import the workflow in n8n web interface (http://localhost:5678)
# Go to: Workflows → Import from File → Select "Next_Gen_Software_Quality_Assurance.json"
```

2. **Extract SonarQube Issues**:
```bash
# Place your source code in the input directory
mkdir input_projects
cp -r /path/to/your/project input_projects/

# Run SonarQube analysis and extract issues
python Extract_All_Issues.py --project-path input_projects/your_project
# This generates: issues_report.csv
```

3. **Trigger the Complete Pipeline**:
```bash
# In n8n workflow interface:
# 1. Click "Execute Workflow" 
# 2. Provide the CSV file location: /path/to/issues_report.csv
# 3. The workflow automatically handles Steps 2-6
```

**That's it!** The n8n workflow will automatically process your CSV file through all remaining pipeline stages.

### n8n Workflow - Complete Automation

The `Next_Gen_Software_Quality_Assurance.json` workflow contains all the automation for Steps 2-6:

**🎯 Trigger Input**: CSV file location containing SonarQube issues  
**🔄 Automated Processing**: All pipeline stages execute sequentially  
**📊 Output**: Revised code, test suites, and evaluation reports  

### Workflow Configuration

1. **Import the workflow**:
   - Open n8n web interface (http://localhost:5678)
   - Navigate to **Workflows** → **Import from File**
   - Select `Next_Gen_Software_Quality_Assurance.json`
   - All nodes will be automatically configured

2. **Configure your credentials**:
   - Set up your LLM API keys (OpenAI, Anthropic, etc.)
   - Configure Ollama endpoints for local models
   - Set SonarQube connection details

3. **Execute the workflow**:
   - Click "Execute Workflow"
   - Enter the path to your `issues_report.csv` file
   - The workflow handles everything else automatically

### Pipeline Execution

#### Step 1: Static Issue Detection (Manual)
```bash
# Configure SonarQube project
sonar-scanner \
  -Dsonar.projectKey=your_project \
  -Dsonar.sources=./input_projects/your_project \
  -Dsonar.host.url=http://localhost:9000

# Extract issues to CSV
python Extract_All_Issues.py --project your_project --output issues_report.csv
```

#### Steps 2-6: Fully Automated via n8n Workflow
Simply trigger the workflow with your CSV file location. The workflow automatically executes:

**Step 2: False Positive Mitigation**
- RAG-enhanced AI agent processes the CSV
- Filters out incorrect static analysis results
- Validates remaining issues for revision

**Step 3: LLM-Based Code Revision**
- Processes each validated issue
- Generates revised code using external knowledge
- Maintains code structure and functionality

**Step 4: Structural Analysis**
- Maps project dependencies
- Analyzes code relationships
- Prepares context for test generation

**Step 5: Test Suite Generation**
- Four LCoT agents work sequentially
- Generates comprehensive test suites
- Creates tests for both original and revised code

**Step 6: Evaluation**
- Calculates PassRatio, CodeBLEU, and CodeScore
- Performs four-fold testing strategy
- Generates detailed performance reports

### Workflow Trigger Example

```bash
# After running Extract_All_Issues.py, you get:
# /home/user/ngqa/issues_report.csv

# In n8n workflow interface:
# 1. Click "Execute Workflow"
# 2. Input: /home/user/ngqa/issues_report.csv
# 3. Wait for completion (processing time depends on project size)
# 4. Check outputs in configured directories
```

![Untitled](https://github.com/user-attachments/assets/6b343989-c86e-4e41-b1ac-6459c332e268)

![Screenshot_19-7-2025_19384_localhost](https://github.com/user-attachments/assets/99258bce-603b-4236-abb5-37c998aae23e)

## 📁 Project Structure

```
NGQA/
├── 📄 Codetree.py                    # Main pipeline orchestrator
├── 📄 Create_sub_folders.py          # Test suite generation and organization
├── 📄 Extract_All_Issues.py          # SonarQube issue extraction
├── 📄 README.md                      # This file
├── 📄 Next_Gen_Software_Quality_Assurance.json  # n8n workflow configuration
```

## 🔧 Configuration

### Model Configuration

```json
{
  "false_positive_agent": {
    "model": "gpt-4",
    "temperature": 0.1,
    "max_tokens": 1000
  },
  "revision_agent": {
    "model": "claude-3-sonnet",
    "temperature": 0.2,
    "max_tokens": 2000
  },
  "lcot_agents": {
    "agent_1": {
      "model": "deepseek-code-v2:16b",
      "local": true
    },
    "agent_2": {
      "model": "deepseek-code-v2:16b",
      "local": true
    },
    "agent_3": {
      "model": "codellama:13b",
      "local": true
    },
    "agent_4": {
      "model": "qwen2.5-code:14b",
      "local": true
    }
  }
}
```

### Pipeline Settings

```json
{
  "supported_languages": ["javascript", "typescript", "python", "java", "c", "cpp", "csharp"],
  "issue_types": ["BUGS", "CODE_SMELLS", "VULNERABILITIES"],
  "evaluation_metrics": ["PassRatio", "CodeBLEU", "CodeScore"],
  "test_generation": {
    "max_tests_per_function": 10,
    "include_edge_cases": true,
    "include_error_cases": true
  }
}
```

## 📊 Evaluation and Metrics

### Four-Fold Testing Strategy

The pipeline generates two test suites (Original Tests - OT, Revised Tests - RT) and evaluates them against both code versions:

1. **OC-OT**: Original Code with Original Tests (baseline)
2. **OC-RT**: Original Code with Revised Tests (test quality assessment)
3. **RC-OT**: Revised Code with Original Tests (revision effectiveness)
4. **RC-RT**: Revised Code with Revised Tests (optimal performance)

### Metrics Calculation

Run evaluation scripts to calculate comprehensive metrics:

```bash
# Calculate all metrics for a project
python evaluation/calculate_metrics.py \
  --original-code input_projects/your_project \
  --revised-code output_revised/your_project \
  --original-tests tests_generated/original \
  --revised-tests tests_generated/revised

# Generate detailed report
python evaluation/generate_report.py --project your_project
```

## 🌍 Multi-Language Support

### Supported Languages and Extensions

| Language | Extensions | Static Analysis Tool |
|----------|------------|---------------------|
| JavaScript | `.js`, `.jsx`, `.mjs` | SonarJS |
| TypeScript | `.ts`, `.tsx` | SonarTS |
| Python | `.py` | SonarPython |
| Java | `.java` | SonarJava |
| C | `.c` | SonarC |
| C++ | `.cpp`, `.cxx`, `.cc` | SonarCpp |
| C# | `.cs` | SonarC# |

### Language-Specific Performance

Based on our evaluation across 70 repositories:

- **Python**: Best performance (90.3% PassRatio)
- **TypeScript**: Strong structural analysis (88.2% PassRatio)
- **JavaScript**: Consistent improvements (87.9% PassRatio)
- **C#**: Good overall performance (86.4% PassRatio)
- **Java**: Complex syntax handling (84.1% PassRatio)
- **C**: Efficient processing (82.9% PassRatio)
- **C++**: Challenging but effective (82.5% PassRatio)

## 🔍 Advanced Features

### Retrieval-Augmented Generation (RAG)

The pipeline uses RAG to enhance AI agents with external knowledge:

```python
# Example RAG configuration
rag_sources = {
    "stackoverflow": {
        "enabled": True,
        "max_results": 5,
        "relevance_threshold": 0.8
    },
    "github": {
        "enabled": True,
        "repositories": ["popular_repos_by_language"],
        "max_results": 3
    },
    "sonarqube_community": {
        "enabled": True,
        "focus": "rules_and_best_practices"
    }
}
```

### Chain-of-Thought (CoT) Reasoning

The LCoT framework implements structured reasoning:

```python
# LCoT agent sequence
def local_chain_of_thought(source_code, project_structure):
    # Agent 1: Functional Analysis
    analysis = agent1.analyze_functionality(source_code)
    
    # Agent 2: Component Extraction
    components = agent2.extract_components(source_code, analysis)
    
    # Agent 3: Test Scenario Generation
    scenarios = agent3.generate_scenarios(components, project_structure)
    
    # Agent 4: Unit Test Creation
    tests = agent4.create_unit_tests(scenarios, source_code)
    
    return tests
```

## 📈 Performance Benchmarks

### Scalability Analysis

| Repository Size | Processing Time | Memory Usage | Success Rate |
|-----------------|-----------------|--------------|--------------|
| Small (< 1K LOC) | 2-5 minutes | 512MB | 95% |
| Medium (1K-10K LOC) | 10-30 minutes | 1GB | 89% |
| Large (10K-50K LOC) | 30-120 minutes | 2GB | 83% |
| Enterprise (50K+ LOC) | 2-8 hours | 4GB | 78% |

### Hardware Requirements

**Minimum Requirements:**
- RAM: 8GB
- CPU: 4 cores
- Storage: 50GB free space
- GPU: Optional (for faster local LLM inference)

**Recommended Requirements:**
- RAM: 16GB+
- CPU: 8+ cores
- Storage: 100GB+ SSD
- GPU: NVIDIA RTX 3060+ or equivalent

## 🔧 Troubleshooting

### Common Issues

1. **SonarQube Connection Issues**:
```bash
# Check SonarQube status
curl -u admin:admin http://localhost:9000/api/system/status

# Restart SonarQube
cd sonarqube-9.9.0.65466/bin/linux-x86-64
./sonar.sh restart
```

2. **Ollama Model Issues**:
```bash
# List available models
ollama list

# Re-download corrupted model
ollama pull deepseek-code-v2:16b --force
```

3. **Memory Issues with Large Projects**:
```bash
# Process in chunks
python Codetree.py --input-csv issues.csv --batch-size 50 --parallel-workers 2
```
## ⚡ Accelerated QA Effort Analysis

This section quantifies NGQA’s impact on software quality assurance (QA) efficiency by comparing automated pipeline execution time against a representative manual QA baseline. The analysis combines:

* **Empirical runtime measurements** of the NGQA pipeline
* **COCOMO II–based effort estimation** for manual QA

---

### ⏱️ NGQA Pipeline Execution Time

Pipeline performance was measured across all stages using the experimental setup:

* **GPU**: NVIDIA RTX 3060
* **RAM**: 16 GB
* **CPU**: AMD Ryzen 5 5600G

#### Fixed Per-Repository Overhead

| Stage                       | Avg Time       |
| --------------------------- | -------------- |
| Static Analysis (SonarQube) | 7.5 min        |
| Dependency Analysis         | 4.0 min        |
| Performance Profiling       | 12.5 min       |
| **Total per repository**    | **24 minutes** |

* Across **10 repositories per language group** → **~4.0 hours fixed overhead**

#### Per-Issue Processing Time

| Component                       | Avg Time                   |
| ------------------------------- | -------------------------- |
| False Positive Mitigation (RAG) | ≤ 25 sec                   |
| LLM Revision (Round 1)          | 12.5 sec                   |
| LLM Revision (Round 2)          | 12.5 sec                   |
| **Total per issue**             | **~50 sec (0.0139 hours)** |

#### Total Runtime

* **Repositories analyzed**: 70
* **Validated issues**: 4,168
* **Total execution time**: **95.9 hours (~4 days continuous runtime)**
* **Infrastructure**: Single mid-range GPU workstation

This demonstrates that NGQA scales effectively **without requiring high-performance or distributed systems**.

---

### 📐 Manual QA Baseline (COCOMO II Estimation)

To establish a realistic manual QA comparison, the **COCOMO II Post-Architecture model** was applied to:

* **750,000 SLOC** (effective dataset)
* **1,510,600 LOC total corpus**
* **Effort Adjustment Factor (EAF)**: 0.70

#### Total Development Effort

| Phase        | Effort (PM)    | Duration    |
| ------------ | -------------- | ----------- |
| Inception    | 236.2          | 6.8 months  |
| Elaboration  | 944.8          | 20.5 months |
| Construction | 2,991.8        | 34.2 months |
| Transition   | 472.4          | 6.8 months  |
| **Total**    | **4,645.2 PM** | —           |

#### QA-Specific Effort (Assessment Activity)

| Phase               | QA Effort (PM)                |
| ------------------- | ----------------------------- |
| Inception           | 18.9                          |
| Elaboration         | 94.5                          |
| Construction        | 718.0                         |
| Transition          | 113.4                         |
| **Total QA Effort** | **944.8 PM (~151,168 hours)** |

---

### 🛠️ Targeted Defect Remediation Estimate

A conservative estimate of **0.75 hours per issue** was used to reflect:

* Issue review
* Root cause analysis
* Fix implementation
* Test creation/update
* Validation

**Total manual QA effort for 4,168 issues:**

> **3,126 person-hours**

---

### 🚀 Acceleration Results

| Metric                  | Value            |
| ----------------------- | ---------------- |
| NGQA Total Time         | 95.9 hours       |
| Manual QA Time          | 3,126 hours      |
| **Acceleration Factor** | **32.6× faster** |

---

### 📊 Per-Language Acceleration

| Language         | LOC       | Issues | Manual QA (hrs) | NGQA (hrs) | Speedup   |
| ---------------- | --------- | ------ | --------------- | ---------- | --------- |
| TypeScript       | 195,800   | 483    | 362.3           | 10.7       | **33.9×** |
| JavaScript       | 154,200   | 395    | 296.3           | 9.5        | **31.2×** |
| Python           | 251,500   | 706    | 529.5           | 13.8       | **38.4×** |
| Java             | 318,900   | 842    | 631.5           | 15.7       | **40.2×** |
| C++              | 187,600   | 618    | 463.5           | 12.6       | **36.8×** |
| C#               | 263,400   | 707    | 530.3           | 13.8       | **38.4×** |
| C                | 139,200   | 417    | 312.8           | 9.8        | **31.9×** |
| **Total / Mean** | 1,510,600 | 4,168  | 3,126.2         | 95.9       | **32.6×** |

---

### 📈 Key Takeaways

* NGQA delivers **>30× acceleration** across all evaluated languages
* Performance gains are consistent despite differences in:

  * Code complexity
  * Issue density
  * Language semantics
* The system achieves **enterprise-scale QA automation** on commodity hardware
* Significant reduction in **manual developer effort and QA cycle time**

---

### 🖼️ Visual Results

<img width="1079" height="472" alt="image" src="https://github.com/user-attachments/assets/9b2033ab-673f-4b24-a092-65c8418ed651" />

<img width="1072" height="752" alt="image" src="https://github.com/user-attachments/assets/2fab9258-0449-4794-943a-adf0fdd53544" />

---

### Key Contributions

1. **Fully Automated QA Pipeline**: First comprehensive framework integrating static analysis with AI-driven code revision
2. **Local Chain-of-Thought (LCoT) Framework**: Novel approach for enhanced test case generation using sequential AI agents
3. **Multi-Language Evaluation**: Comprehensive assessment across 7 programming languages and 70 repositories
4. **Dual Test Generation Strategy**: Innovative approach generating tests for both original and revised code versions

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **SonarSource** for SonarQube static analysis tools
- **n8n** for workflow automation capabilities
- **Ollama** for local LLM deployment
- **OpenAI** and **Anthropic** for cloud-based LLM services
- **Open-source community** for the repositories used in our evaluation

