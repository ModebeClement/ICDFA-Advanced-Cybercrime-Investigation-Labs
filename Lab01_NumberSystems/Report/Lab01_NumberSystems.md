# Advanced Static Malware Analysis of `Malware.stage0.exe.malz`

## Project Overview

This project documents a controlled static malware analysis of `Malware.stage0.exe.malz`, a 32-bit Windows Portable Executable identified as a Nim-compiled GUI application. The analysis was performed inside an isolated Windows 10 FLARE-VM hosted in Oracle VirtualBox.

The investigation focused on safe sample handling, cryptographic identification, strings extraction, PE structure examination, import analysis, API-centric behavioural reconstruction, anti-analysis indicators, and conservative MITRE ATT&CK mapping. All conclusions are limited to the evidence captured during the laboratory workflow. No dynamic execution findings are claimed.

> **Safety Notice:** The analysed file is a malware sample. It was handled only inside an isolated laboratory environment. This repository should not contain a live executable sample. Do not download, execute, or handle malware outside an appropriately isolated and authorised environment.

## Executive Summary

The examined specimen is a PE32 executable for 32-bit x86 systems, compiled with Nim and configured as a Windows GUI application. It was extracted from a password-protected archive, identified with MD5 and SHA256 hashes, and examined using PowerShell, Detect It Easy, a PE analysis tool, Sysinternals Strings, 7-Zip, and MalAPI as a classification reference.

Static evidence revealed:

- Entry point: `0x004014A0`
- Image base: `0x00400000`
- Image size: `0x00065000`
- Compilation timestamp: `2021-10-07 10:43:04`
- Fifteen PE sections
- An unknown overlay at offset `0x00051400`, size `0xE733`
- Two TLS callbacks located in the `.text` section
- A compact import table dominated by `KERNEL32.dll`, `msvcrt.dll`, and `USER32.dll`
- Dynamic API resolution and memory-management functions, including `LoadLibraryA`, `GetProcAddress`, `VirtualAlloc`, and `VirtualProtect`
- Timing and exception-handling functions with potential anti-analysis relevance

The strongest supported assessment is that the specimen is consistent with a **loader or stager exhibiting moderate anti-analysis characteristics**. The available evidence does not demonstrate persistence, privilege escalation, classic remote process injection, direct network communication, credential access, vulnerability exploitation, or a reconstructable command-and-control protocol.

## Objectives

- Handle and extract the malware sample safely in an isolated environment.
- Establish a reproducible file identity using MD5 and SHA256 hashing.
- Identify the file type, architecture, compiler, subsystem, image base, image size, and entry point.
- Extract and inspect readable strings for potential indicators and structural artefacts.
- Examine PE sections, overlay data, imports, exports, resources, and TLS callbacks.
- Analyse imported APIs for behavioural and anti-analysis relevance.
- Reconstruct the most defensible probable execution sequence from static evidence.
- Map supported observations conservatively to MITRE ATT&CK techniques.
- Document both positive findings and evidentiary limitations without speculation.

## Professional Value

This project demonstrates the capacity to examine a suspicious Windows executable methodically, preserve evidentiary continuity, validate file identity across tools, interpret PE structures, reason from imported APIs, distinguish evidence from inference, and communicate technical findings in a format suitable for security operations, malware triage, incident response, and digital forensics teams.

The work also demonstrates an important professional discipline: unsupported behaviours were not asserted merely because they are commonly associated with malware. Findings were bounded by the available static evidence, confidence levels were stated, and missing evidence was recorded explicitly.

## Skills Demonstrated

- Static malware analysis
- Windows PE file analysis
- Digital evidence handling
- Cryptographic hash verification
- Strings extraction and review
- Import Address Table analysis
- API-centric behavioural analysis
- Threat-focused hypothesis development
- Anti-analysis indicator identification
- Conservative MITRE ATT&CK mapping
- Security operations triage
- Technical documentation and evidence reporting

## Technologies and Tools Used

