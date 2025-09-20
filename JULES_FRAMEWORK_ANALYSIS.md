# HexStrike-AI Framework Analysis
This document provides a comprehensive analysis of the HexStrike-AI framework, detailing its operational mechanics, structure, and toolset. It is designed to be a complete blueprint for understanding and utilizing the framework.

---

## 1. Framework Mechanics

### 1.1. Primary Interaction Script
Interaction with the HexStrike-AI framework is primarily managed through two core components:
- **`hexstrike_server.py`**: The backend Flask server that exposes the toolset via a REST API. It handles the logic of executing the security tools.
- **`hexstrike_mcp.py`**: The Master Control Program (MCP) client. This script acts as the bridge between an AI agent (like Claude, GPT, etc.) and the `hexstrike_server`. It uses the `FastMCP` library to expose the server's tools as Python functions that the AI agent can call directly.

The primary entry point for an AI agent is the **`hexstrike_mcp.py`** script.

### 1.2. Command Syntax: Listing Tools
The framework does not provide a single command to list all tools directly from the command line. The intended method for an AI agent to discover tools is by inspecting the functions exposed by the `hexstrike_mcp.py` client.

For a human-readable and machine-readable list of all tools, this analysis has generated the `TOOLS_MANIFEST.json` file, which serves as the definitive catalog. The tool reference in this document is generated from that manifest.

### 1.3. Command Syntax: Running a Specific Tool
Tools are run by calling the corresponding Python function from the `hexstrike_mcp.py` client. The client then makes a REST API call to the `hexstrike_server.py` to execute the actual command-line tool.

**Example: Running Sublist3r (via `amass_scan`) on `example.com`**
While Sublist3r is not directly exposed, a similar tool for subdomain enumeration is `amass`. An AI agent would execute it as a Python function call:
```python
# This is a conceptual example of how an AI agent would call a tool.
# The actual function might be wrapped in a class instance.

amass_scan(domain="example.com", mode="enum")
```
This Python call is then translated by the `HexStrikeClient` into a REST API request:
- **Method**: `POST`
- **Endpoint**: `http://<server_address>:<port>/api/tools/amass`
- **Body**: `{"domain": "example.com", "mode": "enum", "additional_args": ""}`

### 1.4. Output Handling
The HexStrike framework handles tool output in a structured and centralized manner:
1.  **Standard Output/Error**: The `hexstrike_server` captures the `stdout` and `stderr` from the executed command-line tool.
2.  **JSON Response**: The output, along with metadata (success status, execution time, etc.), is packaged into a JSON object.
3.  **API Return**: This JSON object is returned as the response to the API call made by the `hexstrike_mcp.py` client.
4.  **Python Dictionary**: The MCP client receives the JSON response and returns it to the AI agent as a Python dictionary.

Essentially, all tool output is standardized into a JSON format and passed back to the caller through the API. The framework does not typically save results to a file or database by default, although some tools have parameters that allow for file output (e.g., `output_file`).

---

## 2. Directory Structure and Configuration

### 2.1. Directory Structure
The key files and directories in the repository are:
```
/
├── assets/                     # Images and logos for documentation
├── hexstrike_mcp.py            # The primary MCP client for AI agents
├── hexstrike_server.py           # The backend API server
├── hexstrike-ai-mcp.json       # Configuration for launching the MCP client
├── requirements.txt              # Python dependencies
├── README.md                     # Project overview and setup instructions
├── TOOLS_MANIFEST.json           # Machine-readable list of all tools (Generated)
├── JULES_FRAMEWORK_ANALYSIS.md   # This analysis document (Generated)
└── ...
```

### 2.2. Key Configuration Files
- **`requirements.txt`**: Lists all the Python packages required to run both the server and the MCP client. This is the primary file for setting up the Python environment.
- **`hexstrike-ai-mcp.json`**: This is a configuration file for an MCP launcher (like in Claude Desktop or Cursor). It specifies the command and arguments needed to start the `hexstrike_mcp.py` client, effectively registering it as a tool provider for the AI.

---

## 3. Tool Reference
The following is a comprehensive reference for all tools available in the HexStrike-AI framework, generated from the `TOOLS_MANIFEST.json` file.

### Uncategorized

#### `_`advanced_payload_generation`_`
**Description**: Generate advanced payloads with AI-powered evasion techniques and contextual adaptation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `attack_type` | Yes | `N/A` |
| `target_context` | No | `` |
| `evasion_level` | No | `standard` |
| `custom_constraints` | No | `` |

**API Call**: `POST /api/ai/advanced-payload-generation`

