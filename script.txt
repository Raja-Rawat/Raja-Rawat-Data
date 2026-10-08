/**
 * RAJA RAWAT — INFORMATICA ETL ADMINISTRATOR PORTFOLIO
 * Fit-To-Browser Window + Cinematic Warp + Incident Self-Healing + Platform Patching
 */

document.addEventListener('DOMContentLoaded', () => {

  // =========================================================================
  // 1. PROCEDURAL WEB AUDIO SYNTHESIZER
  // =========================================================================
  class SoundEngine {
    constructor() {
      this.ctx = null;
      this.muted = false;
    }

    init() {
      if (!this.ctx && (window.AudioContext || window.webkitAudioContext)) {
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        this.ctx = new AudioCtx();
      }
      if (this.ctx && this.ctx.state === 'suspended') {
        this.ctx.resume();
      }
    }

    playClick() {
      if (this.muted) return;
      this.init();
      if (!this.ctx) return;
      try {
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'sine';
        osc.frequency.setValueAtTime(600, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(250, this.ctx.currentTime + 0.04);
        gain.gain.setValueAtTime(0.08, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.04);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start();
        osc.stop(this.ctx.currentTime + 0.04);
      } catch (e) {}
    }

    playWarpSound() {
      if (this.muted) return;
      this.init();
      if (!this.ctx) return;
      try {
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(140, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(880, this.ctx.currentTime + 0.5);
        osc.frequency.exponentialRampToValueAtTime(220, this.ctx.currentTime + 1.0);

        gain.gain.setValueAtTime(0.001, this.ctx.currentTime);
        gain.gain.linearRampToValueAtTime(0.09, this.ctx.currentTime + 0.4);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 1.0);

        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start();
        osc.stop(this.ctx.currentTime + 1.0);
      } catch (e) {}
    }

    playAlarmSound() {
      if (this.muted) return;
      this.init();
      if (!this.ctx) return;
      try {
        // Double siren pulse
        [0, 0.2, 0.4].forEach((delay) => {
          const osc = this.ctx.createOscillator();
          const gain = this.ctx.createGain();
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(850, this.ctx.currentTime + delay);
          osc.frequency.linearRampToValueAtTime(450, this.ctx.currentTime + delay + 0.15);
          gain.gain.setValueAtTime(0.12, this.ctx.currentTime + delay);
          gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + delay + 0.15);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(this.ctx.currentTime + delay);
          osc.stop(this.ctx.currentTime + delay + 0.15);
        });
      } catch (e) {}
    }

    playCommitChime() {
      if (this.muted) return;
      this.init();
      if (!this.ctx) return;
      try {
        const freqs = [523.25, 659.25, 783.99, 1046.50]; // C5, E5, G5, C6
        freqs.forEach((freq, idx) => {
          const osc = this.ctx.createOscillator();
          const gain = this.ctx.createGain();
          osc.type = 'sine';
          osc.frequency.setValueAtTime(freq, this.ctx.currentTime + idx * 0.08);
          gain.gain.setValueAtTime(0.12, this.ctx.currentTime + idx * 0.08);
          gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + idx * 0.08 + 0.35);
          osc.connect(gain);
          gain.connect(this.ctx.destination);
          osc.start(this.ctx.currentTime + idx * 0.08);
          osc.stop(this.ctx.currentTime + idx * 0.08 + 0.35);
        });
      } catch (e) {}
    }

    toggleMute() {
      this.muted = !this.muted;
      return this.muted;
    }
  }

  const sound = new SoundEngine();

  const audioToggleBtn = document.getElementById('audioToggleBtn');
  const audioIcon = document.getElementById('audioIcon');
  audioToggleBtn.addEventListener('click', () => {
    const isMuted = sound.toggleMute();
    audioIcon.textContent = isMuted ? '🔇' : '🔊';
    if (!isMuted) sound.playClick();
  });

  // =========================================================================
  // 2. SPEEDING DATA STREAM GENERATOR FOR WARP SCREEN
  // =========================================================================
  const warpStreamsData = [
    `>>> DTM_BUFFER_ALLOC: 64MB >>> SELECT CANDIDATE_ID, NAME, SKILLS FROM SRC_RAW >>> HOST: infaprod01.horizonblue.com >>> THREAD_ID: 0x7FFE9B >>> DTM_BUFFER_ALLOC: 64MB >>> SELECT CANDIDATE_ID, NAME, SKILLS FROM SRC_RAW >>>`,
    `[INFORMATICA_POWERCENTER_10_5] >>> RS_PROD_META ONLINE >>> REPO: ORACLE_19C_RAC >>> IS_GRID_01 (2 NODES BALANCED) >>> [INFORMATICA_POWERCENTER_10_5] >>> RS_PROD_META ONLINE >>>`,
    `>>> TRANSFORM: LTRIM(RTRIM(SUBSTR(RAW_PAYLOAD, 1, 4096))) >>> HIGH_AVAILABILITY: 99.9% >>> MTTR_REDUCTION: -35% >>> TRANSFORM: LTRIM(RTRIM(SUBSTR(RAW_PAYLOAD, 1, 4096))) >>>`,
    `PORT_ROUTER: GRP_INFORMATICA, GRP_AUTOMATION, GRP_CLOUD, GRP_DATABASES, GRP_GOVERNANCE, GRP_AI >>> ROW_COUNT: 18 >>> PORT_ROUTER: GRP_INFORMATICA, GRP_AUTOMATION >>>`,
    `>>> JOINER_KEY: CTS_PROD_2022 ⨝ HORIZON_BLUE_NJ_US_HEALTHCARE >>> ENCRYPTION: CYBERARK_PAM_AES256 >>> JOINER_KEY: CTS_PROD_2022 ⨝ HORIZON_BLUE_NJ >>>`,
    `LOOKUP_SQL: SELECT BLUEPRINT, ROI FROM ARCHITECTURE_CATALOG WHERE STATUS = 'PRODUCTION_GRADE' >>> SECURE_AGENT_CLUSTER >>> LOOKUP_SQL: SELECT BLUEPRINT, ROI >>>`,
    `>>> FILTER_VALIDATOR: (CERT_ACCREDITED == TRUE) AND (GPA >= 8.0) >>> VERIFIED_RECORDS: 6 CERTS + B.TECH DEGREE >>> FILTER_VALIDATOR: (CERT_ACCREDITED == TRUE) >>>`,
    `TARGET_INSERT: WRT_8036 BULK LOAD COMMIT >>> 14,250 ROWS/SEC >>> DATA PIPELINE LATENCY: 0.08ms >>> SUCCESS >>> TARGET_INSERT: WRT_8036 BULK LOAD COMMIT >>>`
  ];

  const warpOverlay = document.getElementById('dataWarpOverlay');
  const warpHudTag = document.getElementById('warpHudTag');
  const warpHudTarget = document.getElementById('warpHudTarget');

  document.querySelectorAll('.warp-stream').forEach((streamEl, idx) => {
    const text = warpStreamsData[idx % warpStreamsData.length];
    streamEl.innerHTML = `<span>${text}</span><span>${text}</span>`;
  });

  // =========================================================================
  // 3. PIPELINE STEP STATE & WARP TRANSITION ENGINE (8 STAGES)
  // =========================================================================
  const slidesTrack = document.getElementById('slidesTrack');
  const trackFill = document.getElementById('trackFill');
  const packetGlider = document.getElementById('packetGlider');
  const stripLatestMsg = document.getElementById('stripLatestMsg');
  const stripThroughput = document.getElementById('stripThroughput');

  const topGridPulse = document.getElementById('topGridPulse');
  const agentStatusBadge = document.getElementById('agentStatusBadge');
  const slaDisplay = document.getElementById('slaDisplay');
  const statusBarDot = document.getElementById('statusBarDot');

  const totalSteps = 8;
  let currentStep = 0;
  let isWarping = false;
  let autoPlayActive = false;
  let autoPlayTimer = null;

  // Track if the 2nd transformation failure has been triggered & resolved
  let hasIncidentTriggered = false;
  let hasIncidentResolved = false;

  const stageMeta = [
    {
      step: 0,
      hudTag: 'SOURCE FILE INGESTION',
      hudTarget: 'STAGING RAW CANDIDATE PAYLOAD',
      log: 'Raw flatfile stream ready. Awaiting pipeline ingestion trigger...',
      throughput: '0 rows/s'
    },
    {
      step: 1,
      hudTag: 'TRANSFORMATION [SQ_PROFILE]',
      hudTarget: 'PARSING RAW IDENTITY & PLATFORM METRICS',
      log: 'SQ_PROFILE evaluated: Extracted Raja Rawat, Informatica Admin (3+ Yrs, 99.9% SLA).',
      throughput: '2,840 rows/s'
    },
    {
      step: 2,
      hudTag: 'TRANSFORMATION [RTR_SKILLS]',
      hudTarget: 'ROUTING 18 SKILLS INTO 6 DEDICATED DOMAIN GROUPS',
      log: 'RTR_SKILLS evaluated router conditions: Dispatched skills into 6 domain groups.',
      throughput: '5,120 rows/s'
    },
    {
      step: 3,
      hudTag: 'TRANSFORMATION [JNR_CAREER]',
      hudTarget: 'JOINING COGNIZANT & HORIZON BLUE NJ PRODUCTION GRIDS',
      log: 'JNR_CAREER executed inner join: Cognizant ⨝ Horizon Blue NJ (US Healthcare).',
      throughput: '4,960 rows/s'
    },
    {
      step: 4,
      hudTag: 'TRANSFORMATION [EXP_PROJECTS]',
      hudTarget: 'RESOLVING AUTOMATION, CLOUD & HA ARCHITECTURES',
      log: 'EXP_PROJECTS resolved 3 architecture blueprints: Self-Healing, Cloud Modernization & Grid DR.',
      throughput: '6,450 rows/s'
    },
    {
      step: 5,
      hudTag: 'TRANSFORMATION [FLT_CERTS]',
      hudTarget: 'FILTERING & ACCREDITING 6 ENTERPRISE CERTIFICATIONS',
      log: 'FLT_CERTS validated 6 enterprise certifications (IDMC, Databricks, CLAIRE) & B.Tech degree.',
      throughput: '7,800 rows/s'
    },
    {
      step: 6,
      hudTag: 'PLATFORM MAINTENANCE [MAINT_PATCH]',
      hudTarget: 'INFORMATICA EBF HOTFIX & CLOUD SECURE AGENT UPGRADE',
      log: 'MAINT_PATCH: Zero-downtime rolling update applied. EBF-24892 & Secure Agent v64.2 online.',
      throughput: '9,200 rows/s'
    },
    {
      step: 7,
      hudTag: 'TARGET DWH COMMIT [TGT_RESUME]',
      hudTarget: 'WRITING UNIFIED RECORD TO PRODUCTION DATA WAREHOUSE',
      log: 'WRT_8036: Target Load committed. Full resume assembled. Workflow SUCCEEDED (0 errors).',
      throughput: '14,250 rows/s'
    }
  ];

  function transitionToStep(targetStep, playWarp = true) {
    if (targetStep < 0 || targetStep >= totalSteps) return;
    if (isWarping) return;

    // CHECK FOR 2ND TRANSFORMATION FAILURE (STEP 2: RTR_SKILLS)
    if (targetStep === 2 && !hasIncidentResolved) {
      triggerAgentFailureIncident();
      return;
    }

    const meta = stageMeta[targetStep];
    currentStep = targetStep;

    if (playWarp) {
      isWarping = true;
      sound.init();
      sound.playWarpSound();

      warpHudTag.textContent = meta.hudTag;
      warpHudTarget.textContent = meta.hudTarget;
      warpOverlay.classList.add('active');

      setTimeout(() => {
        applySlidePosition(currentStep);
      }, 400);

      setTimeout(() => {
        warpOverlay.classList.remove('active');
        isWarping = false;

        if (currentStep === 7) {
          sound.playCommitChime();
        }

        if (autoPlayActive && currentStep < totalSteps - 1) {
          clearTimeout(autoPlayTimer);
          autoPlayTimer = setTimeout(() => {
            transitionToStep(currentStep + 1, true);
          }, 4500);
        }
      }, 1050);

    } else {
      applySlidePosition(currentStep);
    }
  }

  function applySlidePosition(step) {
    const meta = stageMeta[step];

    const slideOffset = step * (100 / totalSteps);
    slidesTrack.style.transform = `translateX(-${slideOffset}%)`;

    const progressPct = (step / (totalSteps - 1)) * 100;
    trackFill.style.width = `${progressPct}%`;
    packetGlider.style.left = `${progressPct}%`;

    for (let i = 0; i < totalSteps; i++) {
      const pill = document.getElementById(`stepPill_${i}`);
      if (pill) {
        pill.classList.remove('active', 'completed');
        if (i < step) {
          pill.classList.add('completed');
        } else if (i === step) {
          pill.classList.add('active');
        }
      }
    }

    stripLatestMsg.textContent = meta.log;
    stripThroughput.textContent = meta.throughput;

    // Auto-scroll track bar on mobile so active node pill is centered
    const activePill = document.getElementById(`stepPill_${step}`);
    if (activePill && typeof activePill.scrollIntoView === 'function') {
      activePill.scrollIntoView({ behavior: 'smooth', inline: 'center', block: 'nearest' });
    }
  }

  // =========================================================================
  // 4. CRITICAL AGENT FAILURE & ADMIN SELF-HEALING (STEP 2 FAILURE)
  // =========================================================================
  const agentFailureModal = document.getElementById('agentFailureModal');
  const fixAgentBtn = document.getElementById('fixAgentBtn');
  const selfHealingConsole = document.getElementById('selfHealingConsole');

  function triggerAgentFailureIncident() {
    hasIncidentTriggered = true;
    sound.init();
    sound.playAlarmSound();

    // Visual indicators turn red
    topGridPulse.classList.add('down');
    statusBarDot.classList.add('down');
    agentStatusBadge.textContent = 'Agent: DOWN (Deadlock)';
    agentStatusBadge.className = 'agent-down';
    slaDisplay.textContent = '94.2% (Degraded)';
    slaDisplay.className = 'sla-val text-orange';

    stripLatestMsg.textContent = 'CRITICAL ALERT: [INFA_AGENT_504] Secure Agent SA_AWS_US_EAST_01 DOWN. Deadlock on node NODE01_CHENNAI.';
    stripThroughput.textContent = '0 rows/s (HALTED)';

    // Show Failure Modal
    agentFailureModal.classList.add('active');
    agentFailureModal.setAttribute('aria-hidden', 'false');

    // Pause auto-run if active
    if (autoPlayActive) {
      autoPlayActive = false;
      autoPlayIcon.textContent = '▶';
      autoPlayLabel.textContent = 'Auto-Run';
      autoPlayBtn.classList.remove('primary');
      clearTimeout(autoPlayTimer);
    }
  }

  fixAgentBtn.addEventListener('click', () => {
    sound.playClick();
    fixAgentBtn.disabled = true;
    fixAgentBtn.innerHTML = `<span>⏳ Running Tier-3 RCA &amp; Self-Healing Script...</span>`;
    selfHealingConsole.classList.remove('hidden');

    // Simulate rapid command execution
    setTimeout(() => {
      sound.playClick();
      stripLatestMsg.textContent = '[REMEDIATING] Captured pmstack thread trace. Terminating hung process PID 14288...';
    }, 600);

    setTimeout(() => {
      sound.playClick();
      stripLatestMsg.textContent = '[REMEDIATING] Rotating CyberArk PAM credentials for ORCL_19C...';
    }, 1200);

    setTimeout(() => {
      sound.playCommitChime();
      stripLatestMsg.textContent = '[SUCCESS] Secure Agent SA_AWS_US_EAST_01 reconnected. Grid High Availability restored!';
      
      // Restore indicators to healthy emerald
      topGridPulse.classList.remove('down');
      statusBarDot.classList.remove('down');
      agentStatusBadge.textContent = 'Agent: ONLINE';
      agentStatusBadge.className = 'agent-live';
      slaDisplay.textContent = '99.9% Uptime';
      slaDisplay.className = 'sla-val text-emerald';

      hasIncidentResolved = true;

      // Close modal and rerun failed step 2!
      setTimeout(() => {
        agentFailureModal.classList.remove('active');
        agentFailureModal.setAttribute('aria-hidden', 'true');
        
        // Rerun ETL from failed step 2 with speeding data warp!
        transitionToStep(2, true);
      }, 1500);

    }, 2200);
  });

  // =========================================================================
  // 5. STAGE 6: PLATFORM MAINTENANCE & EBF PATCHING
  // =========================================================================
  const commitPatchAndRunBtn = document.getElementById('commitPatchAndRunBtn');
  const patchItemAgent = document.getElementById('patchItemAgent');
  const patchSpinnerAgent = document.getElementById('patchSpinnerAgent');
  const patchStatusAgent = document.getElementById('patchStatusAgent');
  const patchBarAgent = document.getElementById('patchBarAgent');

  commitPatchAndRunBtn.addEventListener('click', () => {
    sound.playClick();
    
    // Mark second patch as completed
    if (patchItemAgent) {
      patchItemAgent.classList.remove('in-progress');
      patchItemAgent.classList.add('completed');
      if (patchSpinnerAgent) patchSpinnerAgent.outerHTML = '<span class="p-check">✓</span>';
      if (patchStatusAgent) patchStatusAgent.textContent = 'APPLIED';
      if (patchBarAgent) {
        patchBarAgent.classList.remove('fill-anim');
        patchBarAgent.classList.add('fill-100');
      }
    }

    stripLatestMsg.textContent = 'Platform maintenance committed. Triggering final target warehouse data warp...';

    // Rerun to final Stage 7 (Target DWH)
    setTimeout(() => {
      transitionToStep(7, true);
    }, 400);
  });

  // =========================================================================
  // 6. NAVIGATION BUTTONS & STEPPERS
  // =========================================================================
  for (let i = 0; i < totalSteps; i++) {
    const pill = document.getElementById(`stepPill_${i}`);
    if (pill) {
      pill.addEventListener('click', () => {
        transitionToStep(i, true);
      });
    }
  }

  document.querySelectorAll('.next-stage').forEach(btn => {
    btn.addEventListener('click', () => {
      const dest = parseInt(btn.getAttribute('data-dest'), 10);
      transitionToStep(dest, true);
    });
  });

  document.querySelectorAll('.prev-stage').forEach(btn => {
    btn.addEventListener('click', () => {
      const dest = parseInt(btn.getAttribute('data-dest'), 10);
      transitionToStep(dest, true);
    });
  });

  // Skip to Target Button
  document.getElementById('skipToTargetBtn').addEventListener('click', () => {
    hasIncidentResolved = true; // bypass failure if user wants quick jump
    transitionToStep(7, true);
  });

  // Re-run Flow Button
  document.getElementById('reRunFlowBtn').addEventListener('click', () => {
    const docCard = document.getElementById('draggableSourceDoc');
    docCard.classList.remove('consumed');
    hasIncidentTriggered = false;
    hasIncidentResolved = false; // Reset failure so user can experience it again!
    fixAgentBtn.disabled = false;
    fixAgentBtn.innerHTML = `<span>🛠️ Execute Admin Self-Healing &amp; Restart Agent</span>`;
    selfHealingConsole.classList.add('hidden');
    transitionToStep(0, false);
  });

  // Auto-Play Toggle
  const autoPlayBtn = document.getElementById('autoPlayBtn');
  const autoPlayIcon = document.getElementById('autoPlayIcon');
  const autoPlayLabel = document.getElementById('autoPlayLabel');

  autoPlayBtn.addEventListener('click', () => {
    autoPlayActive = !autoPlayActive;
    sound.playClick();
    if (autoPlayActive) {
      autoPlayIcon.textContent = '⏸';
      autoPlayLabel.textContent = 'Pause';
      autoPlayBtn.classList.add('primary');
      if (currentStep < totalSteps - 1) {
        transitionToStep(currentStep === 0 ? 1 : currentStep, true);
      }
    } else {
      autoPlayIcon.textContent = '▶';
      autoPlayLabel.textContent = 'Auto-Run';
      autoPlayBtn.classList.remove('primary');
      clearTimeout(autoPlayTimer);
    }
  });

  // =========================================================================
  // 7. SOURCE FILE DRAG & DROP INGESTION (STAGE 0)
  // =========================================================================
  const draggableSourceDoc = document.getElementById('draggableSourceDoc');
  const pipelineDropZone = document.getElementById('pipelineDropZone');
  const directRunBtn = document.getElementById('directRunBtn');

  function triggerIngestion() {
    sound.init();
    sound.playClick();
    draggableSourceDoc.classList.add('consumed');
    pipelineDropZone.classList.add('processing');

    stripLatestMsg.textContent = 'Ingesting Raja_Rawat_Raw_Data.json. Allocating 64MB DTM buffer pool...';

    setTimeout(() => {
      pipelineDropZone.classList.remove('processing');
      transitionToStep(1, true); // Launch speeding data warp!
    }, 300);
  }

  draggableSourceDoc.addEventListener('dragstart', (e) => {
    e.dataTransfer.setData('text/plain', 'source_doc');
    draggableSourceDoc.classList.add('dragging');
    sound.playClick();
  });

  draggableSourceDoc.addEventListener('dragend', () => {
    draggableSourceDoc.classList.remove('dragging');
  });

  pipelineDropZone.addEventListener('dragover', (e) => {
    e.preventDefault();
    pipelineDropZone.classList.add('dragover');
  });

  pipelineDropZone.addEventListener('dragleave', () => {
    pipelineDropZone.classList.remove('dragover');
  });

  pipelineDropZone.addEventListener('drop', (e) => {
    e.preventDefault();
    pipelineDropZone.classList.remove('dragover');
    triggerIngestion();
  });

  draggableSourceDoc.addEventListener('click', triggerIngestion);
  directRunBtn.addEventListener('click', triggerIngestion);

  // =========================================================================
  // 8. STAGE 7: TARGET PANEL TAB CONTROLLER
  // =========================================================================
  const tabResumeBtn = document.getElementById('tabResumeBtn');
  const tabCliBtn = document.getElementById('tabCliBtn');
  const tabDispatchBtn = document.getElementById('tabDispatchBtn');

  const panelResume = document.getElementById('panelResume');
  const panelCli = document.getElementById('panelCli');
  const panelDispatch = document.getElementById('panelDispatch');

  function switchTargetTab(activeBtn, activePanel) {
    sound.playClick();
    [tabResumeBtn, tabCliBtn, tabDispatchBtn].forEach(b => b.classList.remove('active'));
    [panelResume, panelCli, panelDispatch].forEach(p => p.classList.remove('active'));

    activeBtn.classList.add('active');
    activePanel.classList.add('active');
  }

  tabResumeBtn.addEventListener('click', () => switchTargetTab(tabResumeBtn, panelResume));
  tabCliBtn.addEventListener('click', () => switchTargetTab(tabCliBtn, panelCli));
  tabDispatchBtn.addEventListener('click', () => switchTargetTab(tabDispatchBtn, panelDispatch));

  // Print Resume
  const printResumeBtn = document.getElementById('printResumeBtn');
  const resumeIframe = document.getElementById('resumeIframe');

  printResumeBtn.addEventListener('click', () => {
    sound.playClick();
    try {
      if (resumeIframe && resumeIframe.contentWindow) {
        resumeIframe.contentWindow.print();
      } else {
        window.open('resume.html', '_blank');
      }
    } catch (e) {
      window.open('resume.html', '_blank');
    }
  });

  // =========================================================================
  // 9. INFORMATICA CLI INTERPRETER
  // =========================================================================
  const cliScreen = document.getElementById('cliScreen');
  const cliInputForm = document.getElementById('cliInputForm');
  const cliInputField = document.getElementById('cliInputField');
  const termChips = document.querySelectorAll('.t-chip');

  const cliResponses = {
    'help': `Informatica Commands Available:
  • pmcmd getworkflowdetails   - Active workflow status, throughput & DTM buffers
  • infacmd ping               - Ping domain, master node, and repository services
  • cat skills                 - Print full 6-domain technical competencies
  • whoami                     - Display administrator credentials & authorization
  • clear                      - Clear terminal output`,

    'pmcmd getworkflowdetails': `Workflow: [wf_INGEST_RAJA_RAWAT_PROFILE]
Session: [s_m_EXTRACT_TRANSFORM_LOAD_PORTFOLIO]
Status: [SUCCEEDED] | Integration Service: [IS_GRID_01]
Domain: [DOM_PROD_CHENNAI] | Node: [NODE01_CHENNAI]
Total Rows Read: 1 | Written: 1 | Rejected: 0
Buffer Pool: 64 MB (Optimal) | Execution: ${new Date().toLocaleTimeString()}`,

    'infacmd ping': `[ICMD_10033] Command [ping] succeeded:
------------------------------------------------------------------
Domain Service: [DOM_PROD_CHENNAI]       --> ACTIVE (100% SLA)
Node Name:      [NODE01_CHENNAI]          --> ONLINE (Master Grid)
Service:        [RS_PROD_META] (Repo)     --> RUNNING (Oracle 19c RAC)
Service:        [IS_GRID_01]   (Integ)    --> RUNNING (2 Nodes Active)
Secure Agent:   [SA_AWS_US_EAST_01]       --> CONNECTED (IICS/IDMC)`,

    'cat skills': `=== RAJA RAWAT: TECHNICAL COMPETENCIES ===
• Informatica & Cloud: PowerCenter 10.x, CDIPC, IICS/IDMC, Secure Agent Clustering
• Automation & Tools: Python, Unix Shell (Bash/ksh), Jenkins, ServiceNow, CyberArk PAM
• Cloud & OS: Linux (RHEL, CentOS), AIX, Windows Server, AWS EC2, Databricks
• Databases: Oracle 12c/19c RAC, Teradata, MySQL, SQL Server, PL/SQL, ODBC
• Governance: HA/DR, DTM Buffer Tuning, Tier-3 RCA, LDAP/AD, EBFs & Hotfixes
• AI Productivity: Informatica CLAIRE AI, GitHub Copilot`,

    'whoami': `User:        rajarawat
Role:        Informatica Administrator / Associate Systems Engineer
Organization: Cognizant Technology Solutions
Client:      Horizon Blue NJ (US Healthcare)
Location:    Chennai, Tamil Nadu, India
Clearance:   Production 24x7 Tier-3 On-Call & Grid Admin Authorized`
  };

  function executeCliCommand(rawCmd) {
    const cmd = rawCmd.trim();
    if (!cmd) return;
    sound.playClick();

    const cmdRow = document.createElement('div');
    cmdRow.className = 'term-line cmd';
    cmdRow.textContent = `[rajarawat@infaprod01]$ ${cmd}`;
    cliScreen.appendChild(cmdRow);

    const lower = cmd.toLowerCase();
    if (lower === 'clear') {
      cliScreen.innerHTML = '';
    } else if (cliResponses[lower]) {
      const respRow = document.createElement('div');
      respRow.className = 'term-line resp';
      respRow.textContent = cliResponses[lower];
      cliScreen.appendChild(respRow);
    } else {
      let matched = false;
      for (const k of Object.keys(cliResponses)) {
        if (lower.includes(k)) {
          const respRow = document.createElement('div');
          respRow.className = 'term-line resp';
          respRow.textContent = cliResponses[k];
          cliScreen.appendChild(respRow);
          matched = true;
          break;
        }
      }
      if (!matched) {
        const errRow = document.createElement('div');
        errRow.className = 'term-line err';
        errRow.textContent = `command not found: "${cmd}". Type "help" for a list of available commands.`;
        cliScreen.appendChild(errRow);
      }
    }

    cliScreen.scrollTop = cliScreen.scrollHeight;
    cliInputField.value = '';
  }

  cliInputForm.addEventListener('submit', (e) => {
    e.preventDefault();
    executeCliCommand(cliInputField.value);
  });

  termChips.forEach(chip => {
    chip.addEventListener('click', () => {
      executeCliCommand(chip.getAttribute('data-cmd'));
    });
  });

  // =========================================================================
  // 10. SERVICENOW TICKET DISPATCH FORM
  // =========================================================================
  const contactForm = document.getElementById('contactForm');
  contactForm.addEventListener('submit', (e) => {
    e.preventDefault();
    sound.playCommitChime();

    const name = document.getElementById('senderName').value;
    const email = document.getElementById('senderEmail').value;
    const cat = document.getElementById('opportunityType').value;
    const prio = document.getElementById('ticketPriority').value;
    const msg = document.getElementById('messageBody').value;
    const tId = document.getElementById('snowTicketId').textContent;

    const subject = encodeURIComponent(`[${tId}] [${prio}] Inquiry from ${name}: ${cat}`);
    const body = encodeURIComponent(`ServiceNow Incident Reference: ${tId}
Contact Name: ${name}
Contact Email: ${email}
Category: ${cat}
Priority: ${prio}

Message Details:
${msg}

--------------------------------------------------
Sent via Raja Rawat Informatica Administrator Portfolio`);

    stripLatestMsg.textContent = `Ticket [${tId}] dispatched for ${name}. Opening mail client...`;
    window.location.href = `mailto:rajarawat262000@gmail.com?subject=${subject}&body=${body}`;

    alert(`✅ Incident ${tId} Logged!\n\nA pre-formatted email draft has opened in your mail app to reach Raja Rawat directly at rajarawat262000@gmail.com.`);
  });

  // =========================================================================
  // 11. MOBILE TOUCH SWIPE GESTURES (LEFT/RIGHT SWIPING)
  // =========================================================================
  const stageWrapper = document.querySelector('.slides-stage-wrapper');
  let touchStartX = 0;
  let touchStartY = 0;
  let touchEndX = 0;
  let touchEndY = 0;

  if (stageWrapper) {
    stageWrapper.addEventListener('touchstart', (e) => {
      touchStartX = e.changedTouches[0].screenX;
      touchStartY = e.changedTouches[0].screenY;
    }, { passive: true });

    stageWrapper.addEventListener('touchend', (e) => {
      // Don't trigger swipe if failure modal is active or user is typing
      if (agentFailureModal && agentFailureModal.classList.contains('active')) return;
      if (document.activeElement && ['INPUT', 'TEXTAREA', 'SELECT'].includes(document.activeElement.tagName)) return;

      touchEndX = e.changedTouches[0].screenX;
      touchEndY = e.changedTouches[0].screenY;
      handleTouchSwipe();
    }, { passive: true });
  }

  function handleTouchSwipe() {
    const deltaX = touchEndX - touchStartX;
    const deltaY = touchEndY - touchStartY;
    const minSwipeDistance = 45;

    // Ensure horizontal swipe is dominant
    if (Math.abs(deltaX) > Math.abs(deltaY) * 1.3 && Math.abs(deltaX) > minSwipeDistance) {
      if (deltaX < 0) {
        // Swiped Left -> Next Transformation
        if (currentStep < totalSteps - 1) {
          transitionToStep(currentStep + 1, true);
        }
      } else {
        // Swiped Right -> Previous Transformation
        if (currentStep > 0) {
          transitionToStep(currentStep - 1, true);
        }
      }
    }
  }

});
