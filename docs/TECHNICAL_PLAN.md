# Technical Documentation Plan: Cloud-Based SDN Management Dashboard Simulation using OMNeT++

## Project Metadata

**Project Name:** Cloud-Based SDN Management Dashboard Simulation
**Duration:** Half college semester (~7-8 weeks)
**Team Members:** Taran Agarwal and Justin Bravo
**Institution:** [Your University Name]
**Course:** Cloud Computing
**Start Date:** [Insert Start Date]
**Expected Completion:** [Insert End Date]

---

## Executive Summary

This project addresses the gap in visual, user-friendly SDN simulation tools by creating an interactive dashboard for managing Software-Defined Network abstractions in cloud environments. Using OMNeT++ as the simulation engine, we will implement two core SDN capabilities:

1. **Network Slicing:** Virtual network isolation for multi-tenant scenarios
2. **Dynamic Flow Provisioning:** On-demand flow rule installation and management

The project differentiates itself from existing tools (Mininet, SDN4CoRE) by providing an experiment-focused, web-based dashboard specifically designed for educational and research purposes.

---

## Problem Statement

### Current State
- **SDN adoption** in cloud infrastructure is growing rapidly
- **Existing tools** like Mininet provide topology emulation but lack modular experiment dashboards
- **OMNeT++ SDN frameworks** (SDN4CoRE, OpenFlowOMNeTSuite) enable protocol simulation but have limited visualization
- **Educational gap** exists in hands-on SDN experimentation tools

### Our Solution
A modular dashboard that enables:
- Visual creation and management of network slices
- Real-time flow rule installation and monitoring
- Interactive experimentation with SDN concepts
- Clear visualization of SDN controller operations

---

## Project Scope

### In Scope
✅ Network slice creation, deletion, and configuration
✅ Dynamic flow rule installation and removal
✅ Web-based dashboard with topology visualization
✅ Real-time metrics and statistics display
✅ Basic QoS and ACL configuration per slice
✅ Flow table inspection and management
✅ Multi-tenant simulation scenarios

### Out of Scope
❌ Production-ready SDN controller implementation
❌ Advanced routing algorithms beyond basic OpenFlow
❌ Integration with physical SDN hardware
❌ Machine learning-based traffic optimization
❌ Large-scale performance optimization (1000+ nodes)
❌ Security vulnerability testing

---

## Technical Architecture

### System Components

```
┌─────────────────────────────────────────────────────────────┐
│                    Web Dashboard (Frontend)                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Slice Manager│  │ Flow Manager │  │ Topology View│      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────┬───────────────────────────────────┘
                          │ REST API / WebSockets
┌─────────────────────────▼───────────────────────────────────┐
│              Dashboard Backend (Node.js/Flask)               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  API Router  │  │ Event Handler│  │  DB/Cache    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────┬───────────────────────────────────┘
                          │ JSON-RPC / Custom Protocol
┌─────────────────────────▼───────────────────────────────────┐
│              OMNeT++ Simulation Environment                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         SDN Controller Module (C++)                   │   │
│  │  ┌────────────────┐  ┌────────────────┐             │   │
│  │  │ Slice Manager  │  │  Flow Manager  │             │   │
│  │  └────────────────┘  └────────────────┘             │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         OpenFlow Switches (INET/SDN4CoRE)            │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         Simulated Hosts and Traffic Generators       │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

### Technology Stack

#### Simulation Layer
- **OMNeT++ 7.0.0:** Discrete-event simulation framework
- **INET Framework:** Network protocol models (TCP/IP, Ethernet, etc.)
- **OpenFlowOMNeTSuite or SDN4CoRE:** OpenFlow 1.3+ protocol implementation
- **Language:** C++ for controller logic and network modules
- **Configuration:** NED language for topology, INI files for parameters

#### Dashboard Layer
- **Frontend Framework:** React.js or Vue.js 3
- **Visualization:** D3.js or Cytoscape.js for network topology
- **UI Library:** Material-UI or Ant Design
- **State Management:** Redux or Vuex
- **Backend Framework:** Node.js (Express) or Python (Flask/FastAPI)
- **Real-time Communication:** WebSockets (Socket.io or native)
- **Data Format:** JSON for API communication

#### Development Tools
- **Version Control:** Git with feature branch workflow
- **Build Tools:** npm/webpack (frontend), make (OMNeT++)
- **Testing:** Jest (frontend), OMNeT++ test framework (simulation)
- **Documentation:** Markdown, JSDoc/Doxygen
- **Optional:** Docker for containerized deployment

---

## Implementation Plan

### Phase 1: Environment Setup & Foundation (Weeks 1-2)

#### Week 1: OMNeT++ & SDN Framework Setup
**Team:** Taran & Justin (Pair Programming)

**Tasks:**
1. Install OMNeT++ 7.0.0 (already present in repository)
2. Download and install INET Framework (version 4.5+)
   - Clone from GitHub: `https://github.com/inet-framework/inet`
   - Build INET: `make makefiles && make`