| Technology or Tool | Purpose |
|---|---|
| FLARE-VM on Windows 10 | Isolated malware-analysis environment |
| Oracle VirtualBox | Virtualisation and laboratory isolation |
| 7-Zip | Controlled extraction of the protected archive |
| PowerShell `Get-FileHash` | MD5 and SHA256 calculation |
| Detect It Easy (DIE) | PE identification, compiler detection, metadata review, overlay detection, and entry-point inspection |
| PE analysis tooling | Independent hash validation and PE structure examination |
| Sysinternals Strings | Static extraction of readable strings |
| Notepad | Review of the exported strings file |
| MalAPI | Qualitative API-category reference |
| MITRE ATT&CK | Conservative behavioural technique mapping |

## Lab Environment

| Item | Configuration |
|---|---|
| Analysis VM | `ACI803_FLARE_Win10` |
| Operating system | Windows 10 |
| Virtualisation platform | Oracle VirtualBox |
| Reported version | `7.2.6 r172322 (Qt 6.8.0 on Windows)` |
| Target sample | `Malware.stage0.exe.malz` |
| Analysis type | Static analysis only |
| Safety control | Isolated FLARE-VM environment |

## Repository Structure

```text
ACI803-Advanced-Malware-Static-Analysis/
├── README.md
├── Screenshots/
│   ├── image1.png
│   ├── image2.png
│   ├── image3.png
│   ├── image4.png
│   ├── image5.png
│   ├── image6.png
│   ├── image7.png
│   ├── image8.png
│   ├── image9.png
│   ├── image10.png
│   ├── image11.png
│   ├── image12.png
│   ├── image13.png
│   ├── image14.png
│   ├── image15.png
│   ├── image16.png
│   ├── image17.png
│   ├── image18.png
│   ├── image19.png
│   └── image20.png
└── Documentation/
    └── ACI803_Advanced_Malware_Static_Analysis_Report.pdf
```

> The screenshots must remain in the exact order shown above. Their filenames correspond to their embedded chronological order in the source report.

## Methodology

The investigation followed a controlled, evidence-driven workflow:

1. Confirm the protected archive was present inside the isolated VM.
2. Use 7-Zip to initiate controlled extraction without executing the sample.
3. Obtain the repository-provided extraction password.
4. Confirm the extracted `.malz` specimen was available for analysis.
5. Calculate the MD5 and SHA256 hashes in PowerShell.
6. Cross-check both digests independently in PE analysis tooling.
7. Identify the PE format, architecture, compiler, subsystem, image metadata, and entry point with Detect It Easy.
8. Extract readable strings to `strings.txt` and inspect the visible output.
9. Review overlay data and other structural indicators relevant to staging or packing.
10. Examine the compilation timestamp, imports, exports, resources, and TLS callbacks.
11. Classify imported APIs by functional category and reconstruct a probable static execution sequence.
12. Document supported behaviours, unsupported hypotheses, evidentiary limitations, and conservative ATT&CK mappings.

## Evidence and Analysis

### 1. Initial Sample Handling

#### Step 1: Malware archive present in the Downloads directory

![Malware archive present in the Downloads directory](Screenshots/image1.png)

- **Action performed:** The Downloads directory was reviewed to confirm the malware archive was present before extraction.
- **Visible evidence:** The source archive is visible inside the analysis workstation.
- **Importance:** This establishes the beginning of the documented handling sequence and supports continuity from acquisition to examination.
- **Competency demonstrated:** Evidence handling, laboratory discipline, and safe malware-analysis preparation.

#### Step 2: 7-Zip context menu opened for controlled extraction

![7-Zip context menu used for controlled extraction](Screenshots/image2.png)

- **Action performed:** The archive was accessed through 7-Zip rather than by attempting to execute its contents.
- **Visible evidence:** The 7-Zip extraction options are displayed for the protected archive.
- **Importance:** Controlled extraction reduces accidental execution risk and preserves the original archive.
- **Competency demonstrated:** Safe sample handling and use of an isolated malware-analysis workflow.

#### Step 3: Password prompt displayed during extraction

![Password prompt for protected malware archive](Screenshots/image3.png)

