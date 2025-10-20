# Cloud-Based SDN Management Dashboard Simulation

<div align="center">

![OMNeT++](https://img.shields.io/badge/OMNeT++-7.0.0-blue)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
![License](https://img.shields.io/badge/License-Academic-green)

**An interactive dashboard for managing Software-Defined Network slicing and dynamic flow provisioning in simulated cloud environments**

[Features](#key-features) • [Quick Start](#quick-start) • [Documentation](#documentation) • [Demo](#demo) • [Team](#team)

</div>

---

## 📋 Overview

This project bridges the gap between SDN theory and hands-on experimentation by creating a visual, user-friendly dashboard for managing two critical SDN abstractions:

1. **Network Slicing** - Create isolated virtual networks for multi-tenant cloud scenarios
2. **Dynamic Flow Provisioning** - Install and manage OpenFlow rules on-demand between endpoints

Built on OMNeT++ discrete-event simulation framework with INET and OpenFlow extensions, this tool enables students and researchers to experiment with SDN concepts in a controlled, reproducible environment.

### Why This Project?

Existing SDN tools like **Mininet** focus on network emulation with limited experiment visualization, while **OMNeT++ SDN frameworks** (SDN4CoRE, OpenFlowOMNeTSuite) provide protocol simulation but lack interactive dashboards. Our contribution is a **modular, experiment-focused dashboard** that makes SDN abstractions accessible and visual.

---

## ✨ Key Features

### 🔹 Network Slicing
- **Create/Delete/Configure** network slices via web interface
- **VLAN-based or flow-rule isolation** for multi-tenant scenarios
- **Per-slice QoS configuration** (bandwidth limits, priority levels)
- **Real-time metrics** showing throughput, latency, and packet counts per slice
- **Visual topology** with slice-aware color coding

### 🔹 Dynamic Flow Provisioning
- **Point-and-click flow rule installation** between hosts
- **Flexible match criteria** (IP, MAC, port, protocol, VLAN)
- **Multiple actions** (forward, drop, set queue, modify VLAN)
- **Flow table inspection** with real-time statistics
- **Automatic path computation** for multi-hop flows
- **Flow expiration** with idle and hard timeouts

### 🔹 Interactive Dashboard
- **Web-based UI** accessible from any browser
- **Live topology visualization** using Cytoscape.js/D3.js
- **Real-time statistics** updated via WebSockets
- **Intuitive forms** with validation and error handling
- **Demo mode** with sample data (no simulation required)

### 🔹 Simulation Features
- **Multi-tenant cloud topology** (configurable switches and hosts)
- **OpenFlow 1.3+ protocol** support
- **Comprehensive logging** and statistics collection
- **Export results** to CSV/JSON for analysis
- **Multiple test scenarios** included

---

## 🚀 Quick Start

### Prerequisites
- **OMNeT++ 7.0.0** installed ([Installation Guide](docs/guides/SETUP_GUIDE.md))
- **INET Framework 4.5+**
- **OpenFlowOMNeTSuite** or **SDN4CoRE**
- **Node.js 18+** or **Python 3.9+** (for dashboard backend)
- **Modern web browser** (Chrome, Firefox, Safari)

### Installation (5 Steps)

```bash
# 1. Clone the repository
git clone https://github.com/[username]/tj_omnet.git
cd tj_omnet

# 2. Build OMNeT++ simulation
cd simulations/sdn_cloud
make makefiles
make

# 3. Set up dashboard backend
cd ../../dashboard/backend
npm install  # or: pip install -r requirements.txt

# 4. Set up dashboard frontend
cd ../frontend
npm install

# 5. Run the demo
# Terminal 1: Start OMNeT++ simulation
cd simulations/sdn_cloud
./sdn_cloud -u Cmdenv -c Demo

# Terminal 2: Start backend
cd dashboard/backend
npm start  # or: python server.py

# Terminal 3: Start frontend
cd dashboard/frontend
npm run dev
```

### Access Dashboard
Open browser to **http://localhost:3000**

---

## 📸 Screenshots

### Network Slicing Dashboard
*[Screenshot placeholder: Shows slice list, creation form, and topology view]*

### Flow Provisioning Interface
*[Screenshot placeholder: Shows flow wizard, flow table, and path visualization]*

### Real-Time Metrics
*[Screenshot placeholder: Shows throughput graphs and statistics panels]*

---

## 🎓 Use Cases

### Educational
- **Teach SDN concepts** with visual, hands-on examples
- **Demonstrate network slicing** for multi-tenancy
- **Explore OpenFlow** message flows and switch behavior
- **Compare isolation techniques** (VLAN vs. flow-based)

### Research
- **Test slicing algorithms** in controlled environment
- **Evaluate QoS mechanisms** with reproducible scenarios
- **Prototype new controller logic** without physical hardware
- **Generate performance data** for papers and presentations

### Development
- **Debug SDN applications** before hardware deployment
- **Experiment with flow rules** safely
- **Validate network policies** in simulation
- **Create custom test scenarios** easily

---

## 📚 Documentation

### User Guides
- **[Complete Technical Plan](docs/TECHNICAL_PLAN.md)** - Detailed project plan with weekly breakdown
- **[Setup Guide](docs/guides/SETUP_GUIDE.md)** - Step-by-step installation instructions
- **[User Manual](docs/guides/USER_MANUAL.md)** - Dashboard usage with examples
- **[Network Slicing Guide](docs/guides/NETWORK_SLICING.md)** - How slicing works
- **[Flow Provisioning Guide](docs/guides/FLOW_PROVISIONING.md)** - Flow rule management

### Developer Guides
- **[System Architecture](docs/architecture/SYSTEM_DESIGN.md)** - Component overview and data flow
- **[Developer Guide](docs/guides/DEVELOPER_GUIDE.md)** - Code structure and extension points
- **[API Reference](docs/api/API_REFERENCE.md)** - REST API and WebSocket events

### Project Documentation
- **[Weekly Progress Tracking](docs/WEEKLY_TRACKING.md)** - Development status
- **[Performance Benchmarks](docs/PERFORMANCE.md)** - Test results and metrics
- **[Project Report](docs/PROJECT_REPORT.md)** - Complete project summary

---

## 🎥 Demo

**[Watch Demo Video](https://youtube.com/[video-link])**

5-minute demonstration showing:
- Creating network slices with different QoS settings
- Installing dynamic flow rules between hosts
- Monitoring real-time metrics and traffic statistics
- Demonstrating slice isolation under load

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│           Web Dashboard (React/Vue)             │
│   Slice Manager | Flow Manager | Topology View  │
└────────────────────┬────────────────────────────┘
                     │ REST API / WebSockets
┌────────────────────▼────────────────────────────┐
│      Backend API (Node.js/Flask)                │
│     Routing | Event Handling | State Mgmt      │
└────────────────────┬────────────────────────────┘
                     │ JSON-RPC / TCP Socket
┌────────────────────▼────────────────────────────┐
│         OMNeT++ Simulation Environment          │
│  ┌──────────────────────────────────────────┐   │
│  │  SDN Controller (C++)                    │   │
│  │    Slice Manager | Flow Manager          │   │
│  └──────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────┐   │
│  │  OpenFlow Switches (INET/SDN4CoRE)       │   │
│  └──────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────┐   │
│  │  Hosts & Traffic Generators              │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

---

## 🧪 Testing

### Test Scenarios Included
1. **Basic Slicing** - 2 slices with different bandwidth limits
2. **Slice Isolation** - Verify traffic separation under load
3. **Dynamic Flows** - Install 10+ flows across multiple slices
4. **Flow Expiration** - Test idle and hard timeout mechanisms
5. **Stress Test** - Maximum slices and flows with high traffic
6. **Demo Scenario** - Visual demonstration setup

### Running Tests
```bash
# Run all test scenarios
cd simulations/sdn_cloud
./run_tests.sh

# Run specific test
./sdn_cloud -u Cmdenv -c SliceIsolationTest

# Analyze results
cd ../../scripts
python analyze_results.py ../results/
```

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Simulation** | OMNeT++ 7.0 | Discrete-event network simulation |
| **Network Models** | INET Framework | Protocol implementations (TCP/IP, Ethernet) |
| **SDN Protocol** | OpenFlowOMNeTSuite | OpenFlow 1.3 switch and controller |
| **Controller Logic** | C++ | Network slicing and flow management |
| **Backend API** | Node.js (Express) or Python (Flask) | REST API and WebSocket server |
| **Frontend** | React or Vue 3 | Interactive web interface |
| **Visualization** | D3.js / Cytoscape.js | Network topology rendering |
| **Real-time Updates** | WebSockets (Socket.io) | Live metrics streaming |

---

## 📈 Performance

| Metric | Target | Achieved |
|--------|--------|----------|
| API Response Time | <100ms | ⏳ TBD |
| Dashboard Update Latency | <2s | ⏳ TBD |
| Max Concurrent Flows | 100+ | ⏳ TBD |
| Max Slices | 10+ | ⏳ TBD |
| Simulation Real-Time Factor | >10x | ⏳ TBD |

*Benchmarks will be updated as development progresses.*

---

## 🗺️ Roadmap

### ✅ Phase 1: Foundation (Weeks 1-2)
- [x] OMNeT++ + INET + SDN framework setup
- [x] Basic topology design
- [x] Dashboard skeleton

### 🚧 Phase 2: Core Features (Weeks 3-5)
- [ ] Network slicing implementation
- [ ] Dashboard integration
- [ ] Dynamic flow provisioning

### 📋 Phase 3: Testing (Week 6)
- [ ] End-to-end testing
- [ ] Performance benchmarks
- [ ] Bug fixes

### 🎨 Phase 4: Polish (Weeks 7-8)
- [ ] UI/UX improvements
- [ ] Documentation completion
- [ ] Demo video and presentation

---

## 🤝 Contributing

This is an academic project for coursework. While external contributions are not expected, we welcome:
- **Bug reports** via GitHub Issues
- **Feature suggestions** for future work
- **Documentation improvements**

### Development Workflow
1. Fork the repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m 'feat: add your feature'`
4. Push to branch: `git push origin feature/your-feature`
5. Open Pull Request

---

## 👥 Team

### Taran Agarwal
**Role:** Simulation & Controller Development
- OMNeT++ architecture and topology design
- SDN controller implementation (slicing and flows)
- Performance optimization and testing

### Justin Bravo
**Role:** Dashboard & Integration Development
- Web dashboard frontend and backend
- API design and implementation
- UI/UX design and visualization

**Institution:** [Your University]
**Course:** Cloud Computing
**Instructor:** [Instructor Name]
**Semester:** [Semester/Year]

---

## 📝 License

This project is developed for academic purposes as part of a college course. Code is provided as-is for educational reference.

---

## 🙏 Acknowledgments

- **OMNeT++ Community** for excellent documentation and support
- **INET Framework** developers for comprehensive network models
- **OpenFlowOMNeTSuite** team for SDN simulation capabilities
- **Academic Papers** cited in our research (see [Technical Plan](docs/TECHNICAL_PLAN.md))

---

## 📧 Contact

**Project Repository:** https://github.com/[username]/tj_omnet
**Issue Tracker:** https://github.com/[username]/tj_omnet/issues

**Team Email:** [your-email@university.edu]

---

## 🔗 Related Work

- **Mininet** - Network emulator with basic GUI ([mininet.org](http://mininet.org))
- **SDN4CoRE** - Real-time SDN simulation ([core4inet.net](https://core4inet.net))
- **OpenFlowOMNeTSuite** - OpenFlow protocol for OMNeT++ ([GitHub](https://github.com/lsinfo3/OpenFlowOMNeTSuite))

**How We Differ:**
Our project focuses specifically on interactive, experiment-driven dashboards for SDN abstractions (slicing and flows), while related tools emphasize protocol accuracy or hardware emulation.

---

<div align="center">

**⭐ Star this repo if you find it useful!**

Made with ☕ and OMNeT++ by Taran & Justin

[Back to Top](#cloud-based-sdn-management-dashboard-simulation)

</div>
