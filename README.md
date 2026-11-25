<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AEIE PCB Timetable & Tracker</title>
    <style>
        :root {
            --bg-main: #eef9f2;
            --matcha-soft: #d8f3dc;
            --matcha-card: #e7f6ea;
            --sky-soft: #e0f2fe;
            --sky-card: #eff8ff;
            --pcb-green: #22c55e;
            --pcb-dark-green: #14532d;
            --sky-accent: #0ea5e9;
            --copper: #d97706;
            --gold-pad: #f59e0b;
            --bright-red: #ff1e1e;
            --ink: #0f172a;
            --outline: 2.5px solid #0f172a;
            --shadow-pcb: 5px 5px 0px #0f172a;
            --shadow-sm: 3px 3px 0px #0f172a;
            --radius-lg: 18px;
            --radius-md: 14px;
            --radius-sm: 10px;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Courier New", monospace, sans-serif;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-main);
            background-image:
                radial-gradient(rgba(15, 23, 42, 0.12) 1.25px, transparent 1.25px);
            background-size: 20px 20px;
            color: var(--ink);
            padding: 16px;
            max-width: 640px;
            margin: 0 auto;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }

        /* FIXED BACKGROUND PCB TRACE LAYER */
        .pcb-bg-layer {
            position: fixed;
            inset: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
            opacity: 0.35;
        }

        .trace-line {
            fill: none;
            stroke: #15803d;
            stroke-width: 2.5;
            stroke-linecap: round;
            stroke-linejoin: round;
            stroke-dasharray: 12 8;
            animation: flowTrace 9s linear infinite;
            transition: stroke 0.3s ease, filter 0.3s ease;
        }

        .trace-line.blue {
            stroke: #0284c7;
            animation-direction: reverse;
            animation-duration: 11s;
        }

        .trace-line.energized {
            stroke: #22c55e;
            stroke-width: 3.5;
            stroke-dasharray: none;
            filter: drop-shadow(0 0 6px #22c55e);
        }

        @keyframes flowTrace {
            0% { stroke-dashoffset: 200; }
            100% { stroke-dashoffset: 0; }
        }

        .app-shell {
            position: relative;
            z-index: 1;
        }

        .hidden {
            display: none !important;
        }

        /* =========================================================
           STAGE 1: DETAILED PCB IC CHIP LANDING ("A E I E")
           ========================================================= */
        #landing-stage {
            min-height: 88vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 20px;
            padding: 12px 0;
        }

        .pcb-motherboard-frame {
            position: relative;
            width: 100%;
            max-width: 440px;
            background: linear-gradient(145deg, var(--matcha-soft), var(--sky-soft));
            border: var(--outline);
            border-radius: var(--radius-lg);
            box-shadow: 8px 8px 0px var(--ink);
            padding: 34px 20px;
        }

        .mounting-hole {
            position: absolute;
            width: 14px;
            height: 14px;
            border-radius: 50%;
            background: var(--bg-main);
            border: 2.5px solid var(--ink);
            box-shadow: inset 0 0 0 2px var(--gold-pad);
        }
        .mh-tl { top: 10px; left: 10px; }
        .mh-tr { top: 10px; right: 10px; }
        .mh-bl { bottom: 10px; left: 10px; }
        .mh-br { bottom: 10px; right: 10px; }

        .smd-part {
            position: absolute;
            background: #334155;
            border: 2px solid var(--ink);
            border-radius: 3px;
            font-size: 0.55rem;
            color: #f8fafc;
            font-weight: 800;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .smd-part::before, .smd-part::after {
            content: "";
            position: absolute;
            background: var(--gold-pad);
            border: 1.5px solid var(--ink);
        }
        .smd-resistor {
            width: 34px;
            height: 14px;
            top: 14px;
            right: 42px;
        }
        .smd-resistor::before { width: 5px; height: 14px; left: -6px; top: -2px; }
        .smd-resistor::after { width: 5px; height: 14px; right: -6px; top: -2px; }

        .smd-cap {
            width: 16px;
            height: 22px;
            bottom: 14px;
            left: 42px;
            background: #b45309;
        }

        .silkscreen-tag {
            font-size: 0.68rem;
            font-weight: 900;
            letter-spacing: 1.5px;
            color: var(--pcb-dark-green);
            text-align: center;
            margin-bottom: 14px;
        }

        #aeie-power-btn {
            position: relative;
            width: 100%;
            background: var(--sky-card);
            border: 3px solid var(--ink);
            border-radius: var(--radius-md);
            box-shadow: 6px 6px 0px var(--ink);
            padding: 26px 14px;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            transition: transform 0.12s ease, box-shadow 0.12s ease, background 0.2s ease;
        }

        #aeie-power-btn:hover {
            background: #ffffff;
        }

        #aeie-power-btn:active,
        #aeie-power-btn.pressed {
            transform: translate(4px, 4px);
            box-shadow: 2px 2px 0px var(--ink);
            background: var(--matcha-card);
        }

        .ic-notch {
            position: absolute;
            top: -2px;
            left: 50%;
            transform: translateX(-50%);
            width: 24px;
            height: 11px;
            background: var(--matcha-soft);
            border-left: 3px solid var(--ink);
            border-right: 3px solid var(--ink);
            border-bottom: 3px solid var(--ink);
            border-bottom-left-radius: 12px;
            border-bottom-right-radius: 12px;
        }

        .ic-pin-row {
            display: flex;
            justify-content: space-between;
            width: 86%;
        }

        .ic-pin {
            width: 12px;
            height: 10px;
            background: var(--gold-pad);
            border: 2px solid var(--ink);
            border-radius: 2px;
        }

        .aeie-letters {
            font-size: clamp(2.2rem, 9vw, 3.2rem);
            font-weight: 900;
            letter-spacing: 10px;
            color: var(--ink);
            display: flex;
            align-items: center;
            justify-content: center;
            padding-left: 8px;
        }

        .led-row {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.78rem;
            font-weight: 800;
            text-transform: uppercase;
        }

        .status-led {
            width: 13px;
            height: 13px;
            border-radius: 50%;
            background: #ef4444;
            border: 2px solid var(--ink);
            box-shadow: 0 0 8px rgba(239, 68, 68, 0.8);
            transition: all 0.25s ease;
        }

        .status-led.active {
            background: #22c55e;
            box-shadow: 0 0 12px #22c55e;
        }

        .boot-cta {
            background: var(--matcha-soft);
            border: var(--outline);
            border-radius: 999px;
            box-shadow: var(--shadow-sm);
            padding: 10px 20px;
            font-size: 0.85rem;
            font-weight: 900;
            letter-spacing: 0.6px;
            text-align: center;
        }

        /* =========================================================
           TOP DASHBOARD HEADER & LIVE CLOCKS
           ========================================================= */
        .top-banner {
            background: linear-gradient(90deg, var(--matcha-soft), var(--sky-soft));
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-pcb);
            padding: 14px 16px;
            margin-bottom: 14px;
        }

        .top-banner-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }

        .college-title {
            font-size: 1rem;
            font-weight: 900;
            text-transform: uppercase;
        }

        .branch-sub {
            font-size: 0.78rem;
            font-weight: 800;
            color: var(--pcb-dark-green);
            margin-top: 2px;
        }

        .home-chip-btn {
            background: #ffffff;
            border: 2px solid var(--ink);
            border-radius: var(--radius-sm);
            box-shadow: 2px 2px 0px var(--ink);
            padding: 6px 10px;
            font-size: 0.75rem;
            font-weight: 900;
            cursor: pointer;
        }

        .home-chip-btn:active {
            transform: translate(2px, 2px);
            box-shadow: none;
        }

        /* BRIGHT RED END-SEM COUNTDOWN */
        .endsem-alert-box {
            background: #0b1120;
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-pcb);
            padding: 12px 14px;
            margin-bottom: 14px;
            text-align: center;
        }

        .endsem-label {
            color: #fca5a5;
            font-size: 0.72rem;
            font-weight: 900;
            letter-spacing: 1.5px;
            text-transform: uppercase;
            margin-bottom: 4px;
        }

        .endsem-bright-red {
            color: var(--bright-red);
            font-size: clamp(1.1rem, 4.5vw, 1.5rem);
            font-weight: 900;
            letter-spacing: 1px;
            text-shadow: 0 0 10px rgba(255, 30, 30, 0.75);
        }

        .endsem-date-note {
            color: #94a3b8;
            font-size: 0.68rem;
            margin-top: 4px;
        }

        /* NEXT CLASS LIVE CLOCK CARD */
        .next-class-box {
            background: var(--sky-card);
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-pcb);
            padding: 12px 14px;
            margin-bottom: 14px;
        }

        .clock-top-line {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.72rem;
            font-weight: 900;
            color: #0369a1;
            margin-bottom: 6px;
        }

        .next-class-headline {
            font-size: 0.96rem;
            font-weight: 900;
            color: var(--ink);
        }

        .next-class-sub {
            font-size: 0.78rem;
            font-weight: 700;
            color: #334155;
            margin-top: 4px;
        }

        /* GROUP SWITCHER */
        .group-bar {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 16px;
        }

        .grp-btn {
            background: #ffffff;
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-sm);
            padding: 10px;
            font-size: 0.86rem;
            font-weight: 900;
            cursor: pointer;
            transition: all 0.14s ease;
        }

        .grp-btn.selected {
            background: var(--sky-accent);
            color: #ffffff;
            transform: translate(2px, 2px);
            box-shadow: 1px 1px 0px var(--ink);
        }

        /* =========================================================
           STAGE 2: DAYS PCB NODES MENU
           ========================================================= */
        .section-heading {
            font-size: 0.92rem;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .days-node-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .day-pcb-node {
            position: relative;
            background: linear-gradient(90deg, var(--sky-card), var(--matcha-card));
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-pcb);
            padding: 15px 16px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
            transition: transform 0.14s ease, box-shadow 0.14s ease, background 0.2s ease;
        }

        .day-pcb-node:hover {
            background: linear-gradient(90deg, #e0f2fe, #dcfce7);
        }

        .day-pcb-node:active,
        .day-pcb-node.linking {
            transform: translate(3px, 3px);
            box-shadow: 2px 2px 0px var(--ink);
            background: #bbf7d0;
        }

        .node-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .node-pad {
            width: 16px;
            height: 16px;
            flex-shrink: 0;
            border-radius: 50%;
            background: var(--gold-pad);
            border: 2.5px solid var(--ink);
            box-shadow: 0 0 0 2px var(--matcha-soft);
        }

        .node-day-name {
            font-size: 1.05rem;
            font-weight: 900;
        }

        .node-code {
            font-size: 0.7rem;
            font-weight: 800;
            color: #475569;
        }

        .node-right {
            font-size: 0.82rem;
            font-weight: 900;
            background: #ffffff;
            border: 2px solid var(--ink);
            border-radius: 8px;
            padding: 4px 10px;
            white-space: nowrap;
        }

        /* CIRCUIT CONNECTION PULSE BAR */
        .circuit-bridge {
            height: 6px;
            width: 100%;
            background: #cbd5e1;
            border: 1.5px solid var(--ink);
            border-radius: 999px;
            overflow: hidden;
            margin-bottom: 14px;
        }

        .circuit-bridge-fill {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #22c55e, #0ea5e9);
            transition: width 0.35s ease;
        }

        .circuit-bridge-fill.powered {
            width: 100%;
        }

        /* =========================================================
           STAGE 3: SELECTED DAY CLASSES & DESCRIPTIONS
           ========================================================= */
        .day-toolbar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
            margin-bottom: 14px;
        }

        .back-nodes-btn {
            background: var(--matcha-soft);
            border: var(--outline);
            border-radius: var(--radius-sm);
            box-shadow: var(--shadow-sm);
            padding: 9px 14px;
            font-size: 0.84rem;
            font-weight: 900;
            cursor: pointer;
        }

        .back-nodes-btn:active {
            transform: translate(2px, 2px);
            box-shadow: none;
        }

        .active-node-badge {
            background: var(--sky-soft);
            border: 2px solid var(--ink);
            border-radius: var(--radius-sm);
            padding: 7px 12px;
            font-size: 0.82rem;
            font-weight: 900;
        }

        .class-pcb-card {
            position: relative;
            background: var(--matcha-card);
            border: var(--outline);
            border-radius: var(--radius-md);
            box-shadow: var(--shadow-pcb);
            padding: 14px 16px;
            margin-bottom: 14px;
            animation: cardBoot 0.32s ease both;
        }

        .class-pcb-card.lab-card {
            background: var(--sky-card);
        }

        .via-dot {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: #22c55e;
            border: 2px solid var(--ink);
            box-shadow: 0 0 6px #22c55e;
        }

        .slot-time-pill {
            display: inline-block;
            background: #ffffff;
            border: 1.8px solid var(--ink);
            border-radius: 7px;
            padding: 2px 8px;
            font-size: 0.74rem;
            font-weight: 900;
            margin-bottom: 6px;
        }

        .slot-subject {
            font-size: 1rem;
            font-weight: 900;
            padding-right: 18px;
        }

        .slot-faculty-room {
            font-size: 0.8rem;
            font-weight: 800;
            color: #1e293b;
            margin-top: 3px;
        }

        .slot-description {
            margin-top: 8px;
            padding-top: 8px;
            border-top: 2px dashed rgba(15, 23, 42, 0.35);
            font-size: 0.8rem;
            line-height: 1.42;
            color: #1e293b;
            font-weight: 600;
        }

        .break-pcb-banner {
            background: #ffffff;
            border: 2.5px dashed var(--ink);
            border-radius: var(--radius-sm);
            padding: 9px 12px;
            text-align: center;
            font-size: 0.82rem;
            font-weight: 900;
            margin-bottom: 14px;
        }

        /* Group visibility */
        .only-gr1, .only-gr2 {
            display: none;
        }
        body.gr1-mode .only-gr1 {
            display: block;
        }
        body.gr2-mode .only-gr2 {
            display: block;
        }

        @keyframes cardBoot {
            from {
                opacity: 0;
                transform: translateY(8px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>
<body class="gr1-mode">

    <!-- BACKGROUND PCB TRACES -->
    <svg class="pcb-bg-layer" viewBox="0 0 600 1000" preserveAspectRatio="none">
        <path id="trace-1" class="trace-line" d="M 40 0 L 40 180 L 130 270 L 130 520 L 220 610 L 220 1000" />
        <path id="trace-2" class="trace-line blue" d="M 550 0 L 550 220 L 440 330 L 440 650 L 350 740 L 350 1000" />
        <path id="trace-3" class="trace-line" d="M 150 0 L 150 110 L 320 280 L 490 280" />
        <circle cx="130" cy="270" r="6" fill="#f59e0b" stroke="#0f172a" stroke-width="2" />
        <circle cx="440" cy="330" r="6" fill="#0ea5e9" stroke="#0f172a" stroke-width="2" />
        <circle cx="220" cy="610" r="6" fill="#22c55e" stroke="#0f172a" stroke-width="2" />
    </svg>

    <div class="app-shell">

        <!-- =====================================================
             STAGE 1: PCB IC CHIP ENTRY ("A E I E")
             ===================================================== -->
        <section id="landing-stage">
            <div class="pcb-motherboard-frame">
                <span class="mounting-hole mh-tl"></span>
                <span class="mounting-hole mh-tr"></span>
                <span class="mounting-hole mh-bl"></span>
                <span class="mounting-hole mh-br"></span>

                <div class="smd-part smd-resistor">10k</div>
                <div class="smd-part smd-cap">C1</div>

                <div class="silkscreen-tag">HITK • AEIE • REV 2026-27</div>

                <button id="aeie-power-btn" type="button" aria-label="Power on AEIE Timetable">
                    <div class="ic-notch"></div>
                    <div class="ic-pin-row">
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                    </div>

                    <div class="aeie-letters">A E I E</div>

                    <div class="led-row">
                        <span class="status-led" id="chip-led"></span>
                        <span id="chip-led-label">STANDBY • TAP TO ENERGIZE</span>
                    </div>

                    <div class="ic-pin-row">
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                        <span class="ic-pin"></span>
                    </div>
                </button>
            </div>

            <div class="boot-cta">⚡ PRESS "A E I E" IC TO LOAD DAY NODES</div>
        </section>

        <!-- =====================================================
             MAIN DASHBOARD (STAGES 2 & 3)
             ===================================================== -->
        <main id="dashboard-stage" class="hidden">

            <div class="top-banner">
                <div class="top-banner-row">
                    <div>
                        <div class="college-title">Heritage Institute of Technology</div>
                        <div class="branch-sub">AEIE • B.Tech 1st Year 1st Sem (2026–2027)</div>
                    </div>
                    <button id="return-chip-btn" class="home-chip-btn" type="button">🔌 IC STANDBY</button>
                </div>
            </div>

            <!-- BRIGHT RED END-SEM COUNTDOWN -->
            <div class="endsem-alert-box">
                <div class="endsem-label">🚨 END-SEMESTER EXAM COUNTDOWN 🚨</div>
                <div class="endsem-bright-red" id="endsem-countdown-text">LOADING...</div>
                <div class="endsem-date-note">Target: Dec 16, 2026</div>
            </div>

            <!-- LIVE CLOCK & TIME UNTIL NEXT CLASS -->
            <div class="next-class-box">
                <div class="clock-top-line">
                    <span>🕒 LIVE CLOCK SYNC</span>
                    <span id="live-digital-clock">--:--:--</span>
                </div>
                <div class="next-class-headline" id="next-class-title">Checking next class...</div>
                <div class="next-class-sub" id="next-class-timer">Calculating remaining time...</div>
            </div>

            <!-- GROUP 1 / GROUP 2 SWITCH -->
            <div class="group-bar">
                <button id="group-1-btn" class="grp-btn selected" type="button">GROUP 1</button>
                <button id="group-2-btn" class="grp-btn" type="button">GROUP 2</button>
            </div>

            <!-- CIRCUIT LINK PROGRESS INDICATOR -->
            <div class="circuit-bridge">
                <div id="circuit-fill" class="circuit-bridge-fill"></div>
            </div>

            <!-- STAGE 2: DAYS MENU -->
            <section id="days-menu-stage">
                <div class="section-heading">🧩 SELECT DAY CIRCUIT NODE</div>
                <div class="days-node-list">
                    <div class="day-pcb-node" data-day="mon">
                        <div class="node-left">
                            <span class="node-pad"></span>
                            <div>
                                <div class="node-day-name">Monday</div>
                                <div class="node-code">NODE-01 • ELE1001, MTH1101, HUM1001</div>
                            </div>
                        </div>
                        <span class="node-right">CONNECT ⚡</span>
                    </div>

                    <div class="day-pcb-node" data-day="tue">
                        <div class="node-left">
                            <span class="node-pad"></span>
                            <div>
                                <div class="node-day-name">Tuesday</div>
                                <div class="node-code">NODE-02 • CHM1051 Lab, CHM1001, HUM1001, CSE1001</div>
                            </div>
                        </div>
                        <span class="node-right">CONNECT ⚡</span>
                    </div>

                    <div class="day-pcb-node" data-day="wed">
                        <div class="node-left">
                            <span class="node-pad"></span>
                            <div>
                                <div class="node-day-name">Wednesday</div>
                                <div class="node-code">NODE-03 • CSE1001, CHM1001, ELE1001</div>
                            </div>
                        </div>
                        <span class="node-right">CONNECT ⚡</span>
                    </div>

                    <div class="day-pcb-node" data-day="thu">
                        <div class="node-left">
                            <span class="node-pad"></span>
                            <div>
                                <div class="node-day-name">Thursday</div>
                                <div class="node-code">NODE-04 • CHM1001, CSE1001, MTH1101, ELE1001</div>
                            </div>
                        </div>
                        <span class="node-right">CONNECT ⚡</span>
                    </div>

                    <div class="day-pcb-node" data-day="fri">
                        <div class="node-left">
                            <span class="node-pad"></span>
                            <div>
                                <div class="node-day-name">Friday</div>
                                <div class="node-code">NODE-05 • ELE1051 Lab, Soft Skills, Tutorials, HUM1051</div>
                            </div>
                        </div>
                        <span class="node-right">CONNECT ⚡</span>
                    </div>
                </div>
            </section>

            <!-- STAGE 3: DAY DETAIL VIEW -->
            <section id="day-detail-stage" class="hidden">
                <div class="day-toolbar">
                    <button id="back-to-days-btn" class="back-nodes-btn" type="button">⬅ ALL DAYS</button>
                    <div id="selected-day-badge" class="active-node-badge">DAY</div>
                </div>

                <!-- MONDAY -->
                <div id="schedule-mon" class="day-schedule-panel hidden">
                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">09:00 AM – 10:00 AM</div>
                        <div class="slot-subject">ELE1001 • Basic Electrical Engineering</div>
                        <div class="slot-faculty-room">Prof. Anirban Kolay (AK) • Room ICT004</div>
                        <div class="slot-description">Covers DC/AC circuit laws, network theorems, magnetic circuits, transformers, and basic electrical machine principles.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">10:00 AM – 11:00 AM</div>
                        <div class="slot-subject">MTH1101 • Mathematics-IB</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Sudipta Sarkar (SDS) • Room ICT004</div>
                        <div class="slot-description">Focuses on multivariable calculus, partial differentiation, sequences and infinite series, and convergence tests.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">11:00 AM – 12:00 NOON</div>
                        <div class="slot-subject">HUM1001 • English / Humanities</div>
                        <div class="slot-faculty-room">Prof. Koyel Mallick (KM) • Room ICT004</div>
                        <div class="slot-description">Builds technical communication, structured writing, clarity in expression, and professional vocabulary.</div>
                    </div>

                    <div class="break-pcb-banner">☕ BREAK • 12:00 NOON – 01:00 PM</div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">01:00 PM – 02:00 PM</div>
                        <div class="slot-subject">MTH1101 • Mathematics-IB</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Ashesh Paul (AP) • Room ICT004</div>
                        <div class="slot-description">Problem-solving lecture on integral calculus, multiple integrals, and vector calculus applications.</div>
                    </div>

                    <div class="class-pcb-card only-gr1">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">02:00 PM – 03:00 PM</div>
                        <div class="slot-subject">Mentoring Session (Gr. 1)</div>
                        <div class="slot-faculty-room">Faculty Mentor • AEIE</div>
                        <div class="slot-description">Academic check-in, semester planning, doubt guidance, and study habits review.</div>
                    </div>

                    <div class="class-pcb-card only-gr1">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">03:00 PM – 04:00 PM</div>
                        <div class="slot-subject">Library Hour (Gr. 1)</div>
                        <div class="slot-faculty-room">Central / Dept Library</div>
                        <div class="slot-description">Dedicated reading and self-study block for reference books, notes, and problem sheets.</div>
                    </div>

                    <div class="class-pcb-card lab-card only-gr2">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">02:00 PM – 05:00 PM</div>
                        <div class="slot-subject">CSE1051 • C Programming Lab (Gr. 2)</div>
                        <div class="slot-faculty-room">ANP / SBJ / SKD • Room ICTB01</div>
                        <div class="slot-description">Practical coding lab on control structures, arrays, functions, pointers, and debugging C programs.</div>
                    </div>
                </div>

                <!-- TUESDAY -->
                <div id="schedule-tue" class="day-schedule-panel hidden">
                    <div class="class-pcb-card lab-card only-gr1">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">09:00 AM – 12:00 NOON</div>
                        <div class="slot-subject">CHM1051 • Chemistry Lab (Gr. 1)</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Piyali Paul (PP) / HS • Room CB101</div>
                        <div class="slot-description">Hands-on chemistry experiments covering volumetric analysis, water testing, pH/conductivity, and lab observation logs.</div>
                    </div>

                    <div class="class-pcb-card lab-card only-gr2">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">09:00 AM – 12:00 NOON</div>
                        <div class="slot-subject">CHM1051 • Chemistry Lab (Gr. 2)</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Sangeeta Ghosh (SG) / SKM • Room CB102</div>
                        <div class="slot-description">Hands-on chemistry experiments covering titrations, analytical measurements, and practical lab record work.</div>
                    </div>

                    <div class="break-pcb-banner">☕ BREAK • 12:00 NOON – 01:00 PM</div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">01:00 PM – 02:00 PM</div>
                        <div class="slot-subject">CHM1001 • Engineering Chemistry</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Piyali Paul (PP) • Room ICT004</div>
                        <div class="slot-description">Covers chemical bonding, molecular orbitals, electrochemistry, thermodynamics, and engineering materials.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">02:00 PM – 03:00 PM</div>
                        <div class="slot-subject">HUM1001 • English / Humanities</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Aditi Bhunia Chakraborty (ABC) • Room ICT004</div>
                        <div class="slot-description">Focuses on formal writing, comprehension, presentation structure, and effective workplace communication.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">03:00 PM – 04:00 PM</div>
                        <div class="slot-subject">CSE1001 • C Programming</div>
                        <div class="slot-faculty-room">Dr. Anastuv Pal (ANP) • Room ICT004</div>
                        <div class="slot-description">Core C concepts including data types, operators, loops, branching, and algorithm logic.</div>
                    </div>

                    <div class="class-pcb-card only-gr2">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">04:00 PM – 05:00 PM</div>
                        <div class="slot-subject">ELE1001 Tutorial (Gr. 2)</div>
                        <div class="slot-faculty-room">Prof. Suvobrata Sil (SBS) • Room ICT109</div>
                        <div class="slot-description">Guided tutorial solving numerical problems on DC/AC circuits and electrical fundamentals.</div>
                    </div>
                </div>

                <!-- WEDNESDAY -->
                <div id="schedule-wed" class="day-schedule-panel hidden">
                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">09:00 AM – 10:00 AM</div>
                        <div class="slot-subject">CSE1001 • C Programming</div>
                        <div class="slot-faculty-room">Dr. Anastuv Pal (ANP) • Room ICT004</div>
                        <div class="slot-description">Covers arrays, strings, modular functions, and memory behavior in C programs.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">10:00 AM – 11:00 AM</div>
                        <div class="slot-subject">CHM1001 • Engineering Chemistry</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Dulal Chandra Mukherjee (DCM) • Room ICT004</div>
                        <div class="slot-description">Covers reaction kinetics, chemical equilibrium, catalysis, and industrial chemistry topics.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">11:00 AM – 12:00 NOON</div>
                        <div class="slot-subject">ELE1001 • Basic Electrical Engineering</div>
                        <div class="slot-faculty-room">Prof. Anirban Kolay (AK) • Room ICT004</div>
                        <div class="slot-description">Covers AC phasors, single-phase and three-phase circuits, power factor, and basic transformers.</div>
                    </div>

                    <div class="break-pcb-banner">☕ BREAK • 12:00 NOON – 01:00 PM</div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">01:00 PM – 02:00 PM</div>
                        <div class="slot-subject">CHM1001 • Engineering Chemistry</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Enakshi Dinda (ED) • Room ICT004</div>
                        <div class="slot-description">Covers spectroscopy, polymers, corrosion control, and applied chemical principles.</div>
                    </div>

                    <div class="class-pcb-card lab-card only-gr1">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">02:00 PM – 05:00 PM</div>
                        <div class="slot-subject">CSE1051 • C Programming Lab (Gr. 1)</div>
                        <div class="slot-faculty-room">ANP / KM / SKD • Room ICTB03</div>
                        <div class="slot-description">Practical lab implementing C programs with arrays, pointers, structures, and file operations.</div>
                    </div>

                    <div class="class-pcb-card only-gr2">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">02:00 PM – 03:00 PM</div>
                        <div class="slot-subject">MTH1101 Tutorial (Gr. 2)</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Sudipta Sarkar (SDS) • Room ICT109</div>
                        <div class="slot-description">Problem-solving session on calculus, series convergence, and exam-style math questions.</div>
                    </div>
                </div>

                <!-- THURSDAY -->
                <div id="schedule-thu" class="day-schedule-panel hidden">
                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">09:00 AM – 10:00 AM</div>
                        <div class="slot-subject">CHM1001 • Engineering Chemistry</div>
                        <div class="slot-faculty-room">Prof. (Dr.) Piyali Paul (PP) • Room ICT004</div>
                        <div class="slot-description">Lecture on chemical principles, structure-property relationships, and analytical concepts.</div>
                    </div>

                    <div class="class-pcb-card">
                        <span class="via-dot"></span>
                        <div class="slot-time-pill">10:00 AM – 11:00 AM</div>
                        <div class="slot-subject">CSE1001 • C Programming</div>
                        <div class="slot-faculty-room">Dr. Anastuv Pal (ANP