- **Action performed:** Extraction was initiated and the archive requested its password.
- **Visible evidence:** A password-entry prompt is displayed.
- **Importance:** The screenshot documents that the sample was distributed in a protected archive, a common safety measure used by malware repositories.
- **Competency demonstrated:** Controlled evidence acquisition and secure handling of potentially harmful files.

#### Step 4: Repository password obtained from the supplied password file

![Repository password file used for extraction](Screenshots/image4.png)

- **Action performed:** The repository-provided password file was reviewed to obtain the extraction password.
- **Visible evidence:** The password value `infected` is shown.
- **Importance:** This documents the authorised method used to access the protected laboratory sample without altering it.
- **Competency demonstrated:** Procedural accuracy, evidence traceability, and adherence to laboratory handling controls.

#### Step 5: Extracted malware sample confirmed

![Extracted Malware.stage0.exe.malz sample](Screenshots/image5.png)

- **Action performed:** The Downloads folder was reviewed after extraction.
- **Visible evidence:** `Malware.stage0.exe.malz` is present as the working specimen.
- **Importance:** This closes the extraction phase and identifies the exact file used in all subsequent analysis.
- **Competency demonstrated:** Sample verification and maintenance of a consistent examination target.

### 2. Cryptographic Identification and Cross-Verification

#### Step 6i: MD5 calculated with PowerShell

![PowerShell MD5 calculation](Screenshots/image6.png)

- **Action performed:** PowerShell `Get-FileHash` was used to calculate the sample's MD5 digest.
- **Visible evidence:** `6D8895C63A77EBE5E49B656BDEFDB822`
- **Importance:** The digest provides a baseline identifier for sample tracking, cross-tool comparison, and de-duplication.
- **Competency demonstrated:** Cryptographic hashing and command-line evidence generation.

#### Step 6ii: MD5 independently cross-verified

![MD5 cross-verification in PE analysis tool](Screenshots/image7.png)

- **Action performed:** The MD5 digest was checked in a separate PE analysis tool.
- **Visible evidence:** The tool reproduces `6D8895C63A77EBE5E49B656BDEFDB822`.
- **Importance:** Matching results reduce transcription risk and confirm that the same specimen remained under examination.
- **Competency demonstrated:** Evidence validation, analytical reproducibility, and cross-tool verification.

#### Step 7i: SHA256 calculated with PowerShell

![PowerShell SHA256 calculation](Screenshots/image8.png)

- **Action performed:** PowerShell `Get-FileHash` was used to calculate the SHA256 digest.
- **Visible evidence:** `FCA62097B364B2F0338C5E4C5BAC86134CEDFFA4F8DDF27EE9901734128952E3`
- **Importance:** SHA256 provides a stronger forensic identifier suitable for incident-response reporting and sample correlation.
- **Competency demonstrated:** Forensic file identification and reliable command-line analysis.

#### Step 7ii: SHA256 independently cross-verified

![SHA256 cross-verification in PE analysis tool](Screenshots/image9.png)

- **Action performed:** The SHA256 result was checked in the PE analysis tool.
- **Visible evidence:** The same SHA256 digest appears in the independent tool.
- **Importance:** Cross-verification strengthens confidence in sample identity before deeper structural analysis.
- **Competency demonstrated:** Quality assurance and repeatable forensic methodology.

#### Verified Sample Identifiers

| Identifier | Value | Evidence status |
|---|---|---|
| MD5 | `6D8895C63A77EBE5E49B656BDEFDB822` | Verified across two tools |
| SHA256 | `FCA62097B364B2F0338C5E4C5BAC86134CEDFFA4F8DDF27EE9901734128952E3` | Verified across two tools |
| IMPHASH | Not explicitly calculated in the captured evidence | Not evidenced |

### 3. File Identification and Strings Analysis

#### Step 8: Detect It Easy identifies a PE32 Nim executable

![Detect It Easy PE identification](Screenshots/image10.png)

- **Action performed:** The specimen was loaded into Detect It Easy for format and compiler triage.
- **Visible evidence:** A PE32, I386, Windows GUI executable compiled with Nim is identified. The view reports image size `0x00065000`, image base `0x00400000`, and entry point `0x004014A0`.
- **Importance:** These values define the sample's architecture and load characteristics and provide reference points for later PE and control-flow analysis.
- **Competency demonstrated:** Malware triage, PE header interpretation, and compiler identification.