3. Install SDN framework (choose one):
   - **Option A:** OpenFlowOMNeTSuite
   - **Option B:** SDN4CoRE (if available for OMNeT++ 7.0)
4. Verify installation by running example simulations
5. Set up Git workflow and branching strategy
6. Create project directory structure

**Directory Structure:**
```
tj_omnet/
├── simulations/          # Simulation scenarios
│   ├── sdn_cloud/        # Our project simulations
│   ├── topologies/       # NED topology files
│   └── config/           # INI configuration files
├── dashboard/            # Web dashboard
│   ├── frontend/         # React/Vue app
│   ├── backend/          # Node.js/Flask API
│   └── tests/            # Dashboard tests
├── docs/                 # Documentation (this file)
│   ├── architecture/     # Design documents
│   ├── guides/           # User and developer guides
│   └── api/              # API documentation
├── src/                  # C++ controller code
│   └── sdn_controller/   # Custom controller modules
└── results/              # Simulation outputs
```

**Deliverables:**
- ✅ Functional OMNeT++ + INET + SDN environment
- ✅ Basic OpenFlow example running successfully
- ✅ `docs/SETUP_GUIDE.md` with installation instructions
- ✅ Git repository with proper `.gitignore`

**Success Criteria:**
- All team members can compile and run example SDN simulations
- Example topology shows up in OMNeT++ GUI
- OpenFlow messages visible in simulation log

---

#### Week 2: SDN Topology Design & Baseline

**Taran (Lead): Simulation Architecture**

**Tasks:**
1. Design multi-tenant cloud topology
   - 1 SDN controller
   - 3-5 OpenFlow switches in a mesh/tree topology
   - 10-15 hosts distributed across switches
   - Link capacities: 100Mbps-1Gbps
2. Create NED files for network components
   - `CloudTopology.ned`: Main network definition
   - `TenantHost.ned`: Host with traffic generation
   - `CloudSwitch.ned`: OpenFlow-enabled switch
3. Implement basic OpenFlow controller stub
   - Handle PACKET_IN messages
   - Install default forwarding rules
   - Log all OpenFlow messages
4. Test basic connectivity (ping between hosts)

**Example Topology Pseudocode:**
```ned
network CloudSDNNetwork
{
    submodules:
        controller: OpenFlowController;
        switch[5]: OpenFlowSwitch;
        host[15]: TenantHost;
    connections:
        controller.ethg++ <--> switch[0].ethg++;
        // ... more connections
}
```

**Justin (Lead): Dashboard Foundation**

**Tasks:**
1. Choose web framework (React recommended for component reusability)
2. Set up development environment
   - Initialize npm project
   - Configure build tools (Vite or Create React App)
   - Set up ESLint and Prettier
3. Create project structure
   ```
   dashboard/
   ├── frontend/
   │   ├── src/
   │   │   ├── components/    # UI components
   │   │   ├── services/      # API clients
   │   │   ├── store/         # State management
   │   │   └── views/         # Page components
   │   └── package.json
   └── backend/
       ├── routes/            # API endpoints
       ├── controllers/       # Business logic
       ├── models/            # Data models
       └── server.js
   ```
4. Design REST API structure
   - `/api/slices` - Slice management
   - `/api/flows` - Flow rule management
   - `/api/topology` - Network topology info
   - `/api/metrics` - Real-time statistics
5. Create placeholder dashboard with navigation

**API Design Document:**
```
POST   /api/slices           - Create new slice
GET    /api/slices           - List all slices
GET    /api/slices/:id       - Get slice details
PUT    /api/slices/:id       - Update slice config
DELETE /api/slices/:id       - Delete slice

POST   /api/flows            - Install flow rule
GET    /api/flows            - List all flow rules
DELETE /api/flows/:id        - Remove flow rule

GET    /api/topology         - Get network topology
GET    /api/metrics/:sliceId - Get slice metrics
```

**Deliverables:**
- ✅ Documented network topology with visual diagram
- ✅ Working OMNeT++ simulation with basic forwarding
- ✅ Web dashboard skeleton with routing
- ✅ API design document: `docs/api/REST_API.md`

**Success Criteria:**
- Simulation runs for 100+ simulated seconds
- Dashboard loads and displays placeholder UI
- API endpoints respond with mock data

---

### Phase 2: Core Feature Implementation (Weeks 3-5)

#### Week 3: Network Slicing - Simulation Side

