================================================================================
NMDC LIMITED - BAILADILA IRON ORE MINES
LOW-VISIBILITY HAULAGE SAFETY PROGRAMME
Technical Research Package  |  Revision 1.0
================================================================================

SUBJECT
  Safe and continuous movement of Heavy Earth Moving Machinery (HEMM) on mine
  haul roads under dense monsoon fog and low-visibility conditions at the
  Bailadila sector (Kirandul and Bacheli complexes), Dantewada, Chhattisgarh.

SCALE OF THE PACKAGE
  14 documents  |  151 pages  |  4 volumes + master summary

--------------------------------------------------------------------------------
CONTENTS
--------------------------------------------------------------------------------

Research/
|
+-- Research-Summary.pdf ................................ NMDC-LVH-SUM-00  (11 p)
|     Consolidated findings and recommendations. START HERE.
|     Readable on its own; also serves as the navigation guide.
|
+-- Problem-Research/                                      VOLUME I
|   +-- Problem-Background.pdf .......................... NMDC-LVH-PR-01  (10 p)
|   |     Corporate and production context, Bailadila topography and haul road
|   |     geometry, monsoon meteorology, visibility physics and the extinction
|   |     coefficient, the four-lever production loss model, accident archetypes,
|   |     statutory frame, and the problem restated in engineering terms.
|   |
|   +-- Existing-Challenges.pdf ......................... NMDC-LVH-PR-02  (11 p)
|   |     Why each current control fails at MOR 3-5 m: lighting, delineation,
|   |     cameras, audio-visual alarms, proximity devices, GPS fleet management,
|   |     CCTV, voice radio, and administrative controls. Environmental,
|   |     infrastructure, human-factors and organisational constraints.
|   |     Constraint register C-01 to C-26; failure-mode catalogue F1 to F12.
|   |
|   +-- Field-Requirements.pdf .......................... NMDC-LVH-PR-03  (13 p)
|         Numbered, testable specification. Graded visibility classes V0-V4.
|         Performance envelope DERIVED from stopping distance on a wet 1-in-16
|         descent. Functional, HMI, communications, control room, environmental,
|         integration, reliability, regulatory and acceptance requirements.
|         12 programme KPIs.
|
+-- Existing-Solutions/                                    VOLUME II
|   +-- Existing-Systems.pdf ............................ NMDC-LVH-ES-01  (11 p)
|   |     Survey of mining collision avoidance (Hexagon MineProtect, Cat
|   |     MineStar Detect, OEM packages, magnetic PDS, Indian DGMS-compliant
|   |     suppliers), autonomous haulage, fleet management, fog management, and
|   |     adjacent industries (aviation, automotive, rail, maritime, defence).
|   |     Research prototypes and public datasets. Assessment matrix.
|   |
|   +-- Technology-Comparison.pdf ....................... NMDC-LVH-ES-02  (12 p)
|   |     Weighted evaluation by layer: perception, positioning, communications,
|   |     compute, HMI, visibility sensing. Physics-based screening. Recommended
|   |     technology set and an explicit list of technologies REJECTED.
|   |
|   +-- Gap-Analysis.pdf ................................ NMDC-LVH-ES-03  (12 p)
|         Gaps G1-G6 and why a mature market has left them open. The
|         architectural inversion. The recommended FIHGAS architecture (layers
|         L0-L8), degradation behaviour, phased roadmap, 14-entry risk register,
|         and outline benefit model.
|
+-- Technology/                                            VOLUME III
|   +-- Radar-Research.pdf .............................. NMDC-LVH-TC-01  (12 p)
|   |     FMCW principles, band selection, fog propagation physics, 4D imaging
|   |     radar, haul-road-specific challenges (ground clutter on gradients,
|   |     multipath in a ferrous pit, mutual interference, radome fouling),
|   |     radar odometry, sensor placement, draft specification, validation plan.
|   |
|   +-- Sensor-Research.pdf ............................. NMDC-LVH-TC-02  (12 p)
|   |     The positioning stack in depth (GNSS, RTK, tight-coupled INS,
|   |     dual-antenna heading, integrity and protection levels), the digital
|   |     road model, thermal imaging, LiDAR in its mapping role, visibility
|   |     instrumentation, fusion framework, calibration.
|   |
|   +-- AI-ML-Research.pdf .............................. NMDC-LVH-TC-03  (11 p)
|   |     Where machine learning belongs and where it does not. Radar perception,
|   |     multimodal fusion under degradation, trajectory prediction, fog
|   |     nowcasting, false-alarm reduction, data strategy, edge constraints,
|   |     model governance, and the failure modes ML introduces.
|   |
|   +-- Communication-Technology.pdf .................... NMDC-LVH-TC-04  (11 p)
|         V2V message set, bearer evaluation (C-V2X PC5, ITS-G5, sub-GHz CAS),
|         propagation in ridge-and-pit terrain, congestion at fleet scale,
|         backhaul architecture, personal devices, security, spectrum position,
|         and the RF survey specification.
|
+-- References/                                            VOLUME IV
    +-- Research-Papers/
    |   +-- Research-Papers-Index.pdf .................. NMDC-LVH-RF-01   (8 p)
    |         Annotated bibliography, 44 entries across 10 themes, each with an
    |         EVIDENCE CLASS rating (Field / Chamber / Simulation / Survey /
    |         Industry) and an evidence-strength assessment of the package's
    |         principal claims.
    |
    +-- Government-Reports/
    |   +-- Government-Reports-Index.pdf ............... NMDC-LVH-RF-02   (8 p)
    |         Mines Act 1952, MMR 1961, DGMS Technical Circulars 03/2024,
    |         06/2020 and 08/2017, National Steel Policy 2017, NMDC corporate
    |         sources, spectrum and environmental instruments, IMD sources.
    |         Compliance obligation matrix and the approval pathway for
    |         control-authority functions.
    |
    +-- Standards/
        +-- Standards-Index.pdf ........................ NMDC-LVH-RF-03   (9 p)
              52 standards families across ingress and environmental
              qualification, earth-moving machinery safety (ISO 16001, 5006,
              3450, 17757), functional safety, radar and radio, V2X, positioning,
              visibility measurement (WMO), HMI ergonomics, cybersecurity, data
              interfaces and quality. Includes a 22-line PROCUREMENT
              SPECIFICATION CHECKLIST.