#### Step 9: Strings extracted to a text file

![Sysinternals Strings output redirected to strings.txt](Screenshots/image11.png)

- **Action performed:** Sysinternals Strings was executed against the specimen and the results were redirected to `strings.txt`.
- **Visible evidence:** A reproducible command-line strings-extraction workflow is shown.
- **Importance:** Static strings can expose infrastructure, API names, file paths, execution markers, and structural artefacts without running the sample.
- **Competency demonstrated:** Command-line static analysis and artefact extraction.

#### Step 10: Extracted strings reviewed in Notepad

![Review of extracted strings](Screenshots/image12.png)

- **Action performed:** The exported strings were opened and reviewed.
- **Visible evidence:** The visible excerpt contains the DOS-stub message and section names such as `.text`, `.data`, `.rdata`, `.idata`, `.CRT`, and `.tls`.
- **Importance:** The `.tls` string is consistent with later PE evidence showing TLS callbacks. No clear IP address, domain, or hard-coded file path is visible in the captured portion.
- **Competency demonstrated:** Strings triage, indicator review, and evidence-bounded reporting.

### 4. Structural Indicators Relevant to Packing or Staging

#### Step 11: Unknown overlay identified

![Unknown overlay in Detect It Easy](Screenshots/image13.png)

- **Action performed:** Detect It Easy's structural metadata was reviewed for appended or non-standard content.
- **Visible evidence:** An unknown overlay begins at offset `0x00051400` and has size `0xE733`.
- **Importance:** Overlay data can contain appended configuration, another stage, or non-standard content beyond the normal PE sections. It is suspicious but is not, by itself, proof of packing.
- **Competency demonstrated:** PE structure analysis, anomaly identification, and cautious interpretation.

A graphical entropy plot was not captured. Packing is therefore assessed only as a **moderate suspicion**, not a confirmed finding. Supporting observations are the unknown overlay, fifteen-section layout, Nim runtime characteristics, and TLS callbacks.

### 5. PE Header Deep Dive

#### Step 12: Compilation timestamp examined

![PE compilation timestamp](Screenshots/image14.png)

- **Action performed:** The timestamp in the PE header was reviewed.
- **Visible evidence:** `2021-10-07 10:43:04`
- **Importance:** Compilation timestamps can support temporal assessment and identify obvious timestomping anomalies. No direct evidence of timestamp manipulation appears in the supplied material.
- **Competency demonstrated:** PE metadata examination and cautious timestamp validation.

#### Step 13: Original entry point identified

![Original entry point in Detect It Easy](Screenshots/image15.png)

- **Action performed:** The declared PE entry point was located.
- **Visible evidence:** The entry point is `0x004014A0`.
- **Importance:** The address is a key control-flow landmark. However, the later discovery of TLS callbacks means code may execute before control reaches this entry point.
- **Competency demonstrated:** PE control-flow orientation and static reverse-engineering preparation.

#### Step 14: Import Address Table reviewed

![Import Address Table and visible APIs](Screenshots/image16.png)

- **Action performed:** Imported DLLs and functions were examined to infer probable capabilities.
- **Visible evidence:** The imports are dominated by `KERNEL32.dll`, `msvcrt.dll`, and `USER32.dll`. Visible functions include `GetProcAddress`, `LoadLibraryA`, `GetStartupInfoA`, `GetTickCount`, `QueryPerformanceCounter`, `Sleep`, `SetUnhandledExceptionFilter`, `UnhandledExceptionFilter`, `VirtualAlloc`, `VirtualFree`, `VirtualProtect`, `VirtualQuery`, `TerminateProcess`, and `TlsGetValue`.
- **Importance:** The combination of dynamic resolution, memory management, timing, and exception handling supports a loader or stager hypothesis more strongly than any individual API does alone.
- **Competency demonstrated:** Import analysis, API clustering, and behavioural hypothesis development.