**Taran (Lead), Justin (Support)**

**Objective:** Implement network slice creation, isolation, and configuration in OMNeT++ simulation.

**Tasks:**

1. **Define Slice Data Model**
   ```cpp
   class NetworkSlice {
       int sliceId;
       string tenantName;
       int vlanId;                    // VLAN tag for isolation
       double maxBandwidth;           // Mbps
       int priority;                  // QoS priority (0-7)
       vector<string> allowedHosts;   // Host IDs in this slice
       map<string, string> aclRules;  // Simple ACL rules
   };
   ```

2. **Extend OpenFlow Controller**
   - Add `SliceManager` module to controller
   - Implement slice CRUD operations:
     ```cpp
     int createSlice(SliceConfig config);
     bool deleteSlice(int sliceId);
     SliceConfig getSlice(int sliceId);
     bool updateSlice(int sliceId, SliceConfig config);
     ```
   - Store slice configurations in controller memory

3. **Implement Flow-Based Isolation**
   - When slice is created, install flow rules on all switches
   - Match packets by VLAN tag or source MAC address
   - Set queue/priority based on slice QoS parameters
   - Example flow rule:
     ```
     Match: vlan_id=100
     Actions: set_queue(1), forward(port 2)
     Priority: 100
     ```

4. **Traffic Generation per Slice**
   - Modify TenantHost to include slice ID
   - Tag outgoing packets with appropriate VLAN ID
   - Generate traffic with different profiles:
     - Slice 1: Constant 10Mbps UDP
     - Slice 2: Bursty HTTP traffic
     - Slice 3: Low-rate control traffic

5. **Slice Isolation Testing**
   - Create 3 slices with different bandwidth limits
   - Generate high load in Slice 1
   - Verify Slice 2 and 3 maintain their allocated bandwidth
   - Collect statistics: throughput, latency, packet loss per slice

**Configuration Example (omnetpp.ini):**
```ini
[Config NetworkSlicing]
network = CloudSDNNetwork

# Slice 1: High bandwidth
*.controller.sliceConfig[0].sliceId = 1
*.controller.sliceConfig[0].vlanId = 100
*.controller.sliceConfig[0].maxBandwidth = 500Mbps
*.controller.sliceConfig[0].priority = 7

# Slice 2: Medium bandwidth
*.controller.sliceConfig[1].sliceId = 2
*.controller.sliceConfig[1].vlanId = 200
*.controller.sliceConfig[1].maxBandwidth = 200Mbps
*.controller.sliceConfig[1].priority = 5
```

**Deliverables:**
- ✅ Functional network slicing in OMNeT++ controller
- ✅ Test scenarios: `simulations/sdn_cloud/slicing_test.ini`
- ✅ Verification: Slice isolation demonstrated with graphs
- ✅ Documentation: `docs/guides/NETWORK_SLICING.md`

**Success Criteria:**
- Create 3 slices successfully
- Traffic in different slices is isolated (bandwidth guarantees)
- Slice deletion removes all associated flow rules
- Controller logs show slice operations

---

#### Week 4: Network Slicing - Dashboard Integration

**Justin (Lead), Taran (Support)**

**Objective:** Build interactive dashboard for slice management with real-time visualization.

**Tasks:**

1. **Slice Management UI Components**
   - **SliceList Component:** Display all active slices in a table
     - Columns: Slice ID, Name, VLAN, Bandwidth, Priority, Status
     - Actions: View, Edit, Delete buttons
   - **SliceCreationForm Component:** Modal for creating new slice
     - Fields: Tenant name, VLAN ID, max bandwidth, priority, host selection
     - Validation: Check VLAN uniqueness, valid bandwidth range
   - **SliceDetailView Component:** Detailed slice information
     - Current traffic statistics
     - Associated hosts
     - Active flow rules
     - Configuration parameters

2. **Backend API Implementation**
   - Implement slice API endpoints in Node.js/Flask
   - Connect backend to OMNeT++ simulation via:
     - **Option A:** OMNeT++ SocketRTC scheduler (real-time)
     - **Option B:** Shared file/database (polling)
     - **Option C:** Custom TCP socket between controller and backend
   - Handle slice creation request:
     ```javascript
     app.post('/api/slices', async (req, res) => {
         const sliceConfig = req.body;
         // Send to OMNeT++ controller
         const result = await omnetController.createSlice(sliceConfig);
         res.json({ success: true, sliceId: result.id });
     });
     ```

3. **OMNeT++ External Interface**
   - Modify SDN controller to expose external API
   - Implement message handler for dashboard requests
   ```cpp
   void SDNController::handleExternalMessage(ExternalMessage *msg) {
       if (msg->getType() == CREATE_SLICE) {
           SliceConfig config = parseSliceConfig(msg->getPayload());
           int sliceId = sliceManager->createSlice(config);
           sendExternalResponse(sliceId);
       }
   }
   ```