---
#### `_`ai_generate_attack_suite`_`
**Description**: Generate comprehensive attack suite with multiple payload types.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_url` | Yes | `N/A` |
| `attack_types` | No | `xss,sqli,lfi` |

---
#### `_`ai_generate_payload`_`
**Description**: Generate AI-powered contextual payloads for security testing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `attack_type` | Yes | `N/A` |
| `complexity` | No | `basic` |
| `technology` | No | `` |
| `url` | No | `` |

**API Call**: `POST /api/ai/generate_payload`

---
#### `_`ai_reconnaissance_workflow`_`
**Description**: Execute AI-driven reconnaissance workflow with intelligent tool chaining.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `depth` | No | `standard` |

**API Call**: `POST /api/intelligence/analyze-target`

---
#### `_`ai_test_payload`_`
**Description**: Test generated payload against target with AI analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `payload` | Yes | `N/A` |
| `target_url` | Yes | `N/A` |
| `method` | No | `GET` |

**API Call**: `POST /api/ai/test_payload`

---
#### `_`ai_vulnerability_assessment`_`
**Description**: Perform AI-driven vulnerability assessment with intelligent prioritization.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `focus_areas` | No | `all` |

**API Call**: `POST /api/intelligence/analyze-target`

---
#### `_`analyze_target_intelligence`_`
**Description**: Analyze target using AI-powered intelligence to create comprehensive profile.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |

**API Call**: `POST /api/intelligence/analyze-target`

---
#### `_`api_fuzzer`_`
**Description**: Advanced API endpoint fuzzing with intelligent parameter discovery.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `base_url` | Yes | `N/A` |
| `endpoints` | No | `` |
| `methods` | No | `GET,POST,PUT,DELETE` |
| `wordlist` | No | `/usr/share/wordlists/api/api-endpoints.txt` |

**API Call**: `POST /api/tools/api_fuzzer`

---
#### `_`api_schema_analyzer`_`
**Description**: Analyze API schemas and identify potential security issues.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `schema_url` | Yes | `N/A` |
| `schema_type` | No | `openapi` |

**API Call**: `POST /api/tools/api_schema_analyzer`

---
#### `_`arjun_scan`_`
**Description**: Execute Arjun for parameter discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `method` | No | `GET` |
| `data` | No | `` |
| `headers` | No | `` |
| `timeout` | No | `` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/arjun`

---
#### `_`autorecon_scan`_`
**Description**: Execute AutoRecon for comprehensive target enumeration with full parameter support.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | No | `` |
| `target_file` | No | `` |
| `ports` | No | `` |
| `output_dir` | No | `` |
| `max_scans` | No | `` |
| `max_port_scans` | No | `` |
| `heartbeat` | No | `` |
| `timeout` | No | `` |
| `target_timeout` | No | `` |
| `config_file` | No | `` |
| `global_file` | No | `` |
| `plugins_dir` | No | `` |
| `add_plugins_dir` | No | `` |
| `tags` | No | `` |
| `exclude_tags` | No | `` |
| `port_scans` | No | `` |
| `service_scans` | No | `` |
| `reports` | No | `` |
| `single_target` | No | `False` |
| `only_scans_dir` | No | `False` |
| `no_port_dirs` | No | `False` |
| `nmap` | No | `` |
| `nmap_append` | No | `` |
| `proxychains` | No | `False` |
| `disable_sanity_checks` | No | `False` |
| `disable_keyboard_control` | No | `False` |
| `force_services` | No | `` |
| `accessible` | No | `False` |
| `verbose` | No | `0` |
| `curl_path` | No | `` |
| `dirbuster_tool` | No | `` |
| `dirbuster_wordlist` | No | `` |
| `dirbuster_threads` | No | `` |
| `dirbuster_ext` | No | `` |
| `onesixtyone_community_strings` | No | `` |
| `global_username_wordlist` | No | `` |
| `global_password_wordlist` | No | `` |
| `global_domain` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/autorecon`

---
#### `_`bugbounty_authentication_bypass_testing`_`
**Description**: Create authentication bypass testing workflow for bug bounty hunting.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_url` | Yes | `N/A` |
| `auth_type` | No | `form` |

---
#### `_`bugbounty_business_logic_testing`_`
**Description**: Create business logic testing workflow for advanced bug bounty hunting.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `program_type` | No | `web` |

**API Call**: `POST /api/bugbounty/business-logic-workflow`

---
#### `_`bugbounty_comprehensive_assessment`_`
**Description**: Create comprehensive bug bounty assessment combining all specialized workflows.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `scope` | No | `` |
| `priority_vulns` | No | `rce,sqli,xss,idor,ssrf` |
| `include_osint` | No | `True` |
| `include_business_logic` | No | `True` |

**API Call**: `POST /api/bugbounty/comprehensive-assessment`

---
#### `_`bugbounty_file_upload_testing`_`
**Description**: Create file upload vulnerability testing workflow with bypass techniques.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_url` | Yes | `N/A` |

**API Call**: `POST /api/bugbounty/file-upload-testing`

---
#### `_`bugbounty_osint_gathering`_`
**Description**: Create OSINT (Open Source Intelligence) gathering workflow for bug bounty reconnaissance.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |

**API Call**: `POST /api/bugbounty/osint-workflow`

---
#### `_`bugbounty_reconnaissance_workflow`_`
**Description**: Create comprehensive reconnaissance workflow for bug bounty hunting.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `scope` | No | `` |
| `out_of_scope` | No | `` |
| `program_type` | No | `web` |

**API Call**: `POST /api/bugbounty/reconnaissance-workflow`

---
#### `_`bugbounty_vulnerability_hunting`_`
**Description**: Create vulnerability hunting workflow prioritized by impact and bounty potential.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `priority_vulns` | No | `rce,sqli,xss,idor,ssrf` |
| `bounty_range` | No | `unknown` |

**API Call**: `POST /api/bugbounty/vulnerability-hunting-workflow`

---
#### `_`burpsuite_alternative_scan`_`
**Description**: Comprehensive Burp Suite alternative combining HTTP framework and browser agent for complete web security testing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `scan_type` | No | `comprehensive` |
| `headless` | No | `True` |
| `max_depth` | No | `3` |
| `max_pages` | No | `50` |

**API Call**: `POST /api/tools/burpsuite-alternative`

---
#### `_`burpsuite_scan`_`
**Description**: Execute Burp Suite with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `project_file` | No | `` |
| `config_file` | No | `` |
| `target` | No | `` |
| `headless` | No | `False` |
| `scan_type` | No | `` |
| `scan_config` | No | `` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/burpsuite`