No direct imports for `VirtualAllocEx`, `WriteProcessMemory`, or `CreateRemoteThread` are visible, so classic remote process injection is not evidenced. The captured imports also do not show WinINet, WinHTTP, `WS2_32`, registry or service persistence, OLE/COM, or WMI libraries.

#### Step 15: Export table examined

![Export table unavailable](Screenshots/image17.png)

- **Action performed:** The sample was checked for a standard export table.
- **Visible evidence:** The Export control is unavailable.
- **Importance:** The specimen does not expose conventional analyst-visible exported routines. This is consistent with an executable stage rather than a normal DLL-style component, although the absence of exports does not independently exclude reflective techniques.
- **Competency demonstrated:** PE export analysis and interpretation of negative evidence.

#### Step 16: Resource area reviewed

![PE resource area and manifest indicator](Screenshots/image18.png)

- **Action performed:** The resource panel was examined for icons, manifests, version information, or embedded binaries.
- **Visible evidence:** A Manifest item appears available, while a broader extractable resource tree is not shown.
- **Importance:** The evidence supports limited resource content but does not demonstrate an icon, version block, or embedded secondary binary.
- **Competency demonstrated:** PE resource analysis and disciplined limitation reporting.

#### Step 17: TLS callbacks detected

![Two TLS callbacks in the text section](Screenshots/image19.png)

- **Action performed:** The PE TLS structure was inspected.
- **Visible evidence:** Two callback entries are present, both located in `Section(0) [.text]`.
- **Importance:** TLS callbacks can execute before the declared program entry point. They may support runtime setup or analysis-evasion logic and are among the strongest suspicious structural indicators observed here.
- **Competency demonstrated:** Advanced PE analysis, execution-order reasoning, and anti-analysis identification.

### 6. API-Centric Classification

#### Step 18: MalAPI taxonomy used as a qualitative reference

![MalAPI category taxonomy](Screenshots/image20.png)

- **Action performed:** MalAPI category groupings were used to classify the significance of the APIs visible in the sample's IAT.
- **Visible evidence:** Categories such as Injection, Evasion, Spying, Internet, Anti-Debugging, Ransomware, and Helper are displayed.
- **Importance:** The taxonomy provides a structured reference for grouping observed imports. It is not treated as proof that the sample imports every API displayed on the MalAPI page.
- **Competency demonstrated:** API categorisation, evidence correlation, and avoidance of over-attribution.

The observed distribution is strongest in helper/runtime and evasion-adjacent behaviour. No captured evidence supports persistence-specific or network-specific API activity.

## Behavioural Reconstruction

### Probable Static Execution Sequence

The most defensible sequence supported by the evidence is:

```text
TLS callback execution
        ↓
Runtime and environment initialisation
        ↓
LoadLibraryA and GetProcAddress resolve additional functionality
        ↓
VirtualAlloc and VirtualProtect prepare or modify memory regions
        ↓
Control transfers toward the declared entry point or a subsequent stage
```

This sequence supports a loader/stager hypothesis. It does not establish persistence, network communication, or remote process injection.

### Behavioural Capability Assessment

| Behaviour | Assessment | Evidence basis |
|---|---|---|
| Loader or stager activity | Moderate confidence | TLS callbacks, dynamic resolution, memory preparation APIs, overlay data |
| Anti-analysis behaviour | Moderate confidence | TLS callbacks, timing APIs, exception handling, unknown overlay |
| Confirmed packing | Not established | No entropy graph or unpacker signature captured |
| Classic remote process injection | Not evidenced | No visible `VirtualAllocEx`, `WriteProcessMemory`, or `CreateRemoteThread` imports |
| Persistence | Not evidenced | No visible registry autorun, service creation, scheduled task, COM, or WMI artefacts |
| Privilege escalation | Not evidenced | No visible token manipulation, UAC bypass, service abuse, or exploit indicators |
| Network communication | Not evidenced | No visible WinINet, WinHTTP, socket, DNS, URL, IP, or domain artefacts |
| Data exfiltration | Not evidenced | No supported transport, archive, credential, or outbound communication evidence |
| Vulnerability exploitation | Not evidenced | No visible exploit scaffolding or exploit-oriented API sequence |