4. **Topology Visualization**
   - Choose visualization library (Cytoscape.js recommended)
   - Fetch topology from backend: `GET /api/topology`
   - Display nodes (hosts, switches, controller) and links
   - Color-code nodes by slice membership
   - Show link utilization with different line thickness

5. **Real-Time Metrics Display**
   - Implement WebSocket connection for live updates
   - Display per-slice metrics:
     - Current throughput (Mbps)
     - Packet count
     - Average latency
     - Bandwidth utilization percentage
   - Update every 1-2 seconds
   - Use Chart.js or Recharts for line graphs

**Frontend Component Structure:**
```javascript
// SliceManager.jsx
import { useState, useEffect } from 'react';
import { getSlices, createSlice, deleteSlice } from '../services/api';

function SliceManager() {
    const [slices, setSlices] = useState([]);

    useEffect(() => {
        fetchSlices();
        const ws = new WebSocket('ws://localhost:8000/metrics');
        ws.onmessage = (event) => {
            updateSliceMetrics(JSON.parse(event.data));
        };
    }, []);

    const handleCreateSlice = async (config) => {
        await createSlice(config);
        fetchSlices();
    };

    return (
        <div>
            <SliceList slices={slices} onDelete={handleDeleteSlice} />
            <SliceCreationModal onCreate={handleCreateSlice} />
        </div>
    );
}
```

**Deliverables:**
- ✅ Interactive slice management dashboard
- ✅ Topology visualization showing slices
- ✅ Real-time metrics charts
- ✅ Working API integration with OMNeT++
- ✅ User guide: `docs/guides/DASHBOARD_USAGE.md`

**Success Criteria:**
- Create slice via dashboard → appears in OMNeT++ simulation
- Delete slice via dashboard → removed from simulation
- Real-time metrics update within 2 seconds
- Topology visualization correctly shows network structure

---

#### Week 5: Dynamic Flow Provisioning

**Taran & Justin (Collaborative)**

**Objective:** Implement on-demand flow rule installation with dashboard control.

**Taran Focus: Simulation-Side Implementation**

**Tasks:**

1. **Flow Rule Data Model**
   ```cpp
   class FlowRule {
       int flowId;
       int sliceId;              // Associated slice
       MatchFields match;        // Matching criteria
       Actions actions;          // Forwarding actions
       int priority;
       int idleTimeout;          // Seconds
       int hardTimeout;          // Seconds
       FlowStats stats;          // Packet/byte counters
   };

   struct MatchFields {
       string srcMAC, dstMAC;
       string srcIP, dstIP;
       int srcPort, dstPort;
       int vlanId;
       string protocol;          // TCP, UDP, ICMP
   };

   struct Actions {
       string outputPort;
       int setVlan;
       int setQueue;
       bool drop;
   };
   ```

2. **Flow Manager Module**
   - Add `FlowManager` to SDN controller
   - Implement flow CRUD operations:
     ```cpp
     int installFlow(FlowRule rule);
     bool removeFlow(int flowId);
     FlowRule getFlow(int flowId);
     vector<FlowRule> getFlowsBySlice(int sliceId);
     FlowStats getFlowStats(int flowId);
     ```
   - Maintain flow table with expiration handling

3. **Path Computation**
   - Implement shortest path algorithm (Dijkstra)
   - Given source and destination host, compute path through switches
   - Install flow rules on all switches in the path
   ```cpp
   vector<int> computePath(string srcHost, string dstHost) {
       // Return list of switch IDs in path
   }

   void installPathFlows(FlowRule rule, vector<int> path) {
       for (int switchId : path) {
           // Install rule on this switch with appropriate output port
       }
   }
   ```

4. **Flow Conflict Resolution**
   - Check for overlapping flows before installation
   - Use priority to resolve conflicts (higher priority wins)
   - Reject conflicting flows with error message

5. **Flow Expiration and Cleanup**
   - Implement idle timeout (remove flow if unused for N seconds)
   - Implement hard timeout (remove flow after N seconds regardless)
   - Send flow removal notifications to dashboard

**Justin Focus: Dashboard Implementation**

**Tasks:**

1. **Flow Provisioning UI**
   - **FlowCreationWizard Component:** Multi-step form
     - Step 1: Select source and destination (host picker)
     - Step 2: Define match criteria (IP, port, protocol)
     - Step 3: Specify actions (forward, drop, set VLAN)
     - Step 4: Set priority and timeouts
     - Step 5: Review and submit
   - **FlowList Component:** Table of active flows
     - Columns: Flow ID, Slice, Match, Actions, Priority, Stats
     - Filters: By slice, by switch, by host
     - Actions: View details, Modify, Delete