--------------------------------------------------------------------------------
THE CORE ARGUMENT, IN SIX LINES
--------------------------------------------------------------------------------
  1. At MOR 3-5 m the extinction coefficient is 600-1000 /km. Visible cameras and
     NIR LiDAR are blind. No algorithm recovers a signal that never arrived.
  2. Millimetre-wave radar (3.9 mm) and RF sit in the deep Rayleigh regime
     against 1-20 um droplets. Fog costs radar roughly 1 dB over 120 m.
  3. Therefore: do not try to see through fog. Reduce what must be seen.
  4. The STATIC world comes from a precise position against a digital road model
     (aviation Synthetic Vision). The COOPERATIVE world comes from V2V broadcast
     (ADS-B / AIS / mining CAS). Only the RESIDUAL world needs a sensor - and
     77/79 GHz radar solves that bounded problem.
  5. Every constituent capability already exists commercially. The gap is
     integration and the specific dense-fog operating point - which no vendor
     currently characterises against calibrated visibility.
  6. Replace the binary go/no-go fog decision with graded, instrumented,
     per-zone operating classes V0 to V4.

--------------------------------------------------------------------------------
THE SINGLE HIGHEST-VALUE NEXT ACTION
--------------------------------------------------------------------------------
  Three Phase 0 activities each need one instrumented vehicle driving the haul
  road network across a monsoon: the GNSS availability survey, the RF
  propagation survey, and the instrumented radar campaign with a co-located
  calibrated visibility meter.

  COMBINE THEM INTO ONE FIELD CAMPAIGN. One vehicle, one instrumentation fit,
  one dataset, one monsoon. It resolves the three largest technical
  uncertainties in the programme before any procurement decision, at a small
  fraction of the cost of three separate exercises.

  Fog data can only be collected in a limited annual window. A missed window
  costs a full year.

--------------------------------------------------------------------------------
IMPORTANT CAVEATS - PLEASE READ
--------------------------------------------------------------------------------
  * This package is a RESEARCH AND SOLUTION-DESIGN INPUT, not a validated
    engineering design and not a procurement document in its own right.

  * Figures on NMDC production, statutory provisions, commercial systems and
    published research are compiled from PUBLIC SOURCES as at September 2026.

  * Values marked INDICATIVE - notably the rainfall and humidity climatology,
    the haul road geometry parameters, the braking deceleration coefficient used
    in the stopping-distance derivation, and all cost and maturity positioning -
    are placeholders for framing. They MUST be replaced with measured NMDC site
    data before use in a specification or tender.

  * The package DOES NOT predict radar detection range at Bailadila, quantify
    the production loss, or size the benefit. Section 13 of the Research Summary
    states explicitly what is and is not established, and the Research Papers
    Index carries an evidence-class rating on every source.

  * Statutory instruments and standards are amended. Every provision summarised
    here must be verified against the current published text before it is relied
    upon.

  * An open question that materially affects the benefit case: whether the
    single-track Kirandul-Kothavalasa rail line is ALSO a binding constraint
    during the same monsoon months. If it is, recovered haulage hours may be
    partly absorbed by an inability to despatch. Phase 0 must resolve this.

================================================================================
Prepared: September 2026  |  Revision 1.0  |  Internal Research Document
================================================================================