## Findings

### Primary Findings

1. **The sample is a 32-bit Nim-compiled Windows PE.**
   - It is configured as a GUI executable for I386 systems.
   - The declared entry point is `0x004014A0`.

2. **The specimen contains non-standard structural characteristics.**
   - It has fifteen sections.
   - An unknown overlay exists at offset `0x00051400`, size `0xE733`.
   - Two TLS callbacks are located in `.text`.

3. **The import set is compact but behaviourally meaningful.**
   - `LoadLibraryA` and `GetProcAddress` support dynamic API resolution.
   - `VirtualAlloc` and `VirtualProtect` support memory-region preparation or modification.
   - `Sleep`, `GetTickCount`, and `QueryPerformanceCounter` have timing and evasion relevance.
   - `SetUnhandledExceptionFilter` and `UnhandledExceptionFilter` may influence exception-driven control flow.

4. **The strongest supported classification is loader/stager.**
   - Early TLS execution, dynamic resolution, memory handling, and overlay data collectively support staged local initialisation or payload handoff.

5. **Several common malware capabilities are not evidenced.**
   - The captured data does not support persistence, privilege escalation, classic remote injection, direct networking, C2 reconstruction, COM/WMI activity, scheduled tasks, data exfiltration, or vulnerability exploitation.

### Indicators and Metadata

| Category | Observation |
|---|---|
| Filename | `Malware.stage0.exe.malz` |
| MD5 | `6D8895C63A77EBE5E49B656BDEFDB822` |
| SHA256 | `FCA62097B364B2F0338C5E4C5BAC86134CEDFFA4F8DDF27EE9901734128952E3` |
| Format | PE32 executable |
| Architecture | I386 / 32-bit x86 |
| Subsystem | Windows GUI |
| Compiler | Nim |
| Image base | `0x00400000` |
| Image size | `0x00065000` |
| Entry point | `0x004014A0` |
| Compilation timestamp | `2021-10-07 10:43:04` |
| Sections | 15 |
| Overlay | Offset `0x00051400`, size `0xE733` |
| TLS callbacks | Two, both in `.text` |
| Visible imported DLLs | `KERNEL32.dll`, `msvcrt.dll`, `USER32.dll` |

## MITRE ATT&CK Mapping

The following mappings are intentionally conservative because they are based on static evidence only.

| Technique | Name | Confidence | Supporting observation |
|---|---|---:|---|
| `T1027` | Obfuscated/Compressed Files and Information | Moderate | Unknown overlay and structurally suspicious appended content |
| `T1622` | Debugger Evasion | Low to moderate | Exception-handling APIs and pre-entry-point TLS callback execution |
| `T1497.003` | Time Based Evasion | Low | `Sleep`, `GetTickCount`, `QueryPerformanceCounter`, and `GetSystemTimeAsFileTime` considered in combination |

No ATT&CK mapping is asserted for persistence, credential access, privilege escalation, exfiltration, or command and control because the supplied evidence does not support those behaviours.

## Challenges Encountered

- A full entropy graph was not present, preventing a definitive packing assessment.
- The IMPHASH was not explicitly calculated in the visible evidence.
- The captured strings excerpt did not display IP addresses, domains, URLs, or hard-coded paths.
- A complete section-permission table was unavailable, so explicit RWX-section confirmation could not be made.
- The resource evidence did not show a complete resource tree or extracted embedded objects.
- The MalAPI screenshot provided a category taxonomy rather than a sample-specific scorecard or heatmap.
- Static evidence alone could not confirm whether suspicious APIs were actually called at runtime.
- No dynamic network, process, registry, memory, or filesystem telemetry was available for validation.

These limitations were treated as boundaries on the conclusions rather than filled with assumptions.

## Recommendations