2. **Flow Table Viewer**
   - **SwitchFlowTable Component:** Per-switch flow table
     - Select switch from dropdown
     - Display all flows on that switch
     - Highlight flows by slice (color-coded)
   - **FlowInspector Component:** Detailed flow information
     - Match fields visualization
     - Actions breakdown
     - Real-time statistics (packets, bytes, duration)
     - Update frequency: 1 second

3. **Flow Visualization**
   - Overlay flows on topology visualization
   - Show active flow paths as animated arrows
   - Display flow bandwidth usage on links
   - Click on flow to see details

4. **Flow API Implementation**
   ```javascript
   // API service
   export async function installFlow(flowConfig) {
       const response = await fetch('/api/flows', {
           method: 'POST',
           headers: { 'Content-Type': 'application/json' },
           body: JSON.stringify(flowConfig)
       });
       return response.json();
   }

   export async function getFlows(sliceId = null) {
       const url = sliceId
           ? `/api/flows?slice=${sliceId}`
           : '/api/flows';
       const response = await fetch(url);
       return response.json();
   }
   ```

5. **Error Handling and Validation**
   - Validate flow configuration before submission
   - Display error messages from controller (e.g., conflicting flows)
   - Show success notifications
   - Implement retry logic for failed installations

**Integration Testing**

**Both:** Test end-to-end flow provisioning
1. Create slice via dashboard
2. Select two hosts in that slice
3. Install flow rule between them
4. Generate traffic and verify forwarding
5. Monitor flow statistics in dashboard
6. Delete flow and verify removal

**Test Scenarios:**
- Install 10 flows across 3 slices
- Verify slice isolation is maintained
- Test flow expiration (idle and hard timeout)
- Rapid flow creation/deletion (stress test)
- Conflicting flow handling

**Deliverables:**
- ✅ Dynamic flow provisioning working end-to-end
- ✅ Flow table visualization in dashboard
- ✅ Path computation and multi-hop flow installation
- ✅ Flow statistics monitoring
- ✅ Documentation: `docs/guides/FLOW_PROVISIONING.md`

**Success Criteria:**
- Install flow via dashboard → traffic flows as specified
- Flow statistics update in real-time
- Flow expiration works correctly
- Dashboard shows all active flows accurately
- Can install 50+ flows without performance issues

---

### Phase 3: Integration & Testing (Week 6)

**Taran & Justin (Collaborative)**

**Objective:** End-to-end system testing, performance evaluation, and bug fixing.

#### Comprehensive Testing

**1. Functional Testing**
- **Slice Management:**
  - Create 5 slices with various configurations
  - Update slice parameters (bandwidth, priority)
  - Delete slices and verify cleanup
  - Test VLAN uniqueness validation
- **Flow Provisioning:**
  - Install 20 flows across different slices
  - Test all match field combinations
  - Test all action types (forward, drop, set VLAN)
  - Verify flow priority handling
- **Dashboard:**
  - Test all UI interactions
  - Verify real-time metric updates
  - Test error handling and edge cases
  - Cross-browser testing (Chrome, Firefox, Safari)

**2. Performance Testing**
- **Simulation Performance:**
  - Measure simulation time for different scenarios
  - Profile CPU and memory usage
  - Test with increasing number of flows (10, 50, 100)
  - Test with increasing traffic load
- **Dashboard Performance:**
  - Measure API response times
  - Test WebSocket update latency
  - Monitor frontend rendering performance
  - Test with 100+ flows displayed

**3. Stress Testing**
- Create maximum slices (e.g., 10)
- Install maximum flows (e.g., 200)
- Generate high traffic volume (1000 packets/sec)
- Rapid CRUD operations (create/delete loops)
- Long-running simulation (1000 simulated seconds)

**4. Integration Testing**
- **Dashboard ↔ Backend ↔ OMNeT++:**
  - Test full request/response cycle
  - Verify data consistency
  - Test concurrent operations
  - Test connection recovery after failures

**Test Cases Document:**
```markdown
# Test Case 1: Slice Isolation
- Create Slice A (500 Mbps limit)
- Create Slice B (200 Mbps limit)
- Generate 800 Mbps traffic in Slice A
- Verify Slice B maintains 200 Mbps throughput
- Expected: Slice A capped at 500 Mbps, Slice B unaffected

# Test Case 2: Flow Conflict Resolution
- Install Flow 1: Match IP 10.0.0.1, Priority 100
- Install Flow 2: Match IP 10.0.0.1, Priority 50 (conflict)
- Expected: Flow 2 rejected with error

# Test Case 3: Flow Expiration
- Install flow with 30s idle timeout
- Generate traffic for 20s
- Stop traffic for 40s
- Expected: Flow automatically removed after 30s idle

# ... more test cases
```