---
#### `_`clear_cache`_`
**Description**: Clear the cache on the HexStrike AI server.

**API Call**: `POST /api/cache/clear`

---
#### `_`comprehensive_api_audit`_`
**Description**: Comprehensive API security audit combining multiple testing techniques.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `base_url` | Yes | `N/A` |
| `schema_url` | No | `` |
| `jwt_token` | No | `` |
| `graphql_endpoint` | No | `` |

---
#### `_`correlate_threat_intelligence`_`
**Description**: Correlate threat intelligence across multiple sources with advanced analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `indicators` | Yes | `N/A` |
| `timeframe` | No | `30d` |
| `sources` | No | `all` |

**API Call**: `POST /api/vuln-intel/threat-feeds`

---
#### `_`create_attack_chain_ai`_`
**Description**: Create an intelligent attack chain using AI-driven tool sequencing and optimization.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `objective` | No | `comprehensive` |

**API Call**: `POST /api/intelligence/create-attack-chain`

---
#### `_`create_file`_`
**Description**: Create a file with specified content on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `filename` | Yes | `N/A` |
| `content` | Yes | `N/A` |
| `binary` | No | `False` |

**API Call**: `POST /api/files/create`

---
#### `_`create_scan_summary`_`
**Description**: Create a comprehensive scan summary report with beautiful visual formatting.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `tools_used` | Yes | `N/A` |
| `vulnerabilities_found` | No | `0` |
| `execution_time` | No | `0.0` |
| `findings` | No | `` |

**API Call**: `POST /api/visual/summary-report`

---
#### `_`create_vulnerability_report`_`
**Description**: Create a beautiful vulnerability report with severity-based styling and visual indicators.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `vulnerabilities` | Yes | `N/A` |
| `target` | No | `` |
| `scan_type` | No | `comprehensive` |

---
#### `_`delete_file`_`
**Description**: Delete a file or directory on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `filename` | Yes | `N/A` |

**API Call**: `POST /api/files/delete`

---
#### `_`detect_technologies_ai`_`
**Description**: Use AI to detect technologies and provide technology-specific testing recommendations.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |

**API Call**: `POST /api/intelligence/technology-detection`

---
#### `_`discover_attack_chains`_`
**Description**: Discover multi-stage attack chains for target software with vulnerability correlation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_software` | Yes | `N/A` |
| `attack_depth` | No | `3` |
| `include_zero_days` | No | `False` |

**API Call**: `POST /api/vuln-intel/attack-chains`

---
#### `_`display_system_metrics`_`
**Description**: Display current system metrics and performance indicators with visual formatting.

**API Call**: `GET /api/telemetry`

---
#### `_`dotdotpwn_scan`_`
**Description**: Execute DotDotPwn for directory traversal testing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `module` | No | `http` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/dotdotpwn`

---
#### `_`error_handling_statistics`_`
**Description**: Get intelligent error handling system statistics and recent error patterns.

**API Call**: `GET /api/error-handling/statistics`

---
#### `_`execute_command`_`
**Description**: Execute an arbitrary command on the HexStrike AI server with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `command` | Yes | `N/A` |
| `use_cache` | No | `True` |

---
#### `_`execute_python_script`_`
**Description**: Execute a Python script in a virtual environment on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `script` | Yes | `N/A` |
| `env_name` | No | `default` |
| `filename` | No | `` |

**API Call**: `POST /api/python/execute`

---
#### `_`format_tool_output_visual`_`
**Description**: Format tool output with beautiful visual styling, syntax highlighting, and structure.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `tool_name` | Yes | `N/A` |
| `output` | Yes | `N/A` |
| `success` | No | `True` |

**API Call**: `POST /api/visual/tool-output`

---
#### `_`generate_exploit_from_cve`_`
**Description**: Generate working exploits from CVE information using AI-powered analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `cve_id` | Yes | `N/A` |
| `target_os` | No | `` |
| `target_arch` | No | `x64` |
| `exploit_type` | No | `poc` |
| `evasion_level` | No | `none` |

**API Call**: `POST /api/vuln-intel/exploit-generate`

---
#### `_`generate_payload`_`
**Description**: Generate large payloads for testing and exploitation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `payload_type` | No | `buffer` |
| `size` | No | `1024` |
| `pattern` | No | `A` |
| `filename` | No | `` |

**API Call**: `POST /api/payloads/generate`

---
#### `_`get_cache_stats`_`
**Description**: Get cache statistics from the HexStrike AI server.

**API Call**: `GET /api/cache/stats`

---
#### `_`get_live_dashboard`_`
**Description**: Get a beautiful live dashboard showing all active processes with enhanced visual formatting.

**API Call**: `GET /api/processes/dashboard`

---
#### `_`get_process_dashboard`_`
**Description**: Get enhanced process dashboard with visual status indicators.

**API Call**: `GET /api/processes/dashboard`