1. **Preserve the sample identity.** Use the verified SHA256 as the primary identifier in future authorised analysis and evidence records.
2. **Retain laboratory isolation.** Any further examination should remain inside a controlled malware-analysis environment with appropriate snapshots and host protections.
3. **Generate a full entropy profile.** Review entropy per section and across the overlay to assess packing or embedded-stage likelihood more reliably.
4. **Calculate and record the IMPHASH.** This would support import-set clustering and comparison with related samples.
5. **Perform deeper strings analysis.** Review ASCII and Unicode output comprehensively and inspect the overlay separately for encoded configuration or infrastructure artefacts.
6. **Inspect section characteristics.** Validate section sizes, permissions, virtual-to-raw relationships, and any writable-executable regions.
7. **Disassemble the TLS callbacks first.** Determine what executes before the declared entry point and whether the callbacks perform environment checks, decoding, or control-flow diversion.
8. **Trace dynamic API resolution.** Identify the libraries and functions resolved through `LoadLibraryA` and `GetProcAddress`.
9. **Examine memory-preparation logic.** Determine what data is copied into regions created or modified through `VirtualAlloc` and `VirtualProtect`.
10. **Inspect the overlay.** Establish whether the appended data contains configuration, another executable stage, encrypted content, or unrelated data.
11. **Use authorised dynamic analysis for validation.** If permitted, observe process, memory, filesystem, registry, and network behaviour to test the static hypotheses.
12. **Maintain confidence labels.** Continue distinguishing directly observed evidence from inferred or unconfirmed behaviour in all future reporting.

## Key Learning Outcomes

- File hashes should be calculated and independently validated before deeper analysis.
- PE metadata can reveal the architecture, compiler, loading characteristics, and useful reverse-engineering landmarks.
- TLS callbacks may change the true execution order by running before the declared entry point.
- APIs must be interpreted as clusters and in structural context, not labelled malicious in isolation.
- Overlay data is an investigative lead, not automatic proof of packing or an embedded payload.
- Negative evidence matters: the absence of visible networking, persistence, injection, or privilege APIs limits defensible claims.
- Static analysis supports behavioural hypotheses but does not prove that a code path executed.
- Professional malware reporting requires explicit limitations, reproducible identifiers, and confidence-based conclusions.

## Conclusion

The static analysis identifies `Malware.stage0.exe.malz` as a 32-bit Nim-compiled Windows PE with a declared entry point at `0x004014A0`, fifteen sections, an unknown appended overlay, and two TLS callbacks in `.text`. Its compact import table combines dynamic resolution, memory management, timing, and exception-handling functionality.

Taken together, these characteristics are most consistent with a **loader or stager with moderate anti-analysis relevance**. The evidence supports a probable sequence involving pre-entry-point callback execution, runtime initialisation, dynamic function resolution, memory preparation, and a possible handoff to another stage. It does not support a conclusion that the specimen performs persistence, privilege escalation, classic remote process injection, direct network communication, data exfiltration, or vulnerability exploitation.

The project demonstrates a structured and defensible static-analysis process in which every conclusion is tied to captured evidence and every limitation is reported rather than concealed by speculation.

## Responsible AI Use Declaration

AI assistance was used to restructure the supplied laboratory report into a GitHub portfolio format and improve clarity, organisation, and Markdown presentation. The technical observations, screenshots, hashes, findings, limitations, and conclusions were derived only from the supplied report and its embedded evidence. No additional malware behaviour, command, timestamp, indicator, result, or conclusion was fabricated. The portfolio owner remains responsible for reviewing the final content against the original evidence before publication.

## Disclaimer

This project is provided for authorised educational, defensive security, and professional portfolio purposes. It does not include instructions for deploying malware or conducting unauthorised activity. The findings reflect a limited static examination of the supplied sample and should not be treated as proof of runtime behaviour without controlled dynamic validation.

## Acknowledgements

- International Cybersecurity and Digital Forensics Academy
- Course: ACI803 Malware Analysis for Cybercrime
- Instructor: Mr. Aminu Idris, AMCPN
- Analyst: Clement Modebe

## Attribution and Evidence Integrity

All technical content in this README is based on the supplied laboratory report and its embedded screenshots. Screenshot chronology has been preserved exactly. No screenshot has been omitted, rearranged, combined, or used more than once. Where a requirement was not fully evidenced, the limitation has been stated explicitly.