**Performance Benchmarks:**
| Metric | Target | Measured | Status |
|--------|--------|----------|--------|
| API Response Time | <100ms | TBD | ⏳ |
| Dashboard Update Latency | <2s | TBD | ⏳ |
| Max Concurrent Flows | 100+ | TBD | ⏳ |
| Simulation Real-Time Factor | >10x | TBD | ⏳ |

**Bug Tracking:**
- Use GitHub Issues or simple `docs/BUGS.md`
- Priority levels: Critical, High, Medium, Low
- Track: Bug description, steps to reproduce, assigned to, status

**Deliverables:**
- ✅ Comprehensive test suite
- ✅ Performance benchmark report: `docs/PERFORMANCE.md`
- ✅ Bug tracking document
- ✅ All critical and high-priority bugs fixed

**Success Criteria:**
- All functional tests pass
- Performance meets targets
- Zero critical bugs remaining
- System stable for 10-minute demo run

---

### Phase 4: Refinement & Documentation (Weeks 7-8)

#### Week 7: Feature Polish & Usability

**Justin (Lead): Dashboard Enhancement**

**Tasks:**
1. **UI/UX Improvements**
   - Apply consistent styling (color scheme, typography)
   - Implement responsive design (works on different screen sizes)
   - Add loading indicators and progress bars
   - Improve form validation messages
   - Add tooltips and help icons

2. **User Feedback and Error Handling**
   - Success notifications (toast messages)
   - Error alerts with actionable messages
   - Confirmation dialogs for destructive actions
   - Input validation with clear error messages

3. **Dashboard Help System**
   - Add "Getting Started" tutorial overlay
   - Tooltip explanations for all fields
   - Embedded help panel with FAQ
   - Demo mode with sample data (no OMNeT++ required)

4. **Accessibility**
   - Keyboard navigation support
   - ARIA labels for screen readers
   - High contrast mode option
   - Accessible color palette

**Taran (Lead): Simulation Refinement**

**Tasks:**
1. **Performance Optimization**
   - Profile simulation and identify bottlenecks
   - Optimize flow table lookups (use hash maps)
   - Reduce unnecessary logging
   - Tune simulation parameters for faster execution

2. **Logging and Statistics**
   - Add detailed logging for debugging:
     - Flow installations/removals
     - Slice operations
     - Packet forwarding decisions
   - Collect comprehensive statistics:
     - Per-slice throughput, latency, packet loss
     - Per-flow byte/packet counters
     - Switch processing times
   - Export statistics to CSV/JSON for analysis

3. **Configuration Presets**
   - Create example scenarios in INI files:
     - `basic_slicing.ini`: Simple 2-slice setup
     - `stress_test.ini`: Maximum flows and traffic
     - `demo.ini`: Visual demo with moderate load
   - Document each scenario in comments

4. **Result Export and Visualization**
   - Implement result exporter module
   - Generate plots using Python scripts:
     - Throughput vs. time graphs
     - Slice isolation comparison
     - Flow distribution histograms
   - Create `scripts/analyze_results.py`

**Deliverables:**
- ✅ Polished, professional-looking dashboard
- ✅ Optimized simulation with comprehensive logging
- ✅ Example scenario library (5+ configurations)
- ✅ Result analysis scripts

**Success Criteria:**
- Dashboard looks professional and intuitive
- Simulation runs 2x faster than Week 6 version
- Demo scenarios run without errors
- Results can be exported and analyzed easily

---

#### Week 8: Final Documentation & Presentation

**Taran & Justin (Collaborative)**

**Objective:** Complete all documentation, create demo materials, and prepare final presentation.

**Documentation Tasks:**

1. **Architecture Document** (`docs/architecture/SYSTEM_DESIGN.md`)
   - System overview diagram
   - Component descriptions
   - Data flow diagrams
   - Technology stack rationale
   - Design decisions and tradeoffs

2. **User Manual** (`docs/guides/USER_MANUAL.md`)
   - Installation instructions (step-by-step)
   - Dashboard walkthrough with screenshots
   - Common use cases with examples:
     - How to create a network slice
     - How to install a flow rule
     - How to monitor performance
   - Troubleshooting guide
   - FAQ section

3. **Developer Guide** (`docs/guides/DEVELOPER_GUIDE.md`)
   - Code structure overview
   - How to extend the controller
   - How to add new dashboard features
   - API reference with examples
   - Contribution guidelines

4. **API Documentation** (`docs/api/API_REFERENCE.md`)
   - Complete REST API specification
   - Request/response examples
   - Error codes and messages
   - WebSocket event documentation
   - Code examples in multiple languages