---
#### `_`get_process_status`_`
**Description**: Get the status of a specific process.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `pid` | Yes | `N/A` |

**API Call**: `GET /api/processes/status/`

---
#### `_`get_telemetry`_`
**Description**: Get system telemetry from the HexStrike AI server.

**API Call**: `GET /api/telemetry`

---
#### `_`graphql_scanner`_`
**Description**: Advanced GraphQL security scanning and introspection.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `endpoint` | Yes | `N/A` |
| `introspection` | No | `True` |
| `query_depth` | No | `10` |
| `test_mutations` | No | `True` |

**API Call**: `POST /api/tools/graphql_scanner`

---
#### `_`hashpump_attack`_`
**Description**: Execute HashPump for hash length extension attacks with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `signature` | Yes | `N/A` |
| `data` | Yes | `N/A` |
| `key_length` | Yes | `N/A` |
| `append_data` | Yes | `N/A` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/hashpump`

---
#### `_`http_framework_test`_`
**Description**: Enhanced HTTP testing framework (Burp Suite alternative) for comprehensive web security testing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `method` | No | `GET` |
| `data` | No | `{}` |
| `headers` | No | `{}` |
| `cookies` | No | `{}` |
| `action` | No | `request` |

**API Call**: `POST /api/tools/http-framework`

---
#### `_`http_intruder`_`
**Description**: Simple Intruder (sniper) fuzzing. Iterates payloads over each param individually.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `method` | No | `GET` |
| `location` | No | `query` |
| `params` | No | `None` |
| `payloads` | No | `None` |
| `base_data` | No | `None` |
| `max_requests` | No | `100` |

**API Call**: `POST /api/tools/http-framework`

---
#### `_`http_repeater`_`
**Description**: Send a crafted request (Burp Repeater equivalent). request_spec keys: url, method, headers, cookies, data.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `request_spec` | Yes | `N/A` |

**API Call**: `POST /api/tools/http-framework`

---
#### `_`http_set_rules`_`
**Description**: Set match/replace rules used to rewrite parts of URL/query/headers/body before sending.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `rules` | Yes | `N/A` |

**API Call**: `POST /api/tools/http-framework`

---
#### `_`http_set_scope`_`
**Description**: Define in-scope host (and optionally subdomains) so out-of-scope requests are skipped.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `host` | Yes | `N/A` |
| `include_subdomains` | No | `True` |

**API Call**: `POST /api/tools/http-framework`

---
#### `_`install_python_package`_`
**Description**: Install a Python package in a virtual environment on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `package` | Yes | `N/A` |
| `env_name` | No | `default` |

**API Call**: `POST /api/python/install`

---
#### `_`intelligent_smart_scan`_`
**Description**: Execute an intelligent scan using AI-driven tool selection and parameter optimization.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `objective` | No | `comprehensive` |
| `max_tools` | No | `5` |

**API Call**: `POST /api/intelligence/smart-scan`

---
#### `_`john_crack`_`
**Description**: Execute John the Ripper for password cracking with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `hash_file` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/rockyou.txt` |
| `format_type` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/john`

---
#### `_`jwt_analyzer`_`
**Description**: Advanced JWT token analysis and vulnerability testing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `jwt_token` | Yes | `N/A` |
| `target_url` | No | `` |

**API Call**: `POST /api/tools/jwt_analyzer`

---
#### `_`list_active_processes`_`
**Description**: List all active processes on the HexStrike AI server.

**API Call**: `GET /api/processes/list`

---
#### `_`list_files`_`
**Description**: List files in a directory on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `directory` | No | `.` |

**API Call**: `GET /api/files/list`

---
#### `_`metasploit_run`_`
**Description**: Execute a Metasploit module with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `module` | Yes | `N/A` |
| `options` | No | `{}` |

**API Call**: `POST /api/tools/metasploit`

---
#### `_`modify_file`_`
**Description**: Modify an existing file on the HexStrike server.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `filename` | Yes | `N/A` |
| `content` | Yes | `N/A` |
| `append` | No | `False` |

**API Call**: `POST /api/files/modify`

---
#### `_`monitor_cve_feeds`_`
**Description**: Monitor CVE databases for new vulnerabilities with AI analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `hours` | No | `24` |
| `severity_filter` | No | `HIGH,CRITICAL` |
| `keywords` | No | `` |

**API Call**: `POST /api/vuln-intel/cve-monitor`

---
#### `_`nmap_advanced_scan`_`
**Description**: Execute advanced Nmap scans with custom NSE scripts and optimized timing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `scan_type` | No | `-sS` |
| `ports` | No | `` |
| `timing` | No | `T4` |
| `nse_scripts` | No | `` |
| `os_detection` | No | `False` |
| `version_detection` | No | `False` |
| `aggressive` | No | `False` |
| `stealth` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/nmap-advanced`

---
#### `_`optimize_tool_parameters_ai`_`
**Description**: Use AI to optimize tool parameters based on target profile and context.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `tool` | Yes | `N/A` |
| `context` | No | `{}` |

**API Call**: `POST /api/intelligence/optimize-parameters`

---
#### `_`pause_process`_`
**Description**: Pause a specific running process.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `pid` | Yes | `N/A` |

**API Call**: `POST /api/processes/pause/`