5. **Project Report** (`docs/PROJECT_REPORT.md`)
   - **Introduction:** Problem statement, motivation
   - **Related Work:** Comparison with Mininet, SDN4CoRE
   - **System Design:** Architecture, implementation details
   - **Evaluation:** Performance results, test outcomes
   - **Lessons Learned:** Challenges, solutions
   - **Future Work:** Possible extensions
   - **Conclusion:** Summary of achievements
   - **References:** Citations to papers and tools

**Demo Materials:**

1. **Demo Video** (5-10 minutes)
   - Script outline:
     - 0:00-1:00: Introduction and problem statement
     - 1:00-2:00: System architecture overview
     - 2:00-4:00: Live demo - Network slicing
     - 4:00-6:00: Live demo - Flow provisioning
     - 6:00-7:00: Performance results
     - 7:00-8:00: Future work and conclusion
   - Screen recording with voiceover
   - Edit with transitions and captions
   - Tools: OBS Studio (recording), DaVinci Resolve (editing)

2. **Presentation Slides** (15-20 slides)
   - Title slide with team names
   - Problem statement (1-2 slides)
   - Related work comparison (1 slide)
   - System architecture (2-3 slides)
   - Implementation highlights (3-4 slides)
   - Demo screenshots (3-4 slides)
   - Performance evaluation (2-3 slides)
   - Future work (1 slide)
   - Conclusion and Q&A (1 slide)
   - Tools: Google Slides or PowerPoint

3. **README.md** (Project root)
   - Project title and description
   - Key features (bullet points)
   - Quick start guide (< 5 steps)
   - Screenshots
   - Links to detailed documentation
   - Team information and acknowledgments

**Final Testing and Preparation:**
- Do complete run-through of demo scenario
- Test installation on fresh machine
- Verify all documentation links work
- Proofread all documents
- Practice presentation (timing and flow)

**Deliverables:**
- ✅ Complete documentation suite (6 documents)
- ✅ Demo video (uploaded to YouTube/Vimeo)
- ✅ Presentation slides (PDF and editable format)
- ✅ Updated README.md with project overview
- ✅ Code comments and inline documentation

**Success Criteria:**
- All documentation is clear and comprehensive
- Demo video successfully showcases key features
- Presentation fits within time limit (15 minutes)
- External person can install and run the system using docs
- Code is well-commented and maintainable

---

## Project Timeline (Gantt Chart)

```
Week 1-2: Environment Setup & Foundation
Week 1  [████████████████████████████████] Setup & Installation
Week 2  [██████████ Taran: Topology ████████] [██████ Justin: Dashboard ██████]

Week 3-5: Core Feature Implementation
Week 3  [████████████ Taran: Network Slicing ████████████████]
Week 4  [████████████ Justin: Dashboard Integration ██████████]
Week 5  [██████████ Taran: Flow Logic ████] [██████ Justin: Flow UI ████████]

Week 6: Integration & Testing
Week 6  [████████████████ Joint Testing & Bug Fixing ████████████████]

Week 7-8: Refinement & Documentation
Week 7  [████ Taran: Optimization ████] [████ Justin: UI Polish ████]
Week 8  [████████████████ Joint Documentation & Presentation ████]
```

---

## Risk Management

### Identified Risks and Mitigation Strategies

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| **SDN framework compatibility issues** | Medium | High | Start with well-documented examples; Budget extra time in Week 1; Have backup framework option |
| **Real-time dashboard sync complexity** | Medium | Medium | Implement polling fallback; Use proven WebSocket libraries; Test with small topologies first |
| **Time constraints** | High | High | Prioritize core features; Maintain feature backlog; Cut non-essential features if needed; Weekly progress reviews |
| **Debugging distributed system** | Medium | Medium | Extensive logging at all layers; Use modular testing; Clear API contracts; Version control checkpoints |
| **Performance bottlenecks** | Low | Medium | Profile early (Week 5); Optimize in Week 7; Set realistic scale expectations |
| **Team coordination issues** | Low | Medium | Daily standups (15 min); Use project management tool (Trello/GitHub Projects); Clear task ownership |
| **Unclear requirements** | Low | High | Document assumptions; Get instructor feedback early; Focus on core abstractions |

### Contingency Plans

**If SDN framework proves too complex:**
- Fall back to simpler OpenFlow library
- Reduce topology complexity
- Focus on one core feature (slicing OR flows)

**If dashboard integration fails:**
- Use OMNeT++ GUI with enhanced visualization
- Export data to dashboard post-simulation (not real-time)
- Demonstrate core concepts without live dashboard

**If behind schedule after Week 4:**
- Reduce number of test scenarios
- Simplify dashboard UI (functional over aesthetic)
- Focus on one complete feature rather than two partial ones

---

## Work Distribution

### Taran Agarwal - Primary Responsibilities
- OMNeT++ simulation setup and configuration
- Network topology design and implementation
- SDN controller logic (slicing and flow management)
- OpenFlow message handling
- Performance optimization
- Result analysis and statistics collection

### Justin Bravo - Primary Responsibilities
- Web dashboard architecture and development
- Backend API implementation
- Frontend UI components
- Real-time data visualization
- API integration with OMNeT++
- User experience design

### Joint Responsibilities
- Week 1-2: Pair programming on setup
- Week 6: Integration testing and debugging
- Week 8: Documentation and presentation
- Weekly: Code reviews and progress meetings

---

## Evaluation Criteria

### Technical Achievement (50%)
- ✅ Network slicing fully functional (20%)
- ✅ Dynamic flow provisioning working (20%)
- ✅ Dashboard successfully integrated with simulation (10%)

### Innovation & Complexity (20%)
- ✅ Novel approach to SDN visualization
- ✅ Clean architecture and code quality
- ✅ Going beyond basic requirements

### Documentation & Presentation (20%)
- ✅ Clear and comprehensive documentation
- ✅ Effective demo video
- ✅ Professional presentation

### Testing & Validation (10%)
- ✅ Thorough testing with multiple scenarios
- ✅ Performance benchmarks
- ✅ Bug-free demo

---

## Resources

### Required Software
- OMNeT++ 7.0.0: https://omnetpp.org/
- INET Framework 4.5+: https://inet.omnetpp.org/
- OpenFlowOMNeTSuite: https://github.com/lsinfo3/OpenFlowOMNeTSuite
- Node.js 18+ or Python 3.9+
- React 18+ or Vue 3
- VS Code or IntelliJ IDEA

### Hardware Requirements
- CPU: 4+ cores recommended
- RAM: 8GB minimum, 16GB recommended
- Storage: 10GB free space
- OS: Linux, macOS, or Windows with WSL

### Learning Resources
- OMNeT++ Documentation: https://doc.omnetpp.org/
- INET User Guide: https://inet.omnetpp.org/docs/users-guide/
- OpenFlow Specification 1.3: https://opennetworking.org/
- React Documentation: https://react.dev/
- SDN Tutorials: https://github.com/mininet/openflow-tutorial

### Reference Papers
1. SDN4CoRE: A Simulation Model for Software-Defined Networking for Real-Time Ethernet in OMNeT++ (OMNeT++ Summit 2019)
2. Performance and Security Evaluation of SDN Networks in OMNeT++/INET (Tiloca et al., 2016)
3. A Comparative Review: Accurate OpenFlow Simulation Tools for Prototyping (2014)

---

## Appendices

### Appendix A: Glossary

- **SDN (Software-Defined Networking):** Network architecture that separates control plane from data plane
- **OpenFlow:** Protocol for SDN controller to communicate with switches
- **Network Slice:** Isolated virtual network on shared infrastructure
- **Flow Rule:** Packet matching and forwarding instruction
- **VLAN:** Virtual LAN for network segmentation
- **QoS:** Quality of Service parameters (bandwidth, priority)

### Appendix B: Code Conventions

**C++ (OMNeT++):**
- Class names: PascalCase (e.g., `SDNController`)
- Function names: camelCase (e.g., `installFlow`)
- Constants: UPPER_SNAKE_CASE
- Comments: Doxygen style

**JavaScript/TypeScript (Dashboard):**
- Component names: PascalCase (e.g., `SliceManager`)
- Function names: camelCase (e.g., `createSlice`)
- Constants: UPPER_SNAKE_CASE
- Follow Airbnb style guide

### Appendix C: Git Workflow

**Branches:**
- `main`: Stable releases only
- `develop`: Integration branch
- `feature/<name>`: Feature branches (e.g., `feature/network-slicing`)
- `bugfix/<name>`: Bug fix branches

**Commit Messages:**
- Format: `<type>: <description>`
- Types: feat, fix, docs, style, refactor, test
- Example: `feat: implement slice creation in controller`

---

## Contact and Support

**Team Members:**
- Taran Agarwal: [email]
- Justin Bravo: [email]

**Instructor:** [Name]
**Office Hours:** [Times]

**Project Repository:** https://github.com/[username]/tj_omnet
**Issue Tracker:** https://github.com/[username]/tj_omnet/issues

---

**Document Version:** 1.0
**Last Updated:** [Date]
**Authors:** Taran Agarwal, Justin Bravo

---

*This technical plan is a living document and will be updated as the project progresses.*