---
#### `_`research_zero_day_opportunities`_`
**Description**: Automated zero-day vulnerability research using AI analysis and pattern recognition.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_software` | Yes | `N/A` |
| `analysis_depth` | No | `standard` |
| `source_code_url` | No | `` |

**API Call**: `POST /api/vuln-intel/zero-day-research`

---
#### `_`resume_process`_`
**Description**: Resume a paused process.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `pid` | Yes | `N/A` |

**API Call**: `POST /api/processes/resume/`

---
#### `_`select_optimal_tools_ai`_`
**Description**: Use AI to select optimal security tools based on target analysis and testing objective.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `objective` | No | `comprehensive` |

**API Call**: `POST /api/intelligence/select-tools`

---
#### `_`server_health`_`
**Description**: Check the health status of the HexStrike AI server.

---
#### `_`terminate_process`_`
**Description**: Terminate a specific running process.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `pid` | Yes | `N/A` |

**API Call**: `POST /api/processes/terminate/`

---
#### `_`test_error_recovery`_`
**Description**: Test the intelligent error recovery system with simulated failures.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `tool_name` | Yes | `N/A` |
| `error_type` | No | `timeout` |
| `target` | No | `example.com` |

**API Call**: `POST /api/error-handling/test-recovery`

---
#### `_`threat_hunting_assistant`_`
**Description**: AI-powered threat hunting assistant with vulnerability correlation and attack simulation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target_environment` | Yes | `N/A` |
| `threat_indicators` | No | `` |
| `hunt_focus` | No | `general` |

---
#### `_`xsser_scan`_`
**Description**: Execute XSSer for XSS vulnerability testing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `params` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/xsser`

---
#### `_`zap_scan`_`
**Description**: Execute OWASP ZAP with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | No | `` |
| `scan_type` | No | `baseline` |
| `api_key` | No | `` |
| `daemon` | No | `False` |
| `port` | No | `8090` |
| `host` | No | `0.0.0.0` |
| `format_type` | No | `xml` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/zap`

---

### ☁️ Cloud & Container Security

#### `_`checkov_iac_scan`_`
**Description**: Execute Checkov for infrastructure as code security scanning.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `directory` | No | `.` |
| `framework` | No | `` |
| `check` | No | `` |
| `skip_check` | No | `` |
| `output_format` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/checkov`

---
#### `_`clair_vulnerability_scan`_`
**Description**: Execute Clair for container vulnerability analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `image` | Yes | `N/A` |
| `config` | No | `/etc/clair/config.yaml` |
| `output_format` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/clair`

---
#### `_`cloudmapper_analysis`_`
**Description**: Execute CloudMapper for AWS network visualization and security analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `action` | No | `collect` |
| `account` | No | `` |
| `config` | No | `config.json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/cloudmapper`

---
#### `_`docker_bench_security_scan`_`
**Description**: Execute Docker Bench for Security for Docker security assessment.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `checks` | No | `` |
| `exclude` | No | `` |
| `output_file` | No | `/tmp/docker-bench-results.json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/docker-bench-security`

---
#### `_`falco_runtime_monitoring`_`
**Description**: Execute Falco for runtime security monitoring.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `config_file` | No | `/etc/falco/falco.yaml` |
| `rules_file` | No | `` |
| `output_format` | No | `json` |
| `duration` | No | `60` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/falco`

---
#### `_`kube_bench_cis`_`
**Description**: Execute kube-bench for CIS Kubernetes benchmark checks.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `targets` | No | `` |
| `version` | No | `` |
| `config_dir` | No | `` |
| `output_format` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/kube-bench`

---
#### `_`kube_hunter_scan`_`
**Description**: Execute kube-hunter for Kubernetes penetration testing.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | No | `` |
| `remote` | No | `` |
| `cidr` | No | `` |
| `interface` | No | `` |
| `active` | No | `False` |
| `report` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/kube-hunter`

---
#### `_`pacu_exploitation`_`
**Description**: Execute Pacu for AWS exploitation framework.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `session_name` | No | `hexstrike_session` |
| `modules` | No | `` |
| `data_services` | No | `` |
| `regions` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/pacu`

---
#### `_`prowler_scan`_`
**Description**: Execute Prowler for comprehensive cloud security assessment.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `provider` | No | `aws` |
| `profile` | No | `default` |
| `region` | No | `` |
| `checks` | No | `` |
| `output_dir` | No | `/tmp/prowler_output` |
| `output_format` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/prowler`

---
#### `_`scout_suite_assessment`_`
**Description**: Execute Scout Suite for multi-cloud security assessment.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `provider` | No | `aws` |
| `profile` | No | `default` |
| `report_dir` | No | `/tmp/scout-suite` |
| `services` | No | `` |
| `exceptions` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/scout-suite`

---
#### `_`terrascan_iac_scan`_`
**Description**: Execute Terrascan for infrastructure as code security scanning.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `scan_type` | No | `all` |
| `iac_dir` | No | `.` |
| `policy_type` | No | `` |
| `output_format` | No | `json` |
| `severity` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/terrascan`

---
#### `_`trivy_scan`_`
**Description**: Execute Trivy for container and filesystem vulnerability scanning.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `scan_type` | No | `image` |
| `target` | No | `` |
| `output_format` | No | `json` |
| `severity` | No | `` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/trivy`

---

### 🌐 Web Application Security Testing

#### `_`anew_data_processing`_`
**Description**: Execute anew for appending new lines to files (useful for data processing).

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `input_data` | Yes | `N/A` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/anew`

---
#### `_`arjun_parameter_discovery`_`
**Description**: Execute Arjun for HTTP parameter discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `method` | No | `GET` |
| `wordlist` | No | `` |
| `delay` | No | `0` |
| `threads` | No | `25` |
| `stable` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/arjun`

---
#### `_`dalfox_xss_scan`_`
**Description**: Execute Dalfox for advanced XSS vulnerability scanning with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `pipe_mode` | No | `False` |
| `blind` | No | `False` |
| `mining_dom` | No | `True` |
| `mining_dict` | No | `True` |
| `custom_payload` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/dalfox`

---
#### `_`dirb_scan`_`
**Description**: Execute Dirb for directory brute forcing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/dirb/common.txt` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/dirb`

---
#### `_`dirsearch_scan`_`
**Description**: Execute Dirsearch for advanced directory and file discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `extensions` | No | `php,html,js,txt,xml,json` |
| `wordlist` | No | `/usr/share/wordlists/dirsearch/common.txt` |
| `threads` | No | `30` |
| `recursive` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/dirsearch`

---
#### `_`feroxbuster_scan`_`
**Description**: Execute Feroxbuster for recursive content discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/dirb/common.txt` |
| `threads` | No | `10` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/feroxbuster`

---
#### `_`ffuf_scan`_`
**Description**: Execute FFuf for web fuzzing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/dirb/common.txt` |
| `mode` | No | `directory` |
| `match_codes` | No | `200,204,301,302,307,401,403` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/ffuf`

---
#### `_`gau_discovery`_`
**Description**: Execute Gau (Get All URLs) for URL discovery from multiple sources with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `providers` | No | `wayback,commoncrawl,otx,urlscan` |
| `include_subs` | No | `True` |
| `blacklist` | No | `png,jpg,gif,jpeg,swf,woff,svg,pdf,css,ico` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/gau`

---
#### `_`gobuster_scan`_`
**Description**: Execute Gobuster to find directories, DNS subdomains, or virtual hosts with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `mode` | No | `dir` |
| `wordlist` | No | `/usr/share/wordlists/dirb/common.txt` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/gobuster`

---
#### `_`hakrawler_crawl`_`
**Description**: Execute Hakrawler for web endpoint discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `depth` | No | `2` |
| `forms` | No | `True` |
| `robots` | No | `True` |
| `sitemap` | No | `True` |
| `wayback` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/hakrawler`

---
#### `_`httpx_probe`_`
**Description**: Execute HTTPx for HTTP probing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `targets` | No | `` |
| `target_file` | No | `` |
| `ports` | No | `` |
| `methods` | No | `GET` |
| `status_code` | No | `` |
| `content_length` | No | `False` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/httpx`

---
#### `_`jaeles_vulnerability_scan`_`
**Description**: Execute Jaeles for advanced vulnerability scanning with custom signatures.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `signatures` | No | `` |
| `config` | No | `` |
| `threads` | No | `20` |
| `timeout` | No | `20` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/jaeles`

---
#### `_`katana_crawl`_`
**Description**: Execute Katana for next-generation crawling and spidering with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `depth` | No | `3` |
| `js_crawl` | No | `True` |
| `form_extraction` | No | `True` |
| `output_format` | No | `json` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/katana`

---
#### `_`nikto_scan`_`
**Description**: Execute Nikto web vulnerability scanner with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/nikto`

---
#### `_`nuclei_scan`_`
**Description**: Execute Nuclei vulnerability scanner with enhanced logging and real-time progress.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `severity` | No | `` |
| `tags` | No | `` |
| `template` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/nuclei`

---
#### `_`paramspider_mining`_`
**Description**: Execute ParamSpider for parameter mining from web archives with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `level` | No | `2` |
| `exclude` | No | `png,jpg,gif,jpeg,swf,woff,svg,pdf,css,ico` |
| `output` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/paramspider`

---
#### `_`qsreplace_parameter_replacement`_`
**Description**: Execute qsreplace for query string parameter replacement.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `urls` | Yes | `N/A` |
| `replacement` | No | `FUZZ` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/qsreplace`

---
#### `_`sqlmap_scan`_`
**Description**: Execute SQLMap for SQL injection testing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `data` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/sqlmap`

---
#### `_`uro_url_filtering`_`
**Description**: Execute uro for filtering out similar URLs.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `urls` | Yes | `N/A` |
| `whitelist` | No | `` |
| `blacklist` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/uro`

---
#### `_`wafw00f_scan`_`
**Description**: Execute wafw00f to identify and fingerprint WAF products with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/wafw00f`

---
#### `_`waybackurls_discovery`_`
**Description**: Execute Waybackurls for historical URL discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `get_versions` | No | `False` |
| `no_subs` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/waybackurls`

---
#### `_`wfuzz_scan`_`
**Description**: Execute Wfuzz for web application fuzzing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/dirb/common.txt` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/wfuzz`

---
#### `_`wpscan_analyze`_`
**Description**: Execute WPScan for WordPress vulnerability scanning with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/wpscan`

---
#### `_`x8_parameter_discovery`_`
**Description**: Execute x8 for hidden parameter discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `wordlist` | No | `/usr/share/wordlists/x8/params.txt` |
| `method` | No | `GET` |
| `body` | No | `` |
| `headers` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/x8`

---

### 🏆 CTF & Forensics Tools

#### `_`exiftool_extract`_`
**Description**: Execute ExifTool for metadata extraction with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `file_path` | Yes | `N/A` |
| `output_format` | No | `` |
| `tags` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/exiftool`

---
#### `_`foremost_carving`_`
**Description**: Execute Foremost for file carving with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `input_file` | Yes | `N/A` |
| `output_dir` | No | `/tmp/foremost_output` |
| `file_types` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/foremost`

---
#### `_`steghide_analysis`_`
**Description**: Execute Steghide for steganography analysis with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `action` | Yes | `N/A` |
| `cover_file` | Yes | `N/A` |
| `embed_file` | No | `` |
| `passphrase` | No | `` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/steghide`

---
#### `_`volatility3_analyze`_`
**Description**: Execute Volatility3 for advanced memory forensics with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `memory_file` | Yes | `N/A` |
| `plugin` | Yes | `N/A` |
| `output_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/volatility3`

---

### 🔍 Network Reconnaissance & Scanning

#### `_`amass_scan`_`
**Description**: Execute Amass for subdomain enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `mode` | No | `enum` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/amass`

---
#### `_`arp_scan_discovery`_`
**Description**: Execute arp-scan for network discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | No | `` |
| `interface` | No | `` |
| `local_network` | No | `False` |
| `timeout` | No | `500` |
| `retry` | No | `3` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/arp-scan`

---
#### `_`autorecon_comprehensive`_`
**Description**: Execute AutoRecon for comprehensive automated reconnaissance.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `output_dir` | No | `/tmp/autorecon` |
| `port_scans` | No | `top-100-ports` |
| `service_scans` | No | `default` |
| `heartbeat` | No | `60` |
| `timeout` | No | `300` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/autorecon`

---
#### `_`dnsenum_scan`_`
**Description**: Execute dnsenum for DNS enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `dns_server` | No | `` |
| `wordlist` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/dnsenum`

---
#### `_`enum4linux_ng_advanced`_`
**Description**: Execute Enum4linux-ng for advanced SMB enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `username` | No | `` |
| `password` | No | `` |
| `domain` | No | `` |
| `shares` | No | `True` |
| `users` | No | `True` |
| `groups` | No | `True` |
| `policy` | No | `True` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/enum4linux-ng`

---
#### `_`enum4linux_scan`_`
**Description**: Execute Enum4linux for SMB enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `additional_args` | No | `-a` |

**API Call**: `POST /api/tools/enum4linux`

---
#### `_`fierce_scan`_`
**Description**: Execute fierce for DNS reconnaissance with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `dns_server` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/fierce`

---
#### `_`masscan_high_speed`_`
**Description**: Execute Masscan for high-speed Internet-scale port scanning with intelligent rate limiting.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `ports` | No | `1-65535` |
| `rate` | No | `1000` |
| `interface` | No | `` |
| `router_mac` | No | `` |
| `source_ip` | No | `` |
| `banners` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/masscan`

---
#### `_`nbtscan_netbios`_`
**Description**: Execute nbtscan for NetBIOS name scanning with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `verbose` | No | `False` |
| `timeout` | No | `2` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/nbtscan`

---
#### `_`netexec_scan`_`
**Description**: Execute NetExec (formerly CrackMapExec) for network enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `protocol` | No | `smb` |
| `username` | No | `` |
| `password` | No | `` |
| `hash_value` | No | `` |
| `module` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/netexec`

---
#### `_`nmap_scan`_`
**Description**: Execute an enhanced Nmap scan against a target with real-time logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `scan_type` | No | `-sV` |
| `ports` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/nmap`

---
#### `_`responder_credential_harvest`_`
**Description**: Execute Responder for credential harvesting with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `interface` | No | `eth0` |
| `analyze` | No | `False` |
| `wpad` | No | `True` |
| `force_wpad_auth` | No | `False` |
| `fingerprint` | No | `False` |
| `duration` | No | `300` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/responder`

---
#### `_`rpcclient_enumeration`_`
**Description**: Execute rpcclient for RPC enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `username` | No | `` |
| `password` | No | `` |
| `domain` | No | `` |
| `commands` | No | `enumdomusers;enumdomgroups;querydominfo` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/rpcclient`

---
#### `_`rustscan_fast_scan`_`
**Description**: Execute Rustscan for ultra-fast port scanning with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `ports` | No | `` |
| `ulimit` | No | `5000` |
| `batch_size` | No | `4500` |
| `timeout` | No | `1500` |
| `scripts` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/rustscan`

---
#### `_`smbmap_scan`_`
**Description**: Execute SMBMap for SMB share enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `username` | No | `` |
| `password` | No | `` |
| `domain` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/smbmap`

---
#### `_`subfinder_scan`_`
**Description**: Execute Subfinder for passive subdomain enumeration with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `silent` | No | `True` |
| `all_sources` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/subfinder`

---

### 🔐 Authentication & Password Security

#### `_`hashcat_crack`_`
**Description**: Execute Hashcat for advanced password cracking with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `hash_file` | Yes | `N/A` |
| `hash_type` | Yes | `N/A` |
| `attack_mode` | No | `0` |
| `wordlist` | No | `/usr/share/wordlists/rockyou.txt` |
| `mask` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/hashcat`

---
#### `_`hydra_attack`_`
**Description**: Execute Hydra for password brute forcing with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `target` | Yes | `N/A` |
| `service` | Yes | `N/A` |
| `username` | No | `` |
| `username_file` | No | `` |
| `password` | No | `` |
| `password_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/hydra`

---

### 🔥 Bug Bounty & OSINT Arsenal

#### `_`browser_agent_inspect`_`
**Description**: AI-powered browser agent for comprehensive web application inspection and security analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `url` | Yes | `N/A` |
| `headless` | No | `True` |
| `wait_time` | No | `5` |
| `action` | No | `navigate` |
| `proxy_port` | No | `None` |
| `active_tests` | No | `False` |

**API Call**: `POST /api/tools/browser-agent`

---
#### `_`paramspider_discovery`_`
**Description**: Execute ParamSpider for parameter discovery with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `domain` | Yes | `N/A` |
| `exclude` | No | `` |
| `output_file` | No | `` |
| `level` | No | `2` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/paramspider`

---
#### `_`vulnerability_intelligence_dashboard`_`
**Description**: Get a comprehensive vulnerability intelligence dashboard with latest threats and trends.

**API Call**: `POST /api/vuln-intel/cve-monitor`

---

### 🔬 Binary Analysis & Reverse Engineering

#### `_`angr_symbolic_execution`_`
**Description**: Execute angr for symbolic execution and binary analysis.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `script_content` | No | `` |
| `find_address` | No | `` |
| `avoid_addresses` | No | `` |
| `analysis_type` | No | `symbolic` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/angr`

---
#### `_`binwalk_analyze`_`
**Description**: Execute Binwalk for firmware and file analysis with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `file_path` | Yes | `N/A` |
| `extract` | No | `False` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/binwalk`

---
#### `_`checksec_analyze`_`
**Description**: Check security features of a binary with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |

**API Call**: `POST /api/tools/checksec`

---
#### `_`gdb_analyze`_`
**Description**: Execute GDB for binary analysis and debugging with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `commands` | No | `` |
| `script_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/gdb`

---
#### `_`gdb_peda_debug`_`
**Description**: Execute GDB with PEDA for enhanced debugging and exploitation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | No | `` |
| `commands` | No | `` |
| `attach_pid` | No | `0` |
| `core_file` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/gdb-peda`

---
#### `_`ghidra_analysis`_`
**Description**: Execute Ghidra for advanced binary analysis and reverse engineering.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `project_name` | No | `hexstrike_analysis` |
| `script_file` | No | `` |
| `analysis_timeout` | No | `300` |
| `output_format` | No | `xml` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/ghidra`

---
#### `_`libc_database_lookup`_`
**Description**: Execute libc-database for libc identification and offset lookup.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `action` | No | `find` |
| `symbols` | No | `` |
| `libc_id` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/libc-database`

---
#### `_`msfvenom_generate`_`
**Description**: Execute MSFVenom for payload generation with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `payload` | Yes | `N/A` |
| `format_type` | No | `` |
| `output_file` | No | `` |
| `encoder` | No | `` |
| `iterations` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/msfvenom`

---
#### `_`objdump_analyze`_`
**Description**: Analyze a binary using objdump with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `disassemble` | No | `True` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/objdump`

---
#### `_`one_gadget_search`_`
**Description**: Execute one_gadget to find one-shot RCE gadgets in libc.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `libc_path` | Yes | `N/A` |
| `level` | No | `1` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/one-gadget`

---
#### `_`pwninit_setup`_`
**Description**: Execute pwninit for CTF binary exploitation setup.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `libc` | No | `` |
| `ld` | No | `` |
| `template_type` | No | `python` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/pwninit`

---
#### `_`pwntools_exploit`_`
**Description**: Execute Pwntools for exploit development and automation.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `script_content` | No | `` |
| `target_binary` | No | `` |
| `target_host` | No | `` |
| `target_port` | No | `0` |
| `exploit_type` | No | `local` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/pwntools`

---
#### `_`radare2_analyze`_`
**Description**: Execute Radare2 for binary analysis and reverse engineering with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `commands` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/radare2`

---
#### `_`ropgadget_search`_`
**Description**: Search for ROP gadgets in a binary using ROPgadget with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `gadget_type` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/ropgadget`

---
#### `_`ropper_gadget_search`_`
**Description**: Execute ropper for advanced ROP/JOP gadget searching.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `binary` | Yes | `N/A` |
| `gadget_type` | No | `rop` |
| `quality` | No | `1` |
| `arch` | No | `` |
| `search_string` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/ropper`

---
#### `_`strings_extract`_`
**Description**: Extract strings from a binary file with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `file_path` | Yes | `N/A` |
| `min_len` | No | `4` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/strings`

---
#### `_`volatility_analyze`_`
**Description**: Execute Volatility for memory forensics analysis with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `memory_file` | Yes | `N/A` |
| `plugin` | Yes | `N/A` |
| `profile` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/volatility`

---
#### `_`xxd_hexdump`_`
**Description**: Create a hex dump of a file using xxd with enhanced logging.

**Parameters**:
| Name | Required | Default Value |
|------|----------|---------------|
| `file_path` | Yes | `N/A` |
| `offset` | No | `0` |
| `length` | No | `` |
| `additional_args` | No | `` |

**API Call**: `POST /api/tools/xxd`

---